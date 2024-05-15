# May 15 project call

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
    1. (fitzgen) extending `iadd` to accept `iadd(r64, i64)`, and similar for a few other instructions
    1. (fitzgen) GC-focused optimizations:
       * escape analysis, eliding allocations, and SROA of GC objects
         * idea for integrating with inlining
       * per-type-hierarchy alias analysis
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- fitzgen
- alexcrichton
- elliottt
- uweigand
- avanhatt
- saulcabrera
- jameysharp
- cfallin

### Notes

- fitzgen: how to make iadd take an r64 and i64
  - why not just "add support"? constraints on instruction defs hard to write
    properly and keep verifier happy?
    - "only one, not both" is hard?
    - type inference gets confused with heterogeneous args
    - what does the controlling type parameter apply to?
  - cfallin: why not pull up the "remove safepoints and refpoints" idea, and
    use ints everywhere? (make safepoints explicit ptr dataflow in/out of
    marker insts)
    - (general consensus)

- status
  - abrown: no updates
  - avanhatt
    - working with undergrads for summer projects on isle-veri
  - jameysharp
    - discussions about ISLE and eliminating distinction between constructors
      and extractors
    - working on last parts of cleanup of operand-collection in lowering
      - interesting outcome: no longer need to pass fixed/nonallocatable
        registers to RA2
    - resolving CLIF aliases, single-pass resolution; egraph pass resolves
      aliases already, just move that before egraph pass and do it
      unconditionally
  - elliottt: no updates
  - uweigand:
    - s390x tail call ABI design! posted comment to issue. Still using
      backchains (no FP), compatible with SysV unwinders. Outgoing arg area is
      below rather than above register-save area and allocated during call
      sequence. Some care taken to ensure async unwinding (e.g. due to
      timer-based profilers or whatnot) still see a valid backchain even during
      call sequence
    - planning to implement
    - fitzgen: extra store overhead for temporary backchain is just to support
      async unwinding right?
      - uweigand: indeed; but also if there are traps during outgoing arg
        setup, e.g. div by 0
        - because lowering happens in ISLE, potential from automatic
          conversions
  - saulcabrera:
    - small PoC of wasmtime test-macros crate posted
  - alexcrichton: no updates
  - fitzgen: no updates
  - cfallin: etor/ctor discussion with Jamey

- fitzgen: GC optimizations
  - escape analysis, SROA
  - inlining allocation fastpath (nursery allocation)
    - how to elide this if we determine no escape and promote the allocation?
  - jameysharp: do escape analysis before producing CLIF?
    - fitzgen: one option, yes
    - cfallin: potentially interaction with DCE/cprop/branch folding/... in
      main opt loop?
  - inlining to optimize fused component-model adapter functions
    - idea: build all egraphs; synchronize once all exist; then do a second
      pass and inline
      - cfallin: keeping all IR memory-resident could easily blow up?
      - cfallin: perhaps keep summaries around and selectively rebuild egraphs
        of inlinees when needed
      - cfallin: inlining with egraphs is really nice though! good
        canonicalization and sharing rewrites across all merged code
      - jameysharp: speculatively inlining! put function body in as one option;
        still learn things about it even if selection picks the call in the end
      - cfallin: need some notion of choice for side-effecting ops and/or sub-skeleton
        - jameysharp: maybe this is a more general side-effecting optimization
          change (generalizing so we don't have to commit early) too
  - fitzgen: TBAA for GC types -- extend alias analysis
    - cfallin: interesting observation: today we say we don't do "advanced
      alias analysis" (e.g. on Cranelift landing page); we should clarify
      exactly where the boundary is (but this TBAA does sound reasonable)
    - fitzgen: probably the "take advantage of UB" kind of alias analysis is
      where we don't want to go; this is more about distinct GC types
    - jameysharp: what about subtyping?
      - fitzgen: type hierarchies are units, not individual types
      - jameysharp: is this just structs vs arrays or...?
        - fitzgen: user-defined list
      - cfallin: can also set more than one bit (don't have to do disjoint
        regions) -- pick subtrees, general type sets all
        - actually we don't have bits anymore, rather an index
    - jameysharp: name regions more generally, don't bake in "table", "vmctx",
      ...
    - jameysharp: multimemory wasm could benefit from more generality too
