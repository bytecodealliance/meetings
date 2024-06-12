# June 12 project call

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
- elliottt
- jameysharp
- cfallin
- alexcrichton
- Chinmay

### Notes

- Updates
  - elliottt: no updates
  - jameysharp: worked on annotating CLIF from `--emit-clif` in Wasmtime with
    source file and line number info; want to extend into more parts of
    Cranelift
    - fitzgen: nice, would be good to have this in `wasmtime explore` too
  - Chinmay: just here to observe!
  - cfallin: no updates
  - alexcrichton: no updates
  - fitzgen: first part of stackmaps-and-safepoints in `cranelift-frontend`
    landed; will carry it through to backend, then remove the old mechanisms

- alexcrichton: uload8 and friends; intent was to keep IR smaller?
  - cfallin: indeed, and compile time as a result. keep them if pragmatically
    we need them for, say, 10% win when compiling Wasm; if 1% maybe not; all
    else being equal, would be nice to have just a clean `load` and `store` and
    nothing else
  - fitzgen: no real principled approach at first; these are from ancient days
  - cfallin: example of Wasm-SIMD load-and-splat
  - alexcrichton: cases of the two ops getting separated
    - cfallin: issue still open: sinking to use-site in elaboration in midend
      can sink use to another block; then we can't merge because color is
      different
  - jameysharp: do load sinking the other way, and hoist the extend?
    - cfallin: gets tricky in general case if we're hoisting other ops (e.g.
      adds) that may depend on some other computation
    - jameysharp: but we can do this for single-input cases, doesn't increase
      register pressure
    - (lots of discussion then) let's do Jamey's idea
  - jameysharp: block merging: can we be mildly more clever in cranelift-wasm
    and avoid redundant blocks?
    - fitzgen: we should special case branch-to-return by just doing a return
    - jameysharp: we're eagerly creating fallthrough blocks for every
      control-flow structure, even if it turns out unnecessary
    - cfallin: we should clean that up but also having the general
      block-merging pass would be nice; or maybe in cranelift-frontend we can
      do it automatically

- elliottt: sometimes running out of VRegs when producing giant functions from
  some tooling (matters more when we split up macro-ops)
  - cfallin: we could have a "big" mode where we use 48 or 64 bits for
    Operands; we know before the operand-colleciton pass how many VRegs we
    have; will file an issue
  - jameysharp: discussion about renaming, avoiding creating temps for every
    output; another approach almost worked, not quite
