## August 08, 2023 - SIG Guest Languages meeting

|          |      | 
| -------- | -------- |
| Attending  | Robin Brown, Calvin Prewitt, Jiaxiao (Joe) Zhou, Joel Dice, Damian Gryski, Brett Cannon
| Note Taker | Robin Brown

## Updates

* Robin: [Test Suite](https://hackmd.io/qeVkBYYWQ1C1uiKYk-HzZw)
* Joe: Submitted a PR to create a new subgroup for Go and have been gathering interest.

## Agenda

* Accepted Go Subgroup by consensus vote
* Threads
    * Overview
        * Browser-based initial version involves making new instances (and new function tables) for each thread.
        * WASI-threads followed that approach for non-browser use cases.
        * We'll want to do something better before it gets baked into everything for essentially forever.
        * Core Wasm is getting shared memories and will add functions/globals/heap-types/tables.
        * We'll either get core wasm spawn instruction or a special function that tells the host to make you threads.
        * This can all happen internally to a Component without changes to the component model with the requirement that Component functions by default can only be called from a single thread and in a multi-threaded component that will be the "main" thread
        * This "main" thread restriction naturally comes from the requirement that shared things can only call/use shared things and that if you lower a non-shared Component import to core wasm you get a non-shared import.
        * Component Model level shared will come later.
    * Go
        * Achille: Go is working on a wasm32 architecture but because of go's built-in concurrency model with go routines all of this gets emulated (similar to asyncify). A lot of consequences came from that including that Go doesn't really use the Wasm call stack.
        * Achille: As threads become available, it may be used to make this new architecture and simplify things.
        * Luke: stack switching may be relevant because it lets you make stacks and though its single-threaded it may make things simpler. Browsers are adding this promise integration to do things at the boundaries. There's some debates about algebraic effects vs. make context...
        * Luke: the hard work is being done in browsers and once that arrives, there's room for threads to be more like os threads or lightweight. We may not need to make kernel threads and do pooling. It could be more like a go routine especially if the wasm runtime is doing the scheduling. Components trying to figure out the size of hte machine and how big a pool to make may not work well when they all make a bunch of their own threads.
        * Achille: It would be nice if the guests didn't need to worry about this and it was taken care of instead of them trying to figure out system limits. Even if the spec says things should be done this way, hosts may create a new host process in share memory at the kernel level. From go's perspective, who cares it'd be better to let the host deal with it.
        * Randy: If code that is running within a thread must be shared and it can only call shared exports from other modules does that imply that all of WASI CLI and that whole scope would need to be shared. For example, sockets the whole thing would need to be shared? Does that color everything else up-hill?
        * Luke: Each WASI world needs to opt-in to having a shared variant. Once you have epoll-like functionality, you can get a lot of concurrency with only one thread doing io.
        * Randy: I was thinking about the Go consideration of spawning a few hundred thousand Go routines that each do networking.
        * Luke: ...in some contexts, you want the concurrency be good but you do the massive concurrency one layer up. Serverless is not the only place using Wasm, but that's one of the major use cases.
        * Randy: Getting large long-running services with expensie 
    * Timeline
        * Achille: do you have a sense of what the timeline would be?
        * Luke: there's an in-person CG meeting in October that will be talking about the new thread spawn proposal (which introduces shared). It's not actually a ton of work to implement, so once a direction is agreed on the best thing to do is to prototype experimental implementation. Andrew Brown has done some of this work before and he can probably give advice for where to get started.
        * Luke: I could imagine that we agree on how to spell the bits and there's a prototype implementaiton within a year and it'll take even longer than that to get fully stable and in the browser. Browsers have a lot of requirements, but out of the browser we could have an experimental implementation pretty quick.
        * Achille: would it be useful to provide material for the CG e.g. here's how go would like to use this and how it would unlock
        * Luke: it would, and that could be useful even at a remote CG meeting
        * Achille: Will the in-person meeting be a bigger decision-making moment
        * Luke: it will probably have a vote for moving the proposal to the next phase. Andrew Brown and Conrad Watts would know what would be useful and you should also be able to remote in (it will be in Munich). If there's not enough time a follow-up biweekly would be a good place to bring that up.
        * Damian: If we need eventually to change how go internally handles threading and stuff because there's new threading proposal stuff we can do that and we aren't stuck with one hacked up asyncify implementation forever.
        * Randy: I have a question for Damian or Achille about what Luke said about threads spawning something more like a go-routine than a process. If it did, could we imagine go delegating scheduling to the host instead of managing it itself.
        * Damian: The question is going tob e about what semantics we need and what semantics they provide and whether we can implement one with the other. I think GCC go uses posix threads for their go routine implementation. It would suck to have something really good in wasm but be unable to use it directly because it doesn't have something specific. On the go side we want to have a scheduler that works with the io system. We want them to go to sleep when the io waits. Obviously that's on a separate level from threading but go routines do that today and if we can't do that with the new system or its a big performance drop that would be less useful.
        * Randy: do you think there would be use in having discussions between go folks and the prior art in go
        * Luke: there's a fork in the road over whether thread spawn is more like a system thread or a lightweight thread like a go routine. we should definitely use go and aim to have a production-strength language be able to implement its tools with it.
        * Robin: in the long-run... async/streams/futures
        * Luke: in the preview 3 timeline, that should definitely be true and if we do this right we should be able to get high-performing concurrent/parallel execution without making all these interfaces shared by pipelining well
        * Damian: Go currently supports WASI and changing the way these things works for thereads might be a lot of work. tiny-go might be a good playground to experiment with these things before pushing it into "big go"
        * Luke: there's a standards process and then a prototyping process which Andrew Brown has started if you wanted you could help build a prototype
* Exceptions
    * Luke: Exceptions has progressed pretty far along the proposal process, but recently some of the implementation has produced feedback that things should be changed while we still can.
    * Luke: ... something about the exception stack and rethrowing, references, etc.
    * Luke: It's going to be a bit longer till that one "re-stabilizes" after changing its design. I think Wasmtime either has started or was about to start, but it will want to jump to the new version.
    * Brett: What's a long time? (for stabilization)
    * Luke: I think that'll be a few months and then get in browsers by the end of the year. The Wasmtime question is more about resourcing and who is going to be working on it, which I don't know.
    * Achille: go generally doesn't use exceptions but does use it in the panic system which has a panic/defer/recover mechanism that allows you to handle e.g. a null dereference. in go, this is where excpetion handling would be useful
    * Damian: tiny-go didn't have defer/panic for a long time and now we have it for all platforms except wasm
    * Brett: MicroPython also uses setjmp for exception handling
* [Joel] - Preview 2
    * Joel: We're in the design process for threads and some of this stuff, but we're much further along for preview 2 which has pseudo threads/futures and we're working on bindings and things to map futures to promises for js, and for python/rust mapping it to async/await. For go, I'm curious if anyone has thoughts on how that would work with the current asyncify approach and preview 2 io.
    * Achille: I've done a bunch of experiments on this in the past few months in this area. Integration with the runtime has proven to be complex. If things aren't file descriptors, it's hard to not block on things. We need to be careful to not block on things if we aren't using language primitives. It's proven useful to integrate as much as possible with the standard library. With things that are sockets, can they be represented by the NetConn interface.
    * Randy: To follow up on something he said, when applying wasi http to roundtripper with the go tooling there's some discussoin on what level it should export and the balance between performance and being idiomatic. There's probably going to be few consumers of generated APIs but lots of consumers of things talking to those generated APIs. On the spectrum of idiomatic to performant, you probably want something as simple as possible. If wasi preview 2 is specified in wit, then the language could use the bindgen to ease implementation.
    * Luke: In this preview 2 time frame, you're definitely not going to want to use this directly and instead wrap the poll and other operations in an interface users use while using wit-bindgen as much as possible

## Action Item

* Robin: Merge PR for Go Subgroup and help start first meeting
