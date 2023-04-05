# April 05 project call

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

- afonso360
- cfallin
- alexcrichton
- jameysharp
- elliottt
- avanhatt
- fitzgen

### Notes

- Alex: looking at XNNPACK for SIMD intrinsics. With egraphs, instruction
  scheduling is much worse in a long pipelined loop body. Perf regression of
  10% due to increased register pressure.
  - cfallin: fundamental to how elaboration works: we go to a location-less
    format (sea of nodes) for pure ops, so we throw away any scheduling work
    that was in the original input program. Need to recover this via
    heuristics.
  - jameysharp: register pressure-related scheduling? Several papers on this;
    introduce artificial edges to force scheduling. May require quadratic
    all-pairs queries though.
  - cfallin: reg pressure within block? We could do some sort of simple greedy
    binpacking; for blocks over a certain length, when elaboration wants to
    place a node into a block, rather than just before use, possibly higher in
    the block

- Alex: two blocks, ops split across blocks, load can't sink
  - cranelift-wasm creates unnecessary blocks
  - could we fuse blocks? During egraph pass, before, after?
    - issue for fully general fusing including through constant conditionals
    - we could do something simpler for actual unconditional blocks
    - probably the simplest answer for this particular case is to not generate
      the unnecessary block in cranelift-wasm

- Status
  - cfallin: plan to remove non-egraphs pipeline; discussions, code-review
  - afonso: working on RISC-V SIMD starting next week
  - jameysharp
    - wrote up lots of issues/ideas
    - thinking about OR-patterns in ISLE. Useful for commutative ops
  - elliottt
    - RA2 work
  - alexcrichton: none
  - avanhatt: verification of icmp on aarch64, making progress
  - fitzgen:
    - working on tail-calls, mostly in Wasmtime at the moment
    - clean up aliases before lowering?
