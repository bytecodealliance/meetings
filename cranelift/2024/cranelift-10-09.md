# October 09 project call

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
- abrown
- cfallin
- uweigand

### Notes

- Updates
  - abrown: none
  - alexcrichton: got 128-bit arith (wide arith) proposal to phase 2 with CG
  - uweigand:
    - a few patches simplifying backend after recent changes re: ABI link
      registers, no need for memcpy anymore
    - found issue with stack-overflow test on native; not run on emulator in CI
      - issue is that we don't emit stackprobes; need to implement
      - accessing backchain is a bit tricky -- on stack overflow we can't
        access the stack from a signal handler -- some discussion on issue
      - alexcrichton: stackprobes and guard page are last-ditch protection
        anyway; we have explicit stack checks
      - cfallin: eventually want to move away from this at least in some
        architectures (Jamey's patch for removing stack-limit checks)
      - (some discussion, we'll work this interaction out when we get there)
  - fitzgen:
    - no updates for self
    - update for Alex: cranelift-wasm crate merged into wasmtime-cranelift; no
      need to have abstraction that separates a generic Wasm embedder anymore
  - cfallin: no updates

- alexcrichton: security updates on Wasmtime going out today
