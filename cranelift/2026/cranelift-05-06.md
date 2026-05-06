# May 06 project call

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
    1. [Performance of inlining](https://github.com/bytecodealliance/wasmtime/issues/13254) (@alexcrichton)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- fitzgen
- alexcrichton
- thejimmybrisson
- bjorn3
- erikrose
- cfallin
- uweigand

### Notes

- Updates
  - alexcrichton: no updates, just inlining discussion
  - erikrose: big chunk of signal handler work done for signals-based epochs
    (in Wasmtime)
  - bjorn3: no updates
  - thejimmybrisson: working on pseudo-fuzzing with LLMs on s390x
  - uweigand: no updates
  - cfallin: looked at egraph blowup a little bit; want to add fuel to LHS
    multi-matchers
  - fitzgen:
    - branch-folding opts in aegraph; mutating CFG as we walk; interesting
      requirements wrt DFS preorder of domtree (for GVN to work) + RPO to see
      preds before a given block, so we know if it's reachable.  Will update PR
      with an algo that should do both.
    - alias regions:
      - idea to build true sea-of-nodes with effects on loads/stores
      - but: today egraph doesn't support optimizing insts with multiple
        results
      - what is still useful is arbitrary alias regions (e.g. for TBAA in Wasm
        GC, and other stuff)
      - have a branch where existing fixed alias regions are replaced by
        separate entity definition; currently a compilation slowdown, hope to
        fix soon

- Inlining performance
  - alexcrichton: want to turn on inlining for context.get/set for wasi-p3 stuff
  - refactored config switches a bit
  - measured impact of no-inlining vs. inlining intrinsics (only)
    - fairly large impact to compile time; creating callgraph and strata took a
      lot of time
    - refine: only focus on inlinable callees
    - still slowdown: 15-20% compile time slowdown
    - fundamental impact is the barrier between translation and compilation
  - solutions?
    - want to have finer-grained depenencies: when asking for callees' bodies,
      dynamically wait on translation
    - bjorn3: use Wasm-level heuristics (size of callee etc) instead?
    - cfallin: benign races and recompilation? (long-pole slow to compile
      functions mean we have some free CPU to recompute something locally
      rather than synchronizing)
    - cfallin: user-provided strata and no barrier in the middle?
    - fitzgen: mark certain callees as "easy to compile yourself", without sync?
    - cfallin: break apart tasks into continuations with fine-grained task runtime?
    - fitzgen: use async and a multithreaded async executor!
    - fitzgen: simple first step: extend existing non-stratified branch today
      to eagerly recompile just intrinsic funcs
    - bjorn3: regarding copying of CLIF: if func is big enough that it's not
      inlinable, no need to copy; if small enough, copy doesn't matter
    - (lots of discussion about tradeoffs; ended with: we should open-code the
      codegen of calls to known unsafe intrinsics and not rely on inlining for
      now)

