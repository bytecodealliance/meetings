# July 19 project call

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

## Notes

### Attendees

- jameysharp
- alexcrichton
- elliottt
- fitzgen
- cfallin
- avanhatt
- abrown
- uweigand

### Notes

- alexcrichton: fuzzer with memory padding in functions to test islands etc;
  limit the padding to ensure we don't run out of memory
- elliottt: no updates
- cfallin: working with intern on potentially doing a
  direct-dispatch-interpreter transform on recognized interpreter patterns in
  Wasm input
- avanhatt: working up upstreaming verifier
- abrown: Wasmtime-related -- ColorGuard (memory protection keys for more
  efficient use of VM space)
  - cfallin: fuzzing/testing: make sure we test the trampolines / context
    switches -- most dangerous part (missing updating keys etc)
    - fitzgen: use call-hooks? have never been safety-critical before, need to
      test them more rigorously
  - (discussion about testing, turning on in stages)
- jameysharp: helping fitzgen on tail calls
- uweigand: no updates
- fitzgen: aarch64 tail calls working too; and riscv64 as of yesterday. s390x
  eventually (Q for uweigand)?
  - uweigand: will take a look
