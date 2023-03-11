# January 26th SIG-Debugging Meeting

**See the [instructions](../README.md) for details on how to attend**

## Agenda

#### Goal
Eventually come up with an RFC for Wasmtime’s long-term debugging story

#### Use cases
* Live debugging the whole system, both native host and Wasm guests
  * “I want to step through this Wasm program’s execution and step into its interaction with nitty-gritty host/Wasmtime implementation details.”
* Live debugging just guests, without exposing any native host data
  * “I want to step through my Wasm program’s execution to diagnose a bug; the host is black box that is written/maintained by someone else.”
  * “I want to allow guests to debug their programs without exposing my host implementation details.”
* Live debugging programs running inside a Wasm interpreter
  * “I want to debug my JavaScript program that is running inside a JavaScript engine that was compiled to Wasm.”
* Post-mortem debugging
  * “My Wasm program hit a bug in production and I want to figure out why after the fact”
* Record and replay
  * “I want to record a (potentially nondeterministic) bug in my Wasm program and (deterministically) replay it many times to figure out what went wrong.”
  
* Kind of two dimensions:
  * Debuggee: { host-and-guest, only-guest, interpreted-program-inside-guest }
  * Method: { live, post-mortem, record-and-replay }
  * Cross product gives us 9 scenarios, some are unlikely to be Wasmtime’s responsibility at the end of the day (e.g. record-and-replay of an interpreted program inside the guest)
* Existing DWARF transform
  * Needs reimplementation
  * Only supports debugging both host and guest at the same time
* Core dump proposal in tools conventions
  * Even if we implement support for generating these core dumps on panic in Wasmtime, users will need tooling to make sense of the core dumps
* Live debugging guests layered implementation sketch:
  * Debugger API in Wasmtime to expose stepping/breakpoints/etc and Wasm machine state.
    * Doesn’t know anything about source languages, just exposes Wasm as the spec says it should be.
  * Debugger protocol server implemented on top of debugger API, communicating over the wire with a frontend 
    * Exposes source language info over the wire by interpreting DWARF, if present and requested
* Shared infrastructure between most use cases?
  * Perhaps the debugger protocol server
    * Always serving data over the same protocol
    * Source of data can vary depending on the debuggee and debugging method
* Concerns
  * Testing approach?
    * Will want a randomized/fuzzing approach in addition to regular tests
    * One initial idea in https://github.com/bytecodealliance/wasmtime/issues/5537
  * Debugging protocol(s) and their frontend and feature support
    * Reverse stepping supported?
    * Wide-spread editor support and/or easy to use UI?
  * Debugging interpreted programs requires defining (and standardizing!) new APIs to expose information to the host/debugger
    * Not something we can solve in the Wasmtime project alone
  * Who actually has engineering time to commit to implementation work?
    * Not everything needs to be implemented all at once, all or nothing
    * But doing all this design and RFC work is a bit useless if none of it is going to be implemented
* What to do with the existing DWARF transform in the meantime? Remove? Keep?


## Attendees

- Andrew Brown
- Ben Striegel
- Chris Fallin
- Jeff Charles
- jiazho
- Joel Dice
- Johnnie L. Birch
- Nick Fitzgerald
- Ralph Squillace
- Saúl Cabrera
- Till Schneidereit
- tnachen
- wilsonny371
- Jamey Sharp
- Luke Wagner

## Notes

* Till:
    * Multiple RFCs and potentially forming a SIG to avoid losing interest and motivation. 
    * It's not purely about integration with Wasmtime, but also about integration with other tools. 
* Nick:
    * Having a SIG makes a lot of sense. 
* Andrew B:
    * Right now we can map back to Wasm via DWARF, but it would be great to map from assembly to Wasm AND back to the original source language. 
* Chris F: 
    * Debugging protocol that will allow us to do both (^).
* Ralph:
    * We probably want to allow debugging at all levels, but there needs to be an order and prioritization; having a sort of "matrix".
* Till:
    * Another thing to think about: what's the intersection between debugging and profiling?
* Nick:
    * (Regarding the core dump proposal): taking a snapshot and dumping it into a file might not be enough. This will need tooling to build extra around it.
* Chris F:
    * Debugging API: is Wasmtime the right place? Maybe GDB can interpret Wasm-DWARF and avoid putting that responsibility on Wasmtime.
* Johnnie L Birch:
    * Can we come up with solutions that work with multiple runtimes? And not Wasmtime specific?
* Chat (Andrew B, Ralph):
    * See https://reviews.llvm.org/D78801 as related work; this is from Microsoft’s larger team. Paolo is busy rebuilding that PR so that it builds now before we push to remerge. It is a patch to LLDB using the gdb-remote protocol.
* Nick:
    * This (runtime agnostic) would be in the realm of standards (debugging, etc).
* Till: 
    * The debugging story in Wasmtime perhaps shouldn't be held back by getting into standards. 
* Nick:
    * Agreed. We have enough room on the things that we can do just for Wasmtime before having to get standards involved.
    * Ideally we should build a debugger front end, and instead build our solutions in a way that works with existing tools. 
* Luke:
    * Seems that we are talking about multiple interfaces: low-level and high-level, maybe the high-level is the ones that we don't have to build ourselves. We can worry about the low-level ones and hook tools for the high-level ones.

* Nick:
    * Sort of chaining protocols together. 
* Till:
    * Not deploying the debug info to where you run the code is an advantage, and is less overhead. 
* Nick:
    * There are some challenges with this approach, mainly how to map back the state of the program.
* Joel:
    * Will having a server (as middleware) allow Wasmtime not have to deal with DWARF?
* Nick:
    * Yes.
    * We should treat everything as untrusted (not trust the debugger).
* Joel: 
    * Wanting to debug the guest and host means having to trust (since we are crossing the boundary).
* Ralph: 
    * This (wanting to debug guest and host) is a completely different mode.
* Nick:
    * But it's still a valid use case.
    * Engineering time: need to consider.
* Chris F: 
    * DWARF is broken.
    * Given the current plans, should we delete that code?
* Till:
    * Doesn't work for most things, works for some things.
    * Shouldn't delete as long as it doesn't represent a huge maintenance overhead.
* Joel:
    * Should we advertise that it's broken?
* Chris F:
    * The overhead is a bit of support, not much more than that.
* Jamey:
    * Seems that the most common bug is fixable (an assert) if anyone has the time to do it.
* Nick:
    * Should keep it.
    * Add a message that says that the current implementation is buggy.
* Andrew B:
    * +1 for a message.
* Till:
    * Would it make sense to have a help message that points to a given issue?
* Nick: 
    * Makes sense, we can discuss async, maybe through a PR.

## Action Items
