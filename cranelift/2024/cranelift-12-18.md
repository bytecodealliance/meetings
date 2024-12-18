# December 18 project call

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
- alexcrichton
- abrown
- mmcloughlin
- edoardotinto
- avanhatt
- cfallin

### Notes

Updates
- alexcrichton:
  - Pulley backend progressing -- runs everything but SIMD right now. Now
    planning on looking at optimizations -- making bytecode smaller.
    spidermonkey.wasm is 36M of Pulley bytecode right now vs 20M of x86-64
    machine code. Doing fuzzing, etc.
- avanhatt: no updates
- abrown: RFC for new assembler is up; discussion going on.
- edoardotinto: still working on checkpoint/resume
- mmcloughlin: worked more on PoC for hardware intrinsics in Wasm and
  Cranelift. Got to only 30% slower than native. May share report on this.
- cfallin: no updates
- fitzgen: reviewing Alex's Pulley PRs

- Assembler RFC
  - https://github.com/bytecodealliance/rfcs/pull/41
  - fitzgen: should we just have Cranelift operands rather than a generic operand?
  - cfallin: Winch use-case too; also generic library that might be
    maintained/contributed to by others
  - alexcrichton: how complex are operand generics -- library code or "just"
    generated code?
  - abrown: makes coverage/fuzzing harder
  - cfallin: only divergence is in mod operands being split into read/write
    sides, and vreg rather than real reg names; opcodes are "real"
  - alexcrichton: x86, implicit args
    - cfallin: good point, more args than machine instructions
  - alexcrichton: trait design for things like "this reg is statically rdx" as
    well
  - fitzgen: fuzzing's main value is covering opcode encoding, and we'll still
    have
  - mmcloughin: pseudoinsts -- what's the plan?
    - cfallin: generic "sequence" pseudoinst
  - alexcrichton: Pulley shows how autogeneration of MachInsts is soo much
    nicer
  - abrown: next question is integration with ISLE, and verbosity: helpers at
    the level of `x64_and` vs. specific instruction variants/encodings
    - fitzgen: we can write the polymorphic helpers manually and delegate to
      specific. Some are special, e.g. `imm`
    - cfallin: not either/or, we can have instruction families (`and`, etc) and
      still write `imm` by hand
    - alexcrichton: might be helpful to see `and` lowering rules done both ways
    - alexcrichton: also, what about the AVX variants -- helpers automatically
      select AVX vs classical SSEn, that's useful to abstract too
  - let's do some small PRs to see how it looks!
  - mmcloughlin: metadata like relocations etc?
    - cfallin: yep, metadata is handled by real assemblers too; definitely in scope
    - abrown: way it works now is to have MachBuffer-like trait
    - alexcrichton: put MachBuffer itself in assembler?

- Administrivia: skip next two weeks due to holidays (Dec 25 and Jan 1); meet
  next on Wed Jan 8.
