# July 17 | Wasmtime Project Bi-Weekly

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
   1. Making Wasmtime safer: the case for `Store<T: Send>` (@alexcrichton)
   1. Component Record and Replay support (@arjunr2)
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)


## Attendees

Chris Fallin
Pat Hickey
Nick Fitzgerald
Paul Osborne
Alex Crichton
Victor Adossi
Joel Dice
Andrew Brown
Yan Chen
David Justice
Johnnie Birch
Arjun Ramesh

## Notes

### Making Wasmtime safer: the case for Store<T: Send> (@alexcrichton)

Alex: We recently added T:'static because of CM Async stuff, now I have need for adding T: Send as well.

Alex: Shows example in shared_memory.rs fn grow

Alex: Shows example in fiber.rs, really wrong comment about "this is surely
the most dangerous". The argument there boils down to auditing all of wasmtime
to ensure there are only Send variables on the stack.

Alex: There are a lot of sync functions in wasmtime that are a lie: they are
actually async functions. This started off when we added async to sync in
wasmtime, then we added all these extra features - async resource limiters,
async gc, async call hooks. If you grow async we allocate an entire fiber to
run the async fn that could have been run on the host stack.

Alex: shows vm/memory.rs unsafe fn grow. This is a fundamentally unsound
signature, there are two mutable borrows to unsafe datastructures here. Its
technically ok right now but we want to make it sound.

Alex: so what if we just add async everywhere. There are a few problems that
prevent that: unsafe trait vm::VMStore, which has fn memory_growing - what if
we made that async fn? We need Store's data to be send, because then we will
be closing over the store data pointer in the Future. In libcalls, heres the
memory grow libcall, heres a suspension point as well if we made these async.
In instance/allocator/on_demand.rs unsafe fn allocate_memory, it shows up here
too.

Alex: So I want to add the Send bound. This is a disavantage for people not
doing Send things - theres core wasm things, theres Actix which is
fundamentally a non-Send rust async web framework which had a user recently
come to us.

Alex: we want to make the internals of wasmtime async in a lot more places,
but for the sync interface we will just poll once and it will be Ready because
we arent actually introducing any points where the future will be Pending.

Andrew Brown: What about shared-everything threads, would we have needed Send
for that?

Alex: Unclear, but I expect the answer there is so different that its not
applicable. In a lot of those situations we want Sync as well, and we dont
know whether we want all those threads to have their own Store or to have a
shared Store.

Alex: I do want to push hard about not having a Sync constraint. Thats a big
issue that would prevent cell/refcell. We have a mutable reference to the data
everywhere so we dont need a mutex to turn Send things into Send + Sync, we
can do that without needing a lock and its safe Rust.

Alex, Pat: we dont want to make duplicate async interfaces that have Send and
dont have Send. Its just too much work to maintain it.

Alex: This is getting into RFC territory so I can write up all of this
reasoning for future reference.

Nick: We're going to push async out to the callers of these traits? Does that
have a performance impact?

Alex: Yes I need to measure it, we will have boxed futures in places.

Nick: If its bad enough we can do a homegrown placement new.

Alex: we know Rust is working on solutions to that in the 5 year timescale...

Nick: This will mean that large portions of the runtime will be rewritten
into state machines, like the asyncify overhead it will mess with control flow
but be much better than asyncify in practice.

Alex: In places we have destructors - for example we use closures now to
try to allocate memories and tables and then destroy them if anything fails,
we will have to reify that as destructors instead in case the future gets
dropped.

Alex: Async call hooks make this complicated

Pat: Theres only one prod user of those I know of, can we work with them
to prune anything back there? Paul, can you talk to Aaron T about those?

Alex: we could continue doing the the fiber technique we use today for
async call hooks. The resource limiters are the part that seems the trickiest
in my current exploration.

Nick: what makes call hooks further?

Alex: not sure, I didnt bottom them out everywhere, but my sense is that its
in core places doing a lot of things... I should explore more

Nick: so you're going to continue investigating, and then give us an update
or post an RFC.

Alex: I need to just go and make everything async at once, and then do some
performance analysis, and then with that consider an RFC.

### Component Record and Replay support (@arjunr2)

Arjun: The motivation is to enable recording import function calls and
deterministically replay them back. A group at Fastly had a notion of a text
format of a trace that could be inspected. At F5 we want to be able to use it
in production. I want to hear from all of the use cases embedders see for this
-- do you want to keep recording always on in production?

Arjun: Our use case is that if you encounter a bug in production, you can
capture a trace all the time and replay the ones just where your bug occured.
The replay can be totally separate from the capture environment, and
potentially with a different engine that has better support for debug. You can
generate tests by subset the trace to specific functions and make a unit test
for a specific wasi call. It could also be useful as an interposition
mechanism to profile different parts of the engine. You could potentially fuzz
events as well to explore all of the host side functionality possible.

Arjun: so we want a low-overhead way to save events to memory, we want that to
be able to dump to the disk or writer. This will enable time-travel debugging.

Paul: if you have a record of many different runs you can then replay it
with profiling to get pgo data, without the overhead of profiling in
production.

Chris: offline analysis in general is a great use case, so if you want to for
example write a tool for taint analysis that may not be efficient enough to
run in production but with the trace you can separate it out to run offline

Nick: you can do any potential analysis, if you save traces you can write any
sort of analysis pass later that you hadnt thought of when you captured the
trace.

Arjun: what if you want to test small changes in modules, for example if you
were to tweak internals that wouldnt change the control flow of events? It seems
hard to validate that, requires the same memory layout and interleavings of
memory accesses, you cant change the host side of things.

Nick: We talked that through and the idea is really cool but the details are
pretty vauge and it seems like more of a research project at this stage rather
than building a tool for wasmtime and its users right now.

Arjun: yes youd need either a lot of guarantees from your compiler or else
youd need some symbolic trace

Chris: depends at the level of your trace, when you record the result of every
single cabi_realloc call and then writing the bytes directly to a memory, that
gets a lot more particular than if you record values at the component model
level and allow the replay to end up with different behavior of cabi_realloc.

Nick: Seems like a lot of work for a neat but niche use case, compared to the
main use case of time travel debugging.

Yan: we have a prototype at the wit-bindgen level that doesnt rely on wasmtime
at all, and you can record with wit-bindgen a trace and then synthesize a mock
component that you compose using wac.

Arjun: I did a very initial benchmark yesterday evening, it looks something
like a 4-5% overhead to interpose on the events and record them to an
in-memory rust serialized format, when you dont include writing to disk. I
dont know how much better that will get.

Nick: What sort of test case?

Arjun: I benchmarked a component that does compression with zstd. I generated
a million file directory and compressed it. So the import functions are file
opens and reads.

Nick: In the trace are you recording every single write? Every lift and lower
context get?

Arjun: I am not coalescing events, its just the bare bones every time a lower
context slice of memory is dropped we record the contents

Nick, Chris: there may be potential to improve things there.

Arjun gives a walkthrough of the implementation in his Wasmtime fork.

Nick: I think this is at a point now where its ready for a PR where we start
looking at the code.

Arjun: I dont want this to get upstreamed if there is overhead to run without
recording. Data is inconclusive right now, will spend more time benchmarking.
