# June 24 project call

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

- thejimmybrisson
- erikrose
- uweigand
- avanhatt
- cfallin
- fitzgen
- Jacob Denbeaux

### Notes

- Updates
  - Jacob: no updates
  - uweigand: no updates
  - erikrose: no updates
  - thejimmybrisson: no updates
  - avanhatt:
    - landed the new version of the verifier!
    - still not in CI; cfallin will work on that
  - cfallin:
    - planning to build caching layer for verification so we can check into git
      a set of cached SMT queries; a kind of "proof witness" (trusted) so
      verification in CI can be fast, but still fail if something new is added
      without verification
  - fitzgen:
    - PCA on the Sightglass suite -- down to 5% of original Wasm instructions
      while still being representative
    - goal now is to right-size the benchmarks (maybe ~5M Wasm instructions?)
      now, then do an official PCA+clustering run, present the results in a
      future meeting so we can decide how to proceed
    - working on Wasmtime's use of Cranelift alias regions
    - cutting down on need for constant-phi removal pass (better SSA
      construction -- using cranelift-frontend Variables for Wasm block
      params/results)
    - thinking about dead-store elimination and what it will require; also want
      to LICM trapz/trapnz

- dead-store elimination
  - fitzgen: one of the problems is how to do dead-store in a forward pass, vs.
    doing it backward (especially when one wants chained elimination)
  - also interactions between dead-store and existing alias analysis and
    idempotent stores
  - fitzgen: could build a summary graph of loads/stores
  - Jacob: another tricky problem is related to side-exits. Do they prevent
    removing dead stores or sink store into side-exit?
  - fitzgen: right now, we don't do code-motion for stores
  - cfallin: what if stores are dead by default and live when observed? then
    this doesn't require postdom, and doesn't require a backward pass; works
    within existing alias-analysis framework
    - fitzgen: needs fixpoint to iterate until convergence to be sound?
    - cfallin: on edge to already-visited block (e.g. loop backedge) assume all
      stores live, conservatively
