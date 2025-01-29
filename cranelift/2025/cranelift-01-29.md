# January 29 project call

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
- erikrose
- abrown
- cfallin
- Rahul Chaphalkar

### Notes

- Updates

  - alexcrichton: some minor Pulley things (below)
  - fitzgen: no updates
  - Rahul: no updates
  - abrown:
    - posted PR for new assembler: https://github.com/bytecodealliance/wasmtime/pull/10110
  - erikrose: no updates
  - cfallin: reviewing abrown's PR

- Pulley
  - alexcrichton: mostly wrapping up work -- wrote docs, did benchmarking
  - fleshing out provenance/miri stuff
    - approach for small wast tests: compile separately, then run precompiled
      blobs in miri; falls over for bigger tests
    - uncovered one stack-borrows violation (safe in tree-borrows) in the
      runtime; added a helper to make it safe in both
    - in Pulley: VMPtr that holds a usize underneath, expose-provenance and
      recover when deref'ing
  - looking at perf -- behind other Wasm interpreters
    - regalloc: moves are more expensive in interp
    - making bounds-check faster: define loads-from-zero to trap, and use
      Spectre mitigations to get cheaper checks
    - pattern-match whole bounds-check and have bounds-checking op
    - cfallin: want whole bounds-check as one CLIF op for PCC too (described in
      recent talk) -- should work out what works for both, this would be nice
    - alexcrichton: interesting to note we used to have heaps in Cranelift
      - cfallin: indeed, removed because a lot of extra complexity; here we
        would re-introduce a single instruction with defined semantics, more
        contained
    - cfallin: will write up issue on CLIF side; orthogonal from Pulley side if
      Pulley lowerings initially pattern-match the exploded bounds-check
      sequence
