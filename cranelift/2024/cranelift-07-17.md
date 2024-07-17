# July 17 project call

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
    2. (aspe) updates on new instruction scheduling techniques after egraph optimization

## Notes

### Attendees

- dmitris\_aspetakis
- fitzgen
- abrown
- Panagiotis Karouzakis
- elliottt
- avanhatt
- alexcrichton
- jameysharp
- cfallin

### Notes

- Dmitris and Panagiotis: working on instruction scheduling after egraph
  elaboration (since we lose instruction ordering during opt) -- scheduling
  ideas in #6260
  - implemented critical path, preservation of sequence, use-count heuristics
    - also tried adding costs to critical path; improvements from previous
      impl, but not over baseline
  - not seeing improvement, except in XNNPACK (which started this conversation)
    - fp32 sparse: 15-20% improvement
  - cfallin: first off: thanks! and perfectly ok to have negative results (ins
    scheduling not as important in most benchmarks)
  - cfallin: good to be examples-driven -- look for metrics that could be
    indicative of bad scheduling
  - jameysharp: a good one: fraction of all instructions that are moves to/from
    stack (i.e., spills/reloads)
  - cfallin: use this as first heuristic to find cases to stare at; also use it
    to evaluate why improvements occur. (Would be good to study XNNPACK and why
    it improved: fewer spills? effects with LICM? ...)
  - Dimitris: compile time still 20%-ish (?)
    - cfallin: fine if it brings benefits! will want to put it under a flag
    - jameysharp: also a bit tricky to work out how to make it configurable
      (this is a rewrite of elab)
      - jameysharp: most of cost is from single data structure (prio queue) --
        turn that one off / use a simpler data structure?
      - Panagiotis: unclear how optimized prio queue is
  - cfallin: good next step -- figure out why XNNPACK gets better -- that will
    guide our thinking from here
  - jameysharp: super-cool work! didn't expect anything at all from issue,
    fully satisfied with what we've learned so far, thanks!

- Updates
  - jameysharp: looking at stack-limit checks and stackprobes.
    Interesting bit: how Wasmtime uses Cranelift's libcalls
  - alexcrichton: no updates
  - avanhatt: working on upstreaming isle-veri
  - Dmitris: no other updates
  - elliottt: no updates
  - Panagiotis: looking for similar issues to work on; noticed
    div-with-magic-constants; no further work yet
  - abrown: no updates
  - cfallin: looking at stackprobes with Jamey, no other updates
  - fitzgen: fuzzbugs with new user stackmaps; porting Wasmtime over

- alexcrichton: new math ops in core Wasm for wide multiplies,
  add-with-overflow
  - made one bignum benchmark 15% faster, still far from native
  - trying to address bad codegen -- always reifying carry flag, ...
    - cfallin: could we do a native wide add op (128 bits) rather than
      try to piece it back together from carries?
    - alexcrichton: maybe yeah... multiple output instructions are hard
      in LLVM and Cranelift; also experience with wide multiply was it
      was very easy in comparison
    - (some discussion about known-bits and high 64 bits; variant that
      is 64+64->128?)
    - overall goal is to find the shape that the proposal should be
    - (lots of discussion of codegen options; link for native code:
      https://godbolt.org/z/jPscrG73Y)
