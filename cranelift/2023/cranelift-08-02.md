# August 02 project call

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
- elliottt
- abrown
- cfallin
- lafp
- uweigand
- fitzgen
- avanhatt

### Notes

- Updates
  - cfallin: no updates
  - jameysharp: looking at using parallel-move resolver from RA2 in tail calls
  - elliottt: no updates
  - abrown: no updates
  - lafp: no updates
  - uweigand: no updates currently; planning to look at tail-call work for
    s390x. A little trickier than other platforms because system ABI doesn't
    push/pop args around calls. Also, "backchain" is used instead of FP
    linkage; may end up needing to use FP in new calling convention
  - avanhatt: no updates
  - fitzgen: tail calls landed all the way through to Wasmtime, using Cranelift
    support!

- abrown: refactor `Inst` type -- no longer heirarchy of format then opcode,
  but one enum variant per opcode?
  - cfallin: current factoring does reduce code size
  - fitzgen: but, benefit to shrinking memory size maybe
  - fitzgen: also, side-tables for operands?
  - cfallin: we do build the big array of RA2 `Operand`s; we could directly
    attach those to insts in side-tables
  - cfallin: but we'd need a way to handle the "return a self-contained Inst"
    APIs (e.g., in ABI code) differently -- box up insts with their operands in
    another wrapper type?

- abrown: use yaxpeax?
  - cfallin: maybe; we'd still need emit match for pseudoinsts; and integration
    with MachBuffer, labels, etc; needs some thought

- jameysharp: table-driven approach to inst definition, RA metadata, etc; would
  make above refactors simpler / more approachable
