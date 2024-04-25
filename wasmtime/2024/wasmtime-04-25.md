# April 25 project call

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
    1. [Wasmtime, Cranelift, and no_std](https://github.com/bytecodealliance/wasmtime/issues/8341) (@alexcrichton)
    1. Moving wasmtime-cpp into the wasmtime repository for testing the C API (@alexcrichton)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- Nick Fitzgerald
- Alex Crichton
- Andrew Brown
- Roman Volosatovs
- Jamey Sharp

### Notes

- Alex: propose `no_std` support for Wasmtime, Cranelift
  + Nick: fine by me!
  + Andrew: well-written issues describing the decision!
- Alex: move wasmtime-cpp into Wasmtime repository for better testing
  + Nick: makes sense to me; line to draw--why don't we move all the bindings in? C API is most
    important API to test, others are not as foundational; if C++ helps us test the C API, then it
    is worth it
  + Nick: it would be nice to run the C example programs under Valgrind; more worried about bugs in
    bindings
  + Alex: yeah, the use-after-free was found by Valgrind
  + Alex: don't plan to start this immediately
- Nick: moving forward on GC

