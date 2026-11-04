# November 04 project call

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
    1. fitzgen: Removing instructions from the side-effectful skeleton as we prove them pure during rewrites? (Context: https://github.com/bytecodealliance/wasmtime/pull/14879)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- theotherjimmy
- bjorn3
- alexcrichton
- fitzgen
- cfallin
- erikrose
- Amanieu
- adamrk
- uweigand

### Notes

- fitzgen: side-effectful skeleton: optimizing tests for when quotient is zero
  - requires removing the div/rem op from the skeleton
  - cfallin: existing functionality to remove insts (e.g. loads/stores) not enough?
  - fitzgen: a little weird, want to avoid rooting things, rewrite after, ...
  - https://github.com/bytecodealliance/wasmtime/pull/14879

- Updates
  - theotherjimmy: working on PIC in s390x
  - erikrose: keeping eye on inlining fix for `interrupt_poll`
  - alexcrichton: no updates
  - bjorn3: no updates
  - Amanieu:
    - noticed that BlockOrder in CL is suboptimal
    - have WIP patch to address
  - adamrk: no updates
  - uweigand: been reviewing s390x patches from Jimmy
    - while reviewing, had a few Qs:
      - PLTs? used in Wasmtime? what does near/far mean?
        - near is same-module, expectation is a direct call
        - in Wasmtime, should not use any PLTs; we morally do our own version
          with indirect calls between modules
      - TLS: need reloc pointing to GOT
        - reloc in .text currently
        - effort to push constant pools to .rodata will fix this
  - fitzgen: no updates
  - cfallin: no updates
