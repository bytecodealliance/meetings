# November 01 project call

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
    2. Demo VTune capabilities to root cause a performance bug in Wasmtime (rahulchaphalkar)

## Notes

### Attendees

- jameysharp
- alexcrichton
- cfallin
- fitzgen
- avanhatt
- elliottt
- Rahul Chapulkar

### Notes

- Rahul: demo VTune
  - we have `perf`, but VTune offers some more details (more perf counters,
    uarch analysis, ...)
  - example: issue 7085 (AVX upper-half false dependency)
  - (deep dive in UI, looking at perf counters and bottlenecks)
  - there will always be some bottleneck; if retire is bottleneck, that's the
    good case
  - static (deps) vs. dynamic --> llvm-mca
  - example: div-sinking-into-loop; microcode resteer events due to denormal FP
    values


- status
  - cfallin: proof-carrying code, pairing with fitzgen: static memories fully
    verified on x86 and aarch64; 1% compile-time overhead; dynamic in progress;
    will get it fuzzing so it's ready to turn on by default
  - jameysharp: no updates
  - alexcrichton: no updates
  - Rahual: VTune presentation (above)
  - elliottt: no updates
  - abrown: MPK build failures on CI, need to work out
  - jlbirch: no updates
  - fitzgen: pairing on PCC, and thinking about how we can use PCC for
    verifying Wasm GC
