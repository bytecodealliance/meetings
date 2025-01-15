# January 15 project call

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

- alexcrichton
- fitzgen
- uweigand
- abrown
- John Witulski
- Rauhl Chaphalkar
- saulcabrera
- mmcloughlin
- cfallin

### Notes

Updates

- alexcrichton:
  - Pulley is complete now -- just some fuzzbugs coming in.
    Benchmarking, "still has a ways to go", want to build a profiler for it
  - segfault in fuzzbug, want to spend time debugging it more
- mmcloughlin: working on verifing aarch64 vector rules
- cfallin: no updates
- uweigand: no updates
- Rahul: no updates
- saulcabrera:
  - getting aarch64 spec-tests working in Winch
  - working on getting SIMD done and reviewing PRs for threads
- abrown:
  - working on integrating new assembler for ISLE
- John Witulski: no updates, just a visitor (hi!)
- fitzgen: extended mid-end GVN'ing machinery for side-effectful instructions
  to work with zero-result instructions as well; used for traps when one trap
  dominates another
- mmcloughlin again: quick update on HW-specialized Wasm with Cranelift intrinsics
  - https://github.com/WebAssembly/design/issues/1528#issuecomment-2581252153
