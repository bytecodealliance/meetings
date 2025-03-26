# March 26 project call

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
- fitzgen
- alexcrichton
- Amanieu
- bjorn3
- abrown
- cfallin

### Notes

- Updates
  - alexcrichton: no updates
  - abrown: a few updates to new assembler: SSE alignment; now going to add
    support for lock prefix
  - bjorn3: no updates
  - Amanieu: currently working on new BTreeMap implementation; should improve
    regalloc speed
  - erikrose: looking at epoch-checking overhead
  - cfallin
    - aegraph simplification PR: remove union-find and ID canonicalization. No
      compile-time or runtime effects, just simplifies algorithms and code
    - exceptions in Cranelift
      - worked through block-arguments refactor, added instructions and
        exception tables, now adding lowering. Will take this through at least
        working on normal-return path this week. Later, add testing for unwind
        in filetests, get that working, then build Wasmtime usage of it.
      - fitzgen: more thoughts on testing the unwind -- how will it work?
      - cfallin: test separately in CLIF with a simple unwinder.  Unsure
        whether DWARF or using data structures from MachBuffer directly
  - fitzgen: opened issue about control/effect edges in CLIF, something to
    think about in a Wasm-GC world with more aggressive TBAA and memory op
    motion + opts (store-to-load forwarding etc)
    - bjorn3 linked recent article from V8 folks about removing sea-of-nodes
      - we still retain CFG, and deps let us do opts in a cleaner + simpler way
      - cfallin: basic concern they have is complexity; we contain the SoN
        complexity to aegraph phase, have CFG before and after, and these new
        control-flow edges are like a very fancy alias analysis basically (to
        allow code motion on memory ops)

- abrown: benchmarking infra
  - use codspeed.io?
  - fitzgen: skeptical of a for-profit approach; can we do a simple thing where
    we just dump data into a git repo and then build a static site? Then all we
    need to do is maintain a runner that runs periodically
    - bjorn3: this is what rust-analyzer does, works well
      - https://github.com/rust-analyzer/metrics
      - https://github.com/rust-analyzer/metrics/tree/gh-pages
      - https://github.com/trifectatechfoundation/benchmarker-action
    - erikrose: does codspeed provide runners?
    - abrown: right now, Valgrind-based runners to get deterministic inst
      counts
  - alexcrichton: does the above use a custom runner on a public repo? security
    concerns?
    - bjorn3: allowlisted to only main branch or someone with write access
      triggering it; no way to change the allowlist in a PR
