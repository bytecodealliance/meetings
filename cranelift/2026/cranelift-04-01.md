# April 01 project call

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

- bjorn3
- alexcrichton
- fitzgen
- erikrose
- jimmybrisson
- cfallin
- uweigand
- jlbirch

### Notes

- Updates
  - alexcrichton: no updates
  - erikrose: dead-load-with-context instruction for use with epoch
    optimizations
  - cfallin: no updates
  - bjorn3: no updates
  - jimmybrisson: new to Cranelift, taking over s390x backend maintenance
  - jlbirch: no updates
  - uweigand: reviewing Jimmy's patches for new s390x backend instructions;
    will still be around, not going away
  - fitzgen: no Cranelift updates.

- alexcrichton: should uweigand or jimmybrisson be an official maintainer on
  the repo to approve s390x PRs?
  - will add both uweigand and jimmybrisson

- `_imm` instructions and legalization --
  https://github.com/bytecodealliance/wasmtime/issues/12911
  - lots of discussion about orthogonality; iconst can't have i128s because
    size of InstructionData but can we indirect it?
  - discussion about whether `iadd_imm` and friends are good for code size; but
    we legalize unconditionally anyway
  - leaning toward keeping i128, but ruling it out for `_imm` rules
  - can write helpers to generate iconsts as well

- csdb on aarch64, value speculation, and whether we want to take the penalty
  by default
  - consensus: it's a 2x or more penalty on interpreters due to `br_table`
    mitigation; we don't want this if no one else is taking that penalty; we
    can make it a configurable knob for those who need extra levels of
    assurance
