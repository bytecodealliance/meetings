# January 25 project call

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

- fitzgen
- cfallin
- bjorn3
- abrown
- avanhatt
- jameysharp
- elliottt
- uweigand

### Notes

- cfallin: working on Cranelift 2023 roadmap; let me know if any ideas to add.
  Will publish a draft RFC in a few days.

- Status
  - fitzgen
    - started working on Souper again -- feed rules into egraphs mid-end
    - tail-calls RFC!
  - cfallin
    - egraphs on by default; roadmap; not much else on Cranelift
  - abrown
    - tail-calls RFC: stack protection, CET, thinking.
      - fitzgen: everything still properly nested, returns match actual calls.
        RAs do move around on stack but others think these should be fine
  - jameysharp
    - ISLE codegen backend rework landed
      - ended up not doing translation validation -- seemed to be redoing the
        work the codegen was already doing, may not be worth it
        - thoughts on maybe a dynamic comparison against trivial lowering, may
          come back to it later
        - for now, fuzzing provides good coverage; at least as good as before
  - avanhatt
    - getting close to Wasm-MVP verification
      - some ops still tricky
      - working on switching out solver
  - bjorn3: no updates
  - elliottt
    - merged brif, put up PR to delete brz/brnz
    - next, `br_table` update to use BlockCall with block-args, so we don't
      need to split edges
    - next next, RA2 efficiency improvements making use of SSA properties
  - uweigand
    - fuzzing on s390x merged
      - sanitizers not enabled in upstream Rust fuzzing infra; got PR merged
        there too

- CET
  - fitzgen: understanding of how it works: CET manages a shadow stack; on
    return, checks against address on stack. Since returns still match up to
    calls properly (correctly nested), nothing should break.
  - abrown: yup. Other part of CET is `endbr`; that's fine too, we can insert
    them at the start of functions.
  - cfallin: tangential, but stack switching needs kernel entry?
    - abrown: might be special instructions for userspace to set up its own
      shadow stack; have example to look at
    - cfallin: sounds good, mostly a wasmtime concern actually and not CL I
      guess
