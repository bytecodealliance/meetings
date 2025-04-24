# April 24 | Wasmtime Project Bi-Weekly

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
   1. _Submit a PR to add your item here_
   2. fitzgen: heads up I am starting to prototype CM+GC integration
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)

## Attendees

* TODO

## Notes

- Nick: started prototyping CM GC support
    - Alex: consider eventually adding core wasm feature to alias a component-level type in a module
    - Nick: tricky due to abstraction layering
    - Nick: would be nice to define all modules then do lifts and lowers
    - Alex: exports make that tricky
    - Nick: would to have a "type_of_this_export" operator
    - Alex: doesn't solve lowering case; need to define all types before lowing
    - Nick: meanwhile, will just have to duplicate types
- Alex: C/C++ API now being tested in CI using e.g. GTest
    - Alex: should start writing tests as features added to C++ API
- Roman: what's component model C API status?
    - Alex: There was a WIP contribution 6 months or a year ago; recently picked up again by another contributor
    - Alex: incrementally adding features; can do some stuff, but still lots to do
    - Alex: not sure how much time new contributor has to devote to this
    - Alex: richer type system makes it challenging
    - Roman: starting to work on running component functions via C API wrapper around Rust dynamic API
    - Alex: would love to have an API for that
- Dan: added support for moving libcall support from cranelift to Wasmtime
    - Dan: now asserting that generated code does no cranelift-level libcalls and thus no relocations
    - Chris: that means code can be read-only now
- Paul: planning to open issue about async yield; not really yielding due to LIFO behavior in Tokio; should move to deferred queue via `yield_now`
    - Chris seems like Tokio bug
    - Paul: there's an unstable option to disable LIFO; otherwise intentional behavior
    - Paul: could add config hook for overriding
    - Alex: where are these yields?  Can just use tokio directly in wasmtime-wasi
    - Paul: it's for epoch/fuel interrupts
    - Pat: could we use a user callback for that?
    - Alex: would need to add dep injection trait for that
    - Paul: that was one of the two options discussing with Aaron
    - Alex: guest yield coming with CM async
    - Paul: can maybe talk with Carl (tokio mainainer) about changes there
    - Nick: should maybe add randomness in addition to LIFO?
    - Paul: main think they're considering is to allow "stealing" LIFO slot
    - Nick: maybe it's okay to have tokio-specific code in Wasmtime?
    - Pat: can think of two tokio-free projects
    - Chris: seems like a scheduler priority bug.  Does Poll::Pending/Waker have such semantics?
    - Alex: not specified; runtime specific
    - Chris: guess there's a starvation issue
    - Alex: currently to yield, you call Waker::wake and then return Pending, but runtime can decide what to do
    - Pat: LIFO currently only used for multithreaded executor in Tokio
    - Nick: instead of waking then pending, could wait for next epoch to wait
    - Paul: other benefit of deferred queue is that if there's nothing more urgent will run again immediately
    - Nick: could add support for embedder-provided async callback for epoch interruptions
    - Alex: would require allocation due to type erasure, but maybe not a problem for zero-sized futures
    - Nick: could this be a combinator
    - Alex: no, probably not due to the leaf calling the waker
    - Paul: comment on issue if you have opinions; shouldn't be a huge patch in any case
