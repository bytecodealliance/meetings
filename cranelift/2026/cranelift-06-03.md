# June 03 project call

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
- fitzgen
- alexcrichton
- cfallin

### Notes

- Updates
  - alexcrichton: occasionally running LLMs over recent changes to find bugs;
    that's how the recent inliner issue was found
  - cfallin: no updates
  - thejimmybrisson: no updates
  - fitzgen: landed alias region generalization PR; also made Wasmtime start
    using it.
    - bug was interesting: we were assigning alias regions to each defined
      memory but also imported memories (one for all imports because aliasing
      of them is unknown). Works in world with single Wasm module, but once you
      inline across modules, some code refers to a memory with precise region
      but other with imports alias region.
      - simple fix: if memory is exported, it always uses imported memory
        region. Kind of sad to degrade this way though.
      - More precise fix: similar to known-function-callee analysis. If memory
        is always the same imported memory...
    - also: Sightglass work; almost done with collecting metrics for PCA.
      First, some boolean features ("do you use exceptions", etc), then static
      metrics (like instruction mix), then dynamic metrics (again like
      instruction mix -- via instrumentation)
      - cfallin: microarchitectural metrics as well (e.g. cache misses, branch
        mispredicts) -- interesting wrt instruction scheduling work, etc
    - removing `_imm` instructions (replacing with dynamic expansion during
      InstBuilder pass)

- alexcrichton: Wasmtime topic: async fiber switching is not portable (assembly
  for each ISA), even with Pulley
  - cfallin: getcontext/setcontext (ah: deprecated)
  - alexcrichton: basically propagate yield/pending through Wasm; make
    hostcalls "really async" rather than on a fiber
  - gets away from the soundness footgun of yielding over Rust frames (which
    are not statically guaranteed to be Send)
  - something like stack-switching in terms of its needs for builtin primitives
    (Cranelift instructions)
  - probably not near-term: just write the inline assembly for a few more ISAs
    in the fiber library
