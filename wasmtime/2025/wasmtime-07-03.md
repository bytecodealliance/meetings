# July 03 | Wasmtime Project Bi-Weekly

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
   1. Upstreaming wasip3-prototyping (@alexcrichton)
   1. Upstreaming WASIp3 implementations (@alexcrichton, @dicej)
   1. Improving soundness of Wasmtime's implementation over time (@alexcrichton)
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)

## Attendees

* Till Schneidereit
* Alex Crichton
* Joel Dice
* Dan Gohman
* Paul Osborne
* Chris Fallin
* Yosh Wuyts
* Nick Fitzgerald
* Andrew Brown
* Erik Rose
* Arjun Ramesh

## Notes

### Upstreaming WASIp3 prototyping.

Alex: Heads up: We’re in the process of upstreaming wasip3-prototyping into Wasmime. PSA: This is happening! If you have concerns, please bring them up.

The work is split into 4 PRs for the async impl in Wasmtime; these PRs are open in the Wasmtime repo right now.

I’m doing profiling measuring the perf difference for things like entering and exiting Wasm. I’m seeing 20% slowdowns. I’m planning to do some work to reduce that a little bit, but in general, we haven’t done a lot of profiling yet.

Joel: I’m setting up a harness for high-level benchmarking, FWIW

Nick: there is also https://github.com/bytecodealliance/wasmtime/blob/main/benches/wasmtime-serve-rps.sh fwiw

would be interesting to see wasi-http p2 vs p3 in wasmtime serve and their respective RPS

Joel: Thanks, I’ll take a look.  And yes, old p2 vs. new p2 vs. p3 wasi-http RPS is a top priority to compare.

Till: Ideally we shouldn’t have regressions for people doing something that people normally do in wasip2. Joel, Alex, and I agree that apples-to-apples comparisons should not be bad; this should not be a reason to avoid adopting wasip3.	

Nick: Are you doing any benchmarking that’s less than full applications? FOr example benchmarking streaming in data using poll in wasip2 vs stream in wasip3?

Alex: Haven’t done too much yet. The main concern I have right now is to make sure if you don’t use wasip3 that you won’t be negatively impacted. But it’s a good point, I’ll check out some benchmarking. That said, this work is not done. We’re aiming for landing with no apples-to-apples primitives, and then iterating in the tree to optimize the new features like streams.

Nick: I agree, optimizing new features doesn’t need to happen before landing, but should be before we say wasip3 is done.

Alex: Agreed.

Alex: The fifth step, after all the async PRs, is to land the WASIp3 implementation. This is using the p3 prototypes in the various WASI repos. It’s still using some of the big implementation pieces like cap-std and tokio, but all the glue code is new. We want it to land upstream in some form. The current rough idea is that on one hand we need to start by working with the WASI SG to make the wasip3 wit interfaces more official and have some kind of release.

The code is organized in groups of functionality, like networking, clocks, etc. and land the groups and their tests one group at a time. Probably HTTP will go last. FIlesystems raises the question of whether we should share the impl with wasip2; we don’t have a plan or a timeline for these things yet.

Pat: When we landed wasip2, we didn’t initially have the p1 using the p2 impl; Roman added that later. It’s definitely good to combine the impls for maintainability, so we should do that, but it doesn’t need to block landing.

Alex: We won’t be able to remove the p2 interfaces, but we can change the impls to be based on the p3 implementation over time.

### BA sponsering a WASI testsuite

Andrew: Another dimension to this: BA-sponsored compliance testing...
BA sponsoring a testsuite

Till: This hasn’t been announced yet: The BA has agreed to sponsor Andy Wingo of Igalia to build a compliance test suite for WASI, starting with WASI-HTTP, focused on wast tests, so that it’s not dependent on any guest language toolchain. A key part will be to figure out how to orchestrate the tests with servers and clients. The goal is not to just add things to Wasmtime’s test coverage, but to have it be the start of something that can become an official WASI testsuite.

Let us know if you want to join the kickoff call.

### Unsafe code in Wasmtime

Alex: I think we should consider all unsafe code in Wasmtime as technical debt that we need to pay down. We’ve come to use it somewhat in a somewhat cavalier way, which has complicated Joel’s work on fibers. It’s been difficult to take this existing system which has all this unsafe and extend it.

I’ve been doing a lot of refactoring to make things more safe. We used to hand out &mut for instances, which is fundamentally unsound, but we just passed it around and crossed our fingers. So the fix is to use Pin, which is less ergonomic, but it sound. Another example is libcalls, where we simultaneously had a mutable store and a mutable instance, which I’ve now cleaned up. But we have a lot more like this to clean up.
Miri distinguishes between sound, which is to say correct for all possible ways of using the code, and safe, meaning no bad things happen in the code as it’s currently written.

So I want to push back on the overall culture of unsafe. We need a lot of unsafe things, to do fibers, JITing, memory mapping, and so on, but we should encapsulate these and avoid just using unsafe indiscriminately.

Chris: Why are we using unsafe today?

Alex: Ownership is one of the reasons. Especially with the async stuff, we often used raw pointers instead of having mutable borrows. But also, we also inherited things like the libcalls and VMContext and instances and so on in Wasmtime, and we’ve gone through a lot of iterations. Another example is getting memories and tables from an instance is all raw pointers right now.

Nick: It’s often the combination of ownership with FFI. We’re talking to JIT code where we need raw things, and then we need to get back into our abstractions.

No one has pushed Rust this hard in this way.

Yosh: From a Rust language perspective I would also be interested in some of the patterns that are particularly difficult to encode

Joel: Niko’s view types proposal could help with ownership-related things.

Nick: The thing to focus on is the transition from unsafe APIs to safe APIs, and document what the invariants are. And also scoping down and encapsulating the unsafe.

Alex: We currently disable the lint for unsafe-block-in-unsafe-fn; we should enable it, and get to a place where a call to an unsafe can be understood by local reasoning alone.

Joel: The goal is that people should be able to contribute non-trivial new features to Wasmtime without doing any unsafe stuff. To some degree unsafe is always needed, but we should reduce the scope.

Joel: fiber.rs in the async PRs is a good example. It does encapsulation effectively and can be understood with local reasoning.

Nick: An example of a feature of code that used unsafe but should have needed to is core dumps. It had to iterate over memories and tables, which required it to be unsafe. But we should have safe APIs for things like iterating over basic data structures like this.

Alex: I’d like to get agreement that any time we add unsafe, we commit to removing it, though I don’t have a concrete idea yet.

Till: We have policies like, if you want to implement a feature in Wasmtime, you have to fuzz it. That’s harder to do for unsafe code, but we should do what we can, so that it’s not a matter of individual contributors begging their management for the time to do this, but just a requirement in order to land features. Let’s all take this back to our management chains, so that it’s built into resourcing considerations.

Yosh: Yes. As you go through refactorings in Wasmitime, and notice things that could be improved in the language, please bring them back to the Rust language team. If there are things, like Pin ergonomics or VIew types, please feed that info back. I’m happy to help. If you find an interesting pattern that leads to a less than ideal solution, please write it down!
