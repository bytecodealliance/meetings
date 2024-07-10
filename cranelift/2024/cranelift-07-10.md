# July 10 project call

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

### Attendees
* Nick Fitzgerald
* Jamey Sharp
* Andrew Brown
* Alex Crichton
* Ulrich Weigand
* Alexa VanHattum

### Notes

- no scheduled agenda topics
- weekly updates:
  - alexcrichton: none
  - jamey: none
  - alexa: continuing to get load/store modeling to work better for x86
  - fitzgen: let's discuss ISLE and recursion
  - abrown: none
  - ulrich: back from trips, nothing to report; getting back to tail calls

##### ISLE recursion issue (@fitzgen)
  - LLVM generating large stack frames for ISLE compiler
  - blew the limit in CI windows-latest; reported as a failure to build in a downstream project
  - Alex was not able to recreate locally
  - `rustc` nightly changes pushed this over the limit; probably close before this
  - fitzgen: can we rewrite as an explicit stack?
  - jamey: the offending function is self-recursive... probably not too hard to rewrite

##### Sponsor a proposal to add new instructions (@alexcrichton)
  - some ideas: 128-bit multiplication, swap bytes (reported by another Rust project)
  - easy to prototype in LLVM and Wasmtime; benchmarked with Sightglass (a Jamey benchmark, `blind-sig`)
  - can we ask web engines to implement this?
  - add overflowing arithmetic to WebAssembly? returns the regular type and an i1 for overflow
  - WebAssembly instructions would return an extra i32 value
  - how to do this in Cranelift? during lowering, how can we pattern match that the input to a br_if is _one of_ the outputs of an instruction--same problem in LLVM (everything works great with single-output instructions)
  - how do we start handling that in Cranelift?
  - fitzgen: eglog paper, this kind of thing can be done by database queries, etc.
  - jamey: you tried 64x64 multiplication? not exactly the same as LLVM's multi3 but is needed for wide integer arithmetic
  - alex: multi3 went away in LLVM when I added this
  - jamey: strong motivation from arbitrary precision arithmetic; what code we get from ISLE for this? Expect a `setcc` instruction to reify the flag into a register; we have to because of potential flag clobbers. A different approach: doing a pass after lowering to identify that flags are placed in registers and instead keep them as flags.
  - alex: seems like the way to go
  - fitzgen: bad if regalloc inserts spills, may mess up peephole matching
  - alex: could do this before regalloc to avoid burning a register? Seems like the way to do this. How to easily benchmark?
  - jamey: there's an `rsa` crate that `blind-sig` got pulled from
  - alex: not `bignum`? Want to do the naive thing first.
  - fitzgen: it may be possible to do this in lowering, e.g., a "sinkable inst," maybe with a new extractor
  - jamey: only works if the other result is not used?
  - alex: uniquely consuming one of the results?
  - [more discussion about best way to do this in ISLE]
  - alex: this is an experiment for contributions from standalone WebAssembly engines to W3C
  - fitzgen: not much follow up to multi-value
  - jamey: there's already an existing proposal for arbitrary precision arithmetic?
  - alex: you mean interval arithmetic?
  - fitzgen: no
  - jamey: another alternative--a single result instruction that asks "would this add overflow?"
  - alex: in some ARM manual I saw something about instruction fusing of back-to-back multiplies, could be useful
  - fitzgen: another alternative--"branch on overflow" instruction
  - jamey: in bignum libraries, may want to add a carry into the next operation
  - alex: the branch idea may be more difficult to add to LLVM
