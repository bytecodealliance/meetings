# May 10 project call

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
    1. Any opposition to changing the x64 backend to pretty-print in intel syntax?

## Notes

### Attendees

- avanhatt
- cfallin
- fitzgen
- afonso360
- alexcrichton
- uweigand
- abrown

### Notes

- elliottt: Use Intel syntax for x86 pretty-printing rather than AT&T?
  - reason: more uniform in register order (dest first) wrt other
    architectures, better when making cross-arch changes in e.g. regalloc
  - no objections generally
  - fitzgen: also: pretty-print SSA dests in different form?
    - add v1/v2, v3
    - v2 = add v1, v3
    - former seems better for multi-dest (e.g. div); and more naturally
      "disappears" post-regalloc

- fitzgen: parallelism within function compilation?
  - dependency-on-rayon concern addressed with feature
  - alexcrichton: where is parallelism?
    - cfallin: RA2 hard
    - cfallin: deterministic or allow nondetermism? probably the former
    - dataflow analysis can converge even with nondeterministic order
  - fitzgen: want this for veriwasm-next (proof carrying code)
  - alexcrichton: coordinate threadpools etc? Issue in rustc that it is a child
    process of cargo, parallelism at higher level
    - we can turn off parallelism within CL in that case
 
 Status

- afonso360: RISC-V SIMD progress. vconst, RA changes, arithmetic tests
  working in test suite
- abrown: work on WASI testsuite in Wasmtime, none on CL
- cfallin: talking with Trevor about RA2 stuff
- avanhatt: thesis defense next week! no CL hacking
- alexcrichton: none
- uweigand: none
- elliottt: RA2 performance benchmarking, overlaps branch in good state now
  - some benchmarks have compilation time improvement up to 14%, SM is 1-2%
- fitzgen: tail calls, thinking about parallelism
