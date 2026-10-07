# October 07 project call

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

- cfallin
- fitzgen
- bjorn3
- adamrk
- thejimmybrisson
- alexcrichton
- erikrose
- avanhatt
- Aelin

### Notes

- Updates
  - bjorn3
    - `return_call` works without `tail` ABI? seems to not be caught by validator?
      - fitzgen, cfallin: surprising, not intentional, we should fix the validator
      - wanted for tail calls in Rust with same-signature funcs
      - we could support for SysV reasonably
  - adamrk: no updates
  - thejimmybrisson: no updates
  - alexcrichton: no updates
  - erikrose:
    - put up PR for `interrupt_poll` instruction
  - avanhatt:
    - case where a verifier did not catch a mid-end bug (14526)
    - mid-end opt constructed invalid CLIF
    - working on a more helpful error
  - Aelin:
    - first time joining, hi!
    - PR for 128-bit atomics on aarch64; will do s390x as well
  - fitzgen:
    - working on new register allocator (regicide); passing all Wasmtime tests
    - currently better codegen than regalloc2 on 2 benchmarks, wash on a
      handful, worse on 7-8
      - all swings within 5% or so
    - compile time much slower than regalloc2, but hasn't been a focus yet
    - bug in dead-store elimination: trapping store, infinite loop, trapping
      store; first will be removed
      - cfallin: so divergence is kind of a side-effect
      - fitzgen: avoid DSE across any loop?
      - cfallin: this must be why infinite loops are UB in some languages?
        - bjorn3: LLVM has explicit attribute for loops that will not
          infinitely loop (not diverge)
      - fitzgen: maybe eventually a barrier to make loops observable; for now,
        no DSE across loops
  - cfallin: no updates
