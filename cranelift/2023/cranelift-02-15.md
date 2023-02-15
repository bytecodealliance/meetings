# February 15 project call

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
    1. GitHub's Merge Queue feature for Wasmtime (alexcrichton)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- elliottt
- cfallin
- fitzgen
- alexcrichton
- uweigand
- afonso360
- jameysharp
- jlbirch

### Notes

- alexcrichton: use GitHub merge queue?
  - https://github.com/bytecodealliance/wasmtime/pull/5766
  - current situation: tests run against PR branch alone, not PR on top of
    current main
  - GitHub now has a feature to test against current main and merge only if it
    passes
  - Some tests still run on PR but not as many; filters to select based on
    subpaths that are touched

  - cfallin: how beta? we should monitor and be able to roll back
  - cfallin: concurrency/batching: right now it won't roll up multiple PRs in
    one test; is this fundamental or just a function of our concurrent setting?
    - alexcrichton: seems it can only do one at a time
  - cfallin: maybe not so bad though, much of the bottleneck today is latency
    (see tests pass and click the button not forget) rather than throughput
    (many PRs getting through per day)
  - fitzgen: we should send feedback to GH on inability to do rollups without
    concurrency enabled

Status

- cfallin: not much dev on CL, just codereview
- elliottt:
  - changes to `br_table`: jumptables are inline now; much cleaner;
    almost ready to merge
  - precise-output compile tests using capstone rather than VCode's
    pretty-printing
  - capstone out-of-date on s390x? concerns valid, maybe we could print both
    VCode pretty-print and disassembly
    - does make diffs twice as noisy
  - uweigand: we should make sure Capstone is maintained, up-to-date and
    correct
    - uweigand: llvm disassembler and gas are two sources of truth for s390x
    - value in having any comparison to a third-party
    - does keeping both (vcode and disassembly) cover concerns?
    - sure; at that point do we need vcode pretty-print?
    - still useful to show pseudo-insts
- jameysharp:
  - thinking about branch opts during egraph pass
    - constant-phis, branch folding
    - bigger conversations to have about backedges
- alexcrichton: none
- uweigand
  - scalar versions of min/max ops
  - encoding bug in s390x found by capstone-disassembly PR
  - use constant-pool in s390x?
    - need relocs on constants, is that possible?
    - cfallin: not currently, could be added
- afonso360
  - fuzzgen: calls, `call_indirect`
  - want to get tail calls too so tail-call work has a fuzzer
- jlbirch: none
- fitzgen:
  - tail-calls, with jameysharp. Mostly wasmtime side now, reworking
    trampolines as per RFC
  - souper-synthesized opts
    - issue filed with raw examples, need generalization

Branches in egraphs

- optimizing cycles with blockparams, revisiting blocks
  - jameysharp: have a way this could work: inputs to blockparams from preds
    all need to be defined in dominating blocks, after opts
  - cfallin: potential issue with monotonicity: "generative" nature of
    allocating new node IDs would make the revisit produce a new result, so we
    never converge
  - jameysharp: interning makes us get the same IDs the second time (nice!)
  - potentially an option for a higher (slower) optimization level; will file
    an issue
  - cfallin: would be really nice to get rid of `remove_constant_phis` pass;
    it's the one we remaining that couldn't fold into egraphs pass and this is
    very annoying (and loses opportunity)

- fitzgen: backward analysis -- can we do it during egraph pass?
  - cfallin: during lowering, since that is backward, but not during forward
    egraph pass
  - or separate pass then another round of egraph

- fitzgen: more/different opt levels?
  - standard O1/O2/O3 instead of "size", "speed", etc

- jameysharp: branch folding can alter domtree, implications?
  - cfallin: removing an edge can move a block lower, not higher, on domtree;
    so the thing we depend on (visiting uses before defs) will still be correct
  - jameysharp: indeed, but can lose opportunity

Cranelift interpreter

- fitzgen: concretize the interpreter, remove trait indirection?
  - jameysharp: use during cprop? would be nice to share operator semantics
    implementation

Relaxed SIMD

- alexcrichton: do we have it?
  - not yet, need to add it
  - fitzgen: we could translate to regular Wasm SIMD

- alexcrichton: use VEX 3-arg forms, or any reason not to?
  - no reason, this would be nice; improved regalloc

- afonso360: merge loads into ops?
  - main issue is alignment: loads built-in to ALU ops in x86 SIMD require
    alignment but Wasm loads don't guarantee that, so at least for Wasm, we
    won't have the ability to do this
