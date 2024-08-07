# August 07 project call

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

- abrown
- jameysharp
- avanhatt
- fitzgen
- elliottt
- cfallin

### Notes

- Updates
  - jameysharp
    - Working on removing stack-limit checks in Wasmtime. Seems we may not need
      Cranelift's stack-limit functionality at all; may consider removing it.
      Some additional machinery around libcalls.
      - fitzgen: make sure cg\_clif still has what it needs
      - jameysharp: for sure; believe they use stackprobes so should be fine
  - elliottt: no updates
  - avanhatt:
    - continuing upstreaming Veri-ISLE work
    - in continuing work: finding the difference between term signatures for
      CLIF instructions in mid-end and back-end annoying; may unify on
      signature with type
  - abrown: no updates
  - cfallin: updates on fastalloc in RA2 from Demilade Sonuga (GSoC participant)
    - reviewing code, first draft looks good overall, should be usable in CL
      once we merge it
  - fitzgen:
    - landed rework of user-stackmaps approach; doing true liveness with
      worklist; PR up to move over, then next step is to remove old
      (regalloc-supported) stackmaps from CL
    - working on upstreaming Pulley Cranelift backend
      - trying to generalize from 64-bit to 32/64-bit

- explicit types in CLIF instruction terms in lowering-environment ISLE terms
  - initial issue comes with vector work -- Veri-ISLE's modeling of types as
    just an integer (bitwidth) in analysis domain falls apart when we need to
    disambiguate all 128-bit cases; need actual type
  - jameysharp: no concerns about efficiency; if type isn't used we'll drop the
    extractor call that fetches it

- fitzgen: interesting aside: blog post on LLVM's instruction scheduling
  - https://myhsu.xyz/llvm-sched-model-1/

- jameysharp: extracting irreducible control flow directly from patterns that
  e.g. LLVM generates in Wasm to fit into reducible control flow
  - cfallin: Michelle's intern project last summer is similar --
    direct-threading interpreter performance transform
  - jameysharp: indeed! a little different; paper on recovering perfect CFG
    from region-based (reducible) encoding
  - cfallin: do we have benchmarks that could serve as good examples where Relooper triggers?
  - jameysharp: speed of Relooper pass also an issue
  - (some discussion where realistic irreducible CFGs may come from; regex
    compilation? error handling gotos?)
