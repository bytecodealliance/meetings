# May 27 project call

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

- erikrose
- alexcrichton
- thejimmybrisson
- bjorn3
- fitzgen
- cfallin
- Jacob Denbeaux

### Notes

- Updates
  - bjorn3: no updates
  - thejimmybrisson: no updates
  - alexcrichton:
    - RustWeek: lots of discussions about Wasm (tangential to Cranelift).
      Getting multivalue support in LLVM-based producers. Realized that
      fuel+epochs don't work to interrupt large bulk ops (memory/table copies,
      etc) -- refactored.
  - erikrose: no updates
  - Jacob: no updates
  - fitzgen: looking at bug reports for safepoint spiller for GC stackmaps.
    Trying to land alias regions generalization. Maybe going to focus on
    Sightglass improvements after GC work winds down.
  - cfallin: no updates

- Perf infra to find regressions?
  - alexcrichton: downstream integrators need to gate on perf regressions, so
    we should gate on it upstream to avoid causing issues
  - infra: codspeed.io? use Cachegrind/Valgrind for determinism?
  - want something end-to-end beyond Sightglass as well (Wasmtime-shaped)
  - cfallin: avoid some concerns about noise by always doing differential
    comparisons, not single trend line
  - fitzgen: suite runtime -- maybe not every PR, maybe every night vs previous
    night etc
  - alexcrichton: good to have a fast suite as well so people don't ignore it.
    10-20 min?
  - fitzgen: hour is more realistic
  - cfallin: maybe fast smoke-test suite on every PR (one interesting wasi-http
    program in wasmtime) and full suite every night
  - thejimmybrisson: multiple microarchitectures, ISAs?
  - Jacob: SVD is a useful concept to better understand PCA
    - While PCA is likely the correct technique, brief review of SVD can show
    why this is possible. A matrix can _always_ be decomposed into three
    matrices representing rotation, scaling, and rotation. This factorization is
    often used as an intermediary to compute the PCA.)
    - [Visual Guide](https://www.reddit.com/r/learnmachinelearning/comments/s66d63/what_is_singular_value_decomposition_svd_a/)
    - [Transformation Graphic](https://en.wikipedia.org/wiki/Singular_value_decomposition#/media/File:Singular-Value-Decomposition.svg)
    - [Writeup on Factors & Relation to PCA](https://www.ibm.com/think/topics/singular-value-decomposition)
