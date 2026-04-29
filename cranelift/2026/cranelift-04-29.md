# April 29 project call

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
    1. [Enabling inlining by default in Wasmtime](https://github.com/bytecodealliance/wasmtime/pull/13214) @alexcrichton
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- erikrose
- fitzgen
- alexcrichton
- uweigand
- bjorn3
- cfallin
- thejimmybrisson
- jlb6740 

### Notes

- Inlining by default
  - open PR to enable inlining by default in Wasmtime; Cranelift-related
  - background: wasi p3 will have context.get/set libcalls for shadow stack
    pointer; now we have Cranelift-defined implementation of intrinsics, but we
    need inlining to inline this
  - maybe ok for existing code because we mostly have single LLVM-produced
    modules?
    - fitzgen: kind of true but it still implies additional passes/analyses;
      worth benchmarking the compile-time hits
    - cfallin: could have must-inline tags on just the special functions; avoid
      the analysis pass
    - alexcrichton: inline right during translation?
    - cfallin, fitzgen: push back a bit; better to keep the design that we
      ended up at
    - inlining messes with backtraces?
      - cfallin: we could use the virtual-frames functionality from
        guest-debugging, without the instrumentation turned on, to make
        backtraces more complete in this case; may actually be better generally
        to have a single source of truth for this

- Updates
  - thejimmybrisson: very smooth on s390x now that Ulrich can review (thanks!)
    - working on carry and borrow-in
  - bjorn3: no updates
  - alexcrichton: no updates
  - erikrose: working on Wasmtime interrupt checks with virtual memory; work
    continues
  - cfallin: trying to understand egraph blowup case; need to dig in further
  - jlb6740: no updates
  - fitzgen: added two benchmarks to Sightglass (!):
    - splay-tree with Wasm GC
    - sqlite3 benchmark program
    - hoping to do PCA to look at which benchmarks "overlap" and can be removed
      so eventually we have a reasonable suite we can always run without manual
      subsetting
    - thinking about escape analysis and stackmaps; escape analysis may force
      us to put stackmaps back in Cranelift proper again, but early in the
      pipeline with legalizations before mid-end. This is because we want
      escape analysis *after* inlining
  - uweigand: no updates

- virtual memory-based interrupts
  - cfallin: how do we do call injection? see issues written up from debugging
    work
  - erikrose: not sure yet
  - cfallin: paging in more: arg register pinned by dummy load instruction to
    have vmctx
  - alexcrichton: also clobbered scratch reg for saving the return address; so
    sig handler doesn't need to actually push a frame or access VMStoreContext
