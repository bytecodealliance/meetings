# April 23 project call

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
- erikrose
- abrown
- jlbirch
- cfallin
- alexcrichton

### Notes

- Updates
  - jlbirch: working with Andrew and Rahul on adding instructions to new
    assembler; looking at VEX encodings
  - abrown: more work on x64 assembler; fixed regs
    - a few offsets shift by a few bytes -- slightly different encoding
      somewhere?
      - maybe shifting register numbers leading to extra REX bytes
  - erikrose: no updates
  - alexcrichton: no updates
  - cfallin: update by proxy from bjorn3: `cg_clif` can now use Cranelift
    exceptions in branch, and everything works; will land and update `cg_clif`
    side once we release this in next Cranelift release

- alexcrichton: xor zeroing idiom regalloc idiom -- why not either pseudoinst
  that does all zeroing, or else custom regalloc metadata?
  - have a branch with the first, but also changed operand handling to allocate
    many `Vec`s -- that part probably not desirable
  - in principle, pseudoinst approach is less footgun-y, but no one seems to
    mind too much

- abrown: duplication between `Inst` helpers that construct different
  cases/sizes of ALU insts and the corresponding logic in ISLE
  - cfallin: in fullness of time this shouldn't need to exist -- Winch can use
    new assembler directly, so can ABI code
  - can we move toward reusing ISLE logic directly? tricky -- ISLE context
  - alexcrichton: simplify cases, then do postpass to choose smaller encodings
    - fitzgen: peephole in general as we emit into VCode
