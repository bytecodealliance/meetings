# May 24 project call

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
- elliottt
- cfallin
- avanhatt
- jameysharp
- abrown
- jlbirch
- afonso360

### Notes

Status

- cfallin: nothing this week
- elliottt: nothing this week
- abrown:
  - new `func_addr` lowering
  - questions about EVEX encoding
    - problem: we had to hardcode scaling factor; it's intertwined with "tuple
      type" of vector instruction (64->128 load etc)
    - alexcrichton: maybe a method on Avx512Opcode to get tuple-type; puts this
      up-front when adding new instructions
- alexcrichton: nothing aside from SSE/EVEX stuff
- jameysharp: looking at 128-bit multiply lowering (`__multi3` etc)
  - possible optimizations in mid-end?
  - alexcrichton: change compiler-rt to use 64-bit multiplies instead?
  - jameysharp: can reuse some of the 32-bit muls, does make sense as-written
  - jameysharp: we have umulhi in CLIF but this isn't representable directly in Wasm
  - jameysharp: C ABI and multi-returns vs. struct return pointer
  - (lots of discussion about crypto algo optimization)
  - abrown: why not wasi-crypto?
    - jameysharp: can't use it for many use-cases that need the raw primitives
      to be used in slightly different ways
- avanhatt:
  - working on reimplementing isle-veri annotation language in ISLE parser
    itself, for upstreaming
- jlbirch:
  - looking at optimizations on Wasmtime side for async modes
- afonso360:
  - adding vector instructions for RISC-V SIMD support
  - typesafe wrappers for register types in RISC-V
- fitzgen:
  - working on tail-calls (almost working on x86-64!)
  - (discussion about macroinstruction with clobber-restores, permute, memmove,
    ...)
  - current thinking that throws everything into the one macroinst is probably
    the best we can do
  - long-term, use part of RA2 that does parallel moves, factored out and
    generalized

- alexcrichton: advocate for i128.mul in Wasm? or umulhi? Motivate with
  "pattern-matching is brittle"?
  - jameysharp: in general pattern matching is brittle, but here there's really
    one way to do it
  - alexcrichton: true, but still function call overhead unless we do inlining;
    and lots of users; the earlier we add intrinsics the better
  - fitzgen: work with pairs of i64s instead? then no new types
  - jameysharp: adc and umulhi too
  - cfallin: generally, consider putting together a "bignum ops" Wasm proposal?

- jameysharp: inlining? needed if we rewrite mul pattern into 128-bit muls
  - separate offline tool that does Wasm-level inlining?
  - fitzgen: still have memory writes due to struct return
  - cfallin: does Binaryen do this?
    - does have inlining pass, unclear how we can direct it to inline certain
      things
  - fitzgen: tricky bit is heuristic, "what to inline", we lose noinline;
    extend "branch hinting" proposal to "inline hinting"?
  - cfallin: inlining before or after opt fixpoint loop? makes sense in both
    places in different cases; opt before inline to see that `__multi3` becomes
    tiny and can be inlined; opt after inline to take advantage of visibility
    of inlined code
    - jameysharp: this is one advantage of egraphs: load func body into egraph
      and have visibility
    - jameysharp: difficulty is multiple in-edges/callers; how to use
      knowledge?
    - cfallin: probably a more general block-specialization
      pass/rule/transform: when multiple preds (or multiple callers in this
      case), make copy of block with just one pred, then we can see through
      blockparams
  - cfallin: probably we should do classical inlining first, then consider
    egraph-integrated inlining later as it's a much larger change
