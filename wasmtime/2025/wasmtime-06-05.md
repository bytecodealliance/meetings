# June 05 | Wasmtime Project Bi-Weekly

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
   1. Internal crates and external usage -- https://github.com/rustsec/advisory-db/pull/1999 -- @alexcrichton
   2. Binary artifact compatibility - https://github.com/bytecodealliance/wasmtime/pull/10876 -- @alexcrichton
   3. Compile-time builtins RFC -- https://github.com/bytecodealliance/rfcs/pull/43 -- @fitzgen
   4. Using page maps for fun and profit: a new way to recycle pooling allocator instance slots -- @tschneidereit
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)

## Attendees

* Pat Hickey
* Chris Fallin
* Nick Fitzgerald
* Erik Rose
* Paul Osborne
* Alex Crichton
* Till Schneidereit
* Joel Dice
* Andrew Brown
* Johnnie Birch
* Khagan Karimov

## Notes

### Internal crates with external usage

Alex: There's a rustsec advisory that we had some wasmtime-jit-debug crate
with a signature thats blatantly unsound. the bug was reported and fixed a
long time ago. But people wanted there to be a rustsec advisory posted about
it, which is fine.

Alex: We have a lot of these private crates that are wasmtime-*, with no
safety or stability guarantees provided by our project. But in the crates io
world there are people who would want to treat those public apis from those
crates and file rustsec advisories about them if they are unsound.

Chris: Could we name the crates wasmtime-internal-* ?

Nick: And then import it with a lib name in cargo.toml so we dont have to
change our source code?

Alex: I'll file an issue describing this: we'll change the crate names to have
internal in their names, add stern verbiage somewhere, and then if someone
opens a rustsec advisory we can say we made our best effort to describe how
that was not a public api, and we will fix whatever is reported in the
advisories.


### Binary artifacts compatability

Alex: CI has been getting really flaky. A lot of it is because apt get
installs are failing in docker images for really old operating systems,
because we want to build against the oldest glibc possible. I dont know
the root cause of why the apt gets are failing. We dont currently have a
policy for how old we build binary artifacts against. THe current ubuntu
lts we build against is way beyond its open source EOL and approaching its
support contract EOL.  But, even recent (current LTS) ubuntus are having
this flake occasionally so it wouldnt fully solve the problem.

Chris: Can we support statically linked artifacts so the build os doesnt
matter?

Alex: Maybe, but I dont know if it covers every target

Joel: Can we use an image with the apt-get update/install already completed
published on ghcr.io?

Andrew: Agreed with joel, it does create extra steps for maintence of the
images but it seems like its worth it now. And it may save CI time.

Alex: I agree but the sticking point is that theres some manual steps, I never
figured out how to reliably use an image thats built if the dockerfile is not
changed in CI. right now if i need to update the image i just make an ordinary
PR to the dockerfile and thats it. So, the real problem here is that I dont
know how to orchestrate this in CI.

Joel: I was thinking theres a separate project whose only job is to publish
via CI these images...

Alex: that ends up being a pain to coordinate. But its also a benefit, we
often have to do this with wasm-tools as well...

Nick: Which repo it lives in is an orthogonal question.

Pat: maybe a rule on a CI job where it only runs if the dockerfile changes?

Alex: Maybe something along those lines can work out.

Andrew: how many jobs require these images?

Alex: just the release builds.

Alex: I will file an issue with those conclusions.

Alex: If we decide we are stickign with glibc, which i think we should, whats
our policy there. The oldest LTS that you dont have to pay for?

Nick: Has anyone ever nagged us about this? (No.) Do we need any formalities?

Alex: I just want to document what the policy is, not add process. The fact
that nobody has complained about it is an artifact of that I went as old as I
could possibly get. But thats now too old.

Alex: In conclusion theres two independent things. Making CI not always hit an
apt server, and our glibc policy. I think we have identified reasonable paths
on both.

### Compile time builtins RFC

Nick: I made [this RFC](https://github.com/bytecodealliance/rfcs/pull/43), its similar to the JS Strings Builtins proposal which
is for the web embedding. This is corresponding for wasmtime (which is not
standardized like the web is). But the idea is, if we are linking against
something at compile time, can we inline it into the compiled wasm rather than
make function calls. Our motivation is in specific for zero-copy manipulation
of host buffers - make import functions to get and set bytes in host buffers,
and have those import function calls turn directly into loads and stores.

Nick: Suggestions were to expose the cranelift function builder. Thats not
ideal to expose all of cranelift through wasmtime. Another mini-language? give
a mouse a cookie, the mini-language is always going to grow. Self-hosted wasm?
Allow the host to provide wasm implementations of imports at compile time,
effectively erasing the imports. I'm leaning towards this last one. In the
fullness of time we could allow arbitrary wasm, modulo questions about what
additional imports look like for compile-time builtins. Initially thinking: it
cant describe any state (memories tables globals), just pure functions. So
instead of going through the vmctx builtin table, it makes a direct call to
the compile time import. Or we could even start with always inlining it.

Nick: Suggest we make a cranelift inlining library. We dont want to build this
concept right into cranelift. Provide a hook as the cranelift embedder where
it can ask whether or not to inline a function call - None dont inline, or
Some please inline this body. Wasmtime will use this in its compiler and the
hueristics for inlining are completely controlled by the cranelift embedder
(wasmtime in this case). THis generalizes to the fused adapters we have for
the component model - it often doesnt make sense to do inlining inside a core
module because llvm and wasm-opt have taken care of those already. but when
linking different modules in the component model, an optimizer has never seen
across those module boundaries yet, so this is the first opportunity. So this
approach will give us both the compile-time builtins we want, and the fusion
we want for components, "for free" small asterik.

Nick: The broad strokes of this are all in my RFC. Its not fully detailed out
yet.

Andrew: Both the intrinsics need to be inlined into the builtin, and then the
builtin gets inlined into the module using it?

Nick: the intrinsics are for doing native loads and stores. Chris pointed out
we already give you the power to do this today: you can define a wasmtime Func
that does unsafe loads to whatever pointer. So we just want to avoid a
function call to do that. We'll recognize the calls in the wasm translation,
and just do the associated body.

Andrew: you're talking about two kinds if inliners, why cant it be unified
as one single kind of inliner?

Nick: we dont want to let modules just use these intrinsics directly - that
breaks the safety guarantee, we want modules to still be importing e.g. from
wasi. So we're just allowing the implementation of the calls to these
intrinsics to be directly inlined. Right now the implementation of wasi will
just do some potentially unsafe things, but we control the implementation and
make sure its safe.

Andrew: Cant one inliner be powerful enough to do all of this? THeres two
levels of inliners here.

Nick: well they can define a function and then func.ref it, so even if we
inline it most of the time we'll still need a standalone version for the func
ref to point to. So we cant always just inline everything

Chris: Its not clear what the function we'd inline would look like, for the
intrinsics - theres no wasm instruction for reading host memory. so we need
some level of something special here

Nick: You can write a clif function that takes a pointer and does a load, the
inlining happens at the clif level

Chris; so raw wasm extension where the function body is defined in raw clif?

Nick: The intrinsics are defined with clif, the compile time builtins are
defined in wasm which calls the intrinsics. both gets inlined but theyre two
different sorts of definitions.

Nick: to address winch because Saul isnt here today: If you care about this
sort of performance youre already using cranelift. its supposed to be a
semantically transparent optimization, missing the optimization is fine, so
if youre using winch you wont get the optimization and thats going to be ok.

Till: it would also work in pulley, and in pulley it could be inlined.

Nick: yes everything should just work in pulley without any work on our part,
because pulley is just cranelift.

Till: I think this is a great solution, because this addresses some concerns
we have about component composition scenarios, if we dont make those fast then
people will be incentivsed to create bigger components without security
boundaries, and also its annoying if you cant implement host interfaces that
are both fast and fine-grained. the more we can make this available to
wasmtime embedders who implement of arbitrary wit interfaces, the more
valuable it will be. How can we make that easy?

Nick: we will definitely enable that, making it super easy is another thing?

Till: ok, making it easy can be out of scope for now, as long as we arent
saying that its only for wasmtime to use and not restricted from use by every
wasmtime embedder.

Nick: do we need to separate the concept of a core compile-time builtin and a
component compile-time builtin? Not sure yet.

Nick: Please comment on [the
RFC](https://github.com/bytecodealliance/rfcs/pull/43)


### Page table manipulation for fun and profit

Till: I was muddling in Linux ioctls and found a relatively new one called
page map scan, Alex and I collaborated on getting into wasmtime. Here is an
intermediate
[result](https://github.com/bytecodealliance/wasmtime/compare/main...tschneidereit:wasmtime:pagemap_scan)

Till: The ioctl allows us to efficiently find the dirty pages in cow map
regions. So now we get structs containing regions of offsets which are dirty.
So those are a mix of pages that need to be reset to zero, or need to be
restored to the original content of the cow map. Right now we just
madvise_dontneed or (forget the exact mechanism for cow images). with this
patch we iterate and either memset(0) them, or memcpy() them, so it completely
eliminates TLB shootdowns and the associated IPIs, and it also reduces minor
page faults, there are no major page faults now.

Nick: No major page faults iff you use the keep resident and dont go over
that. Do you find thats common?

Till: (notetaker missed some) 16kib memcpy is what resetting the instance is
down to. the keep resident setting doesnt know how much it needs to keep
resident, up to the limit or the heap size. now we keep resident exactly what
is needed and no more. so now keep resident 2MB would be a very bad decision
for a tiny component so it would cost a 2MB memcpy even if 16kib is all thats
needed. In general we expect many components will be less costly. I did a
bunch of benchmarking, it boils down that resetting components, syscalls for
each memory and table. this page map scan is almost a perfect syscall, i do
wish we could pass in vecs of ranges so that we could amortize a few more
syscalls, but its still a big win in general. it scales pretty linearly with
the number of cores, we can throw more cores at an engine without causing
issues.

Till: I buried the lede, on a test on intel i9 pinned to 8 perf cores 16
threads, and on my macbook pro in linux vm, in both settings with same number
of cores I get about 250 to 300% requests per second on an unloaded node. the
guest under tests were many different rust and one javascript components,
doing lightweight work. most relevant the less compute you do in the
component, but still very relevant in javascript. I assume if you had a loaded
node with memory bandwidth consuming neighbors. this makes wasmtime a much
less noisy neighbor and almost a normal web server.

Alex: The tlb shootdowns still happen if you resize memory. so this is most
beneficial if your component doesnt grow memory, which is on the table for a
wizened component.

Till: this syscall is almost like it was created for us. Its now used by one
of the userspace live migration across nodes, not qemu... the motivation was
to implement something related to windows that may have been an anti-cheat
check for games but that motivation doesnt help much.

Paul: if the ioctl takes the vmas youre actually interested in that seems like
what we need, doing the whole process would be quite expensive and not what we
want, as long as we can limit it thats a good design

Alex: another nice thing is you configure the maximum size that we want the
information about, effectively telling the kernel our keep resident
configuration, so it doesnt keep iterating in the kernel for more information
than we actually need.

Till: maybe this was a deliberate decision but it was unexpected that the keep
resident flag for memories and tables are multiplied by the numbers of
memories and tables? so a module which has 2 memories gets 2MB limit times 2
memories.

Nick: the way I have is that we have some limits per-entity, and then some
limits on the number of entities. So that creates the upper bound, you can
derive what that limit implicitly is, but we dont do limits in a way that
budgets.

Alex: the historical rationale is that the keep resident stuff predates the
limits on the number of tables and memories. if we could change the limits to
be budget based i think that would make more sense.

Till: I'll propose we change to budget limits as part of this, it will be much
more useful that way. I started working on that. Get the overall budget, get a
reference to the return buffer for the syscalls so we keep that around, then
every time we reset anything subtract it from the budget, then stop doing it
once budget exhausted. that makes a lot more sense to me than the current
structure. To me it seems like it doesnt make a difference where we spend th
ebudget - a page fault on the table entry or a page fault on a memory
location, seems like it doesnt make a differnce, so if thats wrong please tell
me if one of those makes sense to spend budget on first

Nick: it would depend on the probability of the thing being accessed we'd want
to avoid the page faults on the higher probability items more

Till: the way I described would make it so we approach using more things, I
wasnt thinking we should keep track of how many pages are resident in a given
table or resident, so we want to actually keep track of that so after many
resets of different patterns we dont increase the total residency. assuming
the component over time eventaully touches the whole thing.

Nick: it would be easy to write a malicious module that does the worst case
behavior

Till: so i will keep a dirty set and not just the page count, and if it grows
too much trigger an madvise of the whole thing

Chris: many/most/maybe all of the table writes compiled by llvm which arent
using table.set, so it will just be the lazy initialization touching those
table values. so itll be the same value every time...

Nick: vmctx is malloced and freed, even under pooling allocator, so that could
change every time. different free lists for tables and for memories, so its
not the same instance getting the same table each time.

Alex: for imports that same host import is the same vm funcref every time.

Alex: the page map scan also tells if a file is still backed by cow. so if its
still mapped read only to a file or a zero we just skip that.

Till: this is like our old friend userfaultfd, but it only uses the page table
flags that userfaultfd has, so it doesnt have all of those terrible
performance problems. and its since the last time i told you to reset flags.
and its page table not vma operations so its quite cheap. cost of an additonal
syscall after resetting. So you need to reset the pages and then send a
syscall to remove the not-modified flag.

Nick: (a clever idea notetaker didnt catch that till agrees should wait until
after this is complete)

Till: I'll get a PR up for this as soon as I can.

