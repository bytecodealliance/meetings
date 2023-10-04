# October 04 project call

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
    2. fitzgen: [New RFC: what is vs is not a security bug in Wasmtime (and Cranelift)](https://github.com/bytecodealliance/rfcs/pull/32)
    3. alexcrichton: Idea: target-specific CLIF rewriting to simplify lowering?

## Notes

### Attendees

- afonso360
- viktar
- jameysharp
- saulcabrera
- fitzgen
- alexcrichton
- abrown
- elliottt
- cfallin

### Notes

- fitzgen: heads-up about [new RFC: what is a security
  bug](https://github.com/bytecodealliance/rfcs/pull/32)

- alexcrichton: target-specific CLIF rewriting to simplify lowering?
  - currently RISC-V backend rules are a bit tricky because of ISA limitations;
    e.g., 64-bit compares only
  - also, every backend has duplicated code now for i128
  - would be nice to rewrite CLIF into a certain shape in the mid-end first

  - cfallin: indeed, has come up before with e.g. div+rem in x86
    - some discussion in https://github.com/bytecodealliance/wasmtime/issues/5623
      - udivrem for an op that provides two results (dividend and remainder);
        rewrite into this on x86 to make use of the one machine instruction
    - would factor a lot of things better
    - interesting history -- Cranelift used to work this way for all lowering!
    - only real issue is compile time; but for rare things may be OK
  - jameysharp: compose mid-end with backend?

  - alexcrichton: to be clear, don't want to lift all machine lowering into
    this level! Just some specific tweaks

  - how to ensure we don't rewrite back? use cost functions

- Status
  - viktar: no updates
  - cfallin: plan to start working on Proof-Carrying Code idea soon
  - afonso360: implemented all compressed insts in RISC-V, reviewing Alex's
    RISC-V PRs, have some ideas for SIMD opts, found weird regalloc case and
    filed issue
    - elliottt: may be fixed with latest regalloc2 changes, will check
    - alexcrichton: a few weeks ago, used latest RA2, verified we're compatible still
  - jameysharp: no updates
  - saulcabrera: finished call-indirect PR, working on table insts now, then
    multivalue support
    - fitzgen: would be neat to support wasmtime-bench-api if it doesn't
      already; then we can compare compile+runtimes with Cranelift
    - cfallin: what else is missing for Wasm MVP?
      - saulcabrera: just some FP stuff and some memory ops; goal is to be done
        by Dec
  - alexcrichton: looking a RISC-V a lot, refactoring and improving lowerings
    (icmp, removing pseudo-insts, ...)
  - abrown: wasmtime MPK work merged, tracking issue for remaining bits needed
  - elliottt: no updates
  - simonas: filed issue for allowing external backends
    - https://github.com/bytecodealliance/wasmtime/issues/7124
  - fitzgen: no updates on CL, mostly looking at Wasm GC stuff

- abrown: benchmarking stuff
  - fitzgen: Wasm CG may create a subgroup for benchmarking?
  - tricky bit is a lot of benchmarks rely on JS
  - micro vs macrobenchmarks, concern if focus goes to the former
  - simonas: also depends on what one calls "real-world": different use-cases
    exist (not everyone is encoding video, or ...)
  - fitzgen: would consider success if proper statistics frameowrk, and would
    want to include SpiderMonkey.wasm as a real-world benchmark
  - abrown: unclear if will use Sightglass or not
  - cfallin: still useful if it creates a benchmark suite we can import
    - alexcrichton: glue code may be an issue though, if it uses JS
  - cfallin: would be useful to have open discussion about this in CG context
