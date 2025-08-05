# July 23 project call

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
- Amanieu
- alexcrichton
- abrown
- cfallin
- jlbirch
- uweigand

### Notes

- Updates
  - cfallin:
    - exception handling: dynamic context
      (bytecodealliance/wasmtime#11285)
    - decided not to support inclusion of RA3 due to
      maintenance-burden concerns
  - alexcrichton: no updates
  - bjorn3: no updates
  - abrown:
    - added logic to understand boolean expressions of CPU features
  - jlbirch: no updates; hoping to get back to CI enabling for APX
    instructions (qemu, etc)
  - Amanieu: no updates
  - fitzgen: inlining supported on Cranelift side now! Working on
    updating compilation orchestration on Wasmtime side
  - uweigand: no updates
  
- regalloc3: discussion about decision not to take it for now
  - cfallin: primary reasoning has to do with maintenance burden

- exceptions:
  - why not use blockparams?
    - state at try-call, not in successor block; moves occur in
      successor block and we'd need to interpret them
  - why not use stackslots?
    - re-inventing parts of regalloc, reusing them, etc
  
- discussion: vmctx in frame in general? Does this affect what we do?
  - (lots of discussion about source locations, inlined functions,
    referring to vmctx value location in metadata)
  - maybe this pushes us to stackslots instead for dynamic tag
    contexts?
  - conclusion: more discussion needed; may or may not switch to
    stackslots now for exception-table (but existing in-progress PR
    uses SSA values)

- milestone: new x64 assembler handles all instruction emissions now!
