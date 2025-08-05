# July 31 | Wasmtime Project Bi-Weekly

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
   1. Note: meeting notes linked in the invite.
   1. Please help add your name to the meeting notes.
   1. Please help take notes.
   1. Thanks!
1. Announcements
   1. _Submit a PR to add your announcement here_
1. Other agenda items
   1. Instance reuse for p3 components
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)

## Attendees

Pat Hickey
Nick Fitzgerald
Alex Crichton
Joel Dice
David Justice
Johnnie Birch
Dan Gohman

## Notes

### wasmtime serve and defaults regarding instance reuse

Till: For wasip3, I propose making wasmtime serve default to concurrent instance reuse.

If you have an async handler export, you shouldn't need something special to opt into
concurrent. There isn't even a way to do it.

Right now, wasmtime serve doesn't make use of any of this.

With the switch to p3, change that to make full use of the full extent of reuse we have available.

Dan: What if we applications have state?

Alex: We say "don't do this". And we can configure the maximum number of requests per instance.

Nick: And we can make it somewhat random.

Joel: Guests can't know?

Till: Guests might do batching that could make thees assumptions.

Pat: I think of Wasmtime CLI as being the simplest possible case for an embedding. You're
not trying to connect it to bigger things. So what if P3 is, I will reuse this for the
lifetime of the process. Rather than trying to be clever. Just leave it to custom embeddings
to figure out these policies.

Nick: I agree that makes sense from a wasmtime-as-library point of view, but looking at the
ecosystem, people will be doing development locally. The randomization is there to prevent
people from making these assumptions.

Till: I agree. Wasmtime serve does set performance expectations. We've seen users evaluate
wasmtime and wasi-http based on wasmtime serve performance, so we want to make sure the
default configuration is fast, including effective instance reuse.

Wasmtime serve should provide as good a testing environment as possible. Performance and
so on should reflect real-world usage as much as possible. I do my testing for Starling Monkey
with Wasmtime serve and could not do that in p3 under Pat's proposal.

Joel: We've got this notion of a start function in core wasm and components, having an end
function or a cleanup function, could be an export which components could chose to provide,
and if it's there could be called by the host, perhaps not guaranteed to be called, which
would address the telemetry use case.

CM async functions can and often do continue running after they return a value. And they can
keep running concurrently with other calls, so it wouldn't work if

Pat: Do we have a concept on a store of waiting until all tasks or done?

Joel: I'm not sure. But we could do that pretty easily. The call_concurrent function would
return a future that resolves to result and another future, and you could collect those
futures and wait on all of them.

Pat: If we drop that collection, would that cancel any futures running in there?

Joel: We haven't finished that implementation yet. A guest can cancel subtasks, but we
haven't exposed that to the host API yet. It's a TODO.

Till: Joel was mentioning lifecycle hooks. In the p3 timeframe, we don't have anything. Post-p3,
I think we should add an exported cleanup hook, meaning "call this before sending me a new
request". That might be a convenient place for a runtime to do GC. Without such a hook, doing a
GC after each request would be wasteful if there are few requests per instance, and doing a

Nick: The teardown hook makes sense to me, instances could use that to send of telemetry and
so on. But the GC part doesn't make as much sense. But GC is often more expensive than
reinstantiating.

Till: Perhaps don't think of it as a GC thing. Just a way to reset state that's cheaper to
reset than to create a whole new instance.

Then again, if you're called hundreds of times, then it makes sense to do it.

This takes work off the critical path of the request handler, and it allows the host to schedule
when the work happens.

On the other hand, another approach would be to give futures "nice" levels and allow low-priority
futures that could defer to other tasks whenever there's other work to do.

Nick: Priorities sound tricky without source-language participation.

Till: Ok, getting back to the present discussion.

Joel: Embedders can always kill things. The missing piece right now is a way for hosts to
gracefully shut down instances.

Till: We're also missing a way for hosts to determine whether any current instance is
ready to handle a request.

Joel: We have a canonical builtin to indicate backpressure. It would be pretty trivial to expose
this to the embedder.

There is a way to join all pending tasks in the current API. We have `instance_run_concurrent`.
It's just knowing which task completed when that would need a new API. You git it an async
`FnOnce` which will produce

Alex: Is the idea we have a pool of instances? That dynamically shrinks or grows?

Till: Yes, we should have knobs configuring min/max pool size.

Nick: So there's a pool of instances, and then a set of which ones are ready?

Till: Yes

Alex: This is a little non-trivial in the wasmtime serve command, but doable.
I'd like to avoid a big "is it P2 or P3", and instance keep them as unified as much
as possible. To maximize the amount of code shared between the two. Perhaps a P2 thing
just sets the knobs to max of a single instance.

Joel: There are a lot of p2 components in the wild not expecting to be reused. TinyGo with the
leak collector, StarlingMonkey apparently doesn't support reuse, and so on.

Till: We cannot change this for p2 without an active signal from a component. StarlingMonkey
just crashes if you send it a second request. A fix for this never landed because that would
be even worse, because it wouldn't fail loudly, but it still might not be prepared for reuse.
Switching to p3 is a major undertaking, and we should use this point in time to change the
default to reuse.

Pat: Agree. No reuse for p2.

Joel: We could add an opt-in flag for reuse with p2.

Alex: Yeah, we could just do this by setting defaults for p2 to no reuse.

Pat: I suspect trying for code reuse is going to be awkward and more work than its worth.

Alex: I'd like to try for code reuse between p2 and p3, but if it's too much, then it's too much.

### P3 status update

Alex: All the CM async PR have landed in the wasmtime crate. wasmtime-wasi is ported except for
filesystem and http. We are discovering issues, doing benchmarking, working on internals. I'm
currently working on filesystem, http will come next. Once we have those, wasmtime run/serve
integration will come. By mid-augest we may have everything landed. Http is going to be a big
thing, so that may take a little longer.

### RFC for compile-time builtins

Nick: RFC is basically done, just needs the motion to finalize etc. Take a look and I'll start the
process to merge next week. Give feedback! I've landed an inliner in Cranelift which can inline in
cross-component calls.

Alex: The inliner is off by default right now?

Nick: Yes.

Nick: In the future, something we might want to inline is cross-module calls, assuming that
intra-module calls are already well optimized. Another we might want to inline is calls in
programs that use GC, because they don't have LLVM and might not be as optimized.

In the future, I might propose to enable it by default.

Till: I could imagine wanting to do toolchain sniffing. While the LLVM assumption holds up
pretty well, it's a different matter with Big Go and other producers.
