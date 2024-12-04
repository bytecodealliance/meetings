# December 04 project call

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

- mmcloughlin
- uweigand
- alexcrichton
- avanhatt
- saulcabrera
- abrown
- cfallin

### Notes

- Updates
  - mmcloughlin: have a PoC to integrate hardware intrinsics in
    Cranelift/Wasmtime (class project). May give a talk about it.
    - replacing general logic with hardware instructions where possible;
      prototype toward abrown's Wasm hardware-specific optimizations proposal.
    - issue with vec-to-GPR moves.
  - uweigand: bug yesterday: `RUST_BACKTRACE=1` caused testsuite to crash on
    s390x. Regression from tailcall ABI and wrong CFI unwind info with incoming
    stack area. Fixed with PR. New interesting case: SP is not unchanged, also
    not saved to stack, but recomputed on unwind by adding to existing SP.
    (Specifically the add to remove arg area from stack was not represented.)
  - alexcrichton: wrestling with CI, no other updates
  - avanhatt: in verification project, some students starting to look at
    mid-end.
  - saulcabrera:
    - refactoring Winch internals to avoid panic'ing where possible.
      - https://github.com/bytecodealliance/wasmtime/issues/9566
    - two PRs as prereq for epoch support in Winch
  - abrown:
    - prototyping new assembler layer for Cranelift. Difficult integration with
      ISLE, slowly working through it.
  - cfallin: no updates on CL; mostly in Wasmtime right now
