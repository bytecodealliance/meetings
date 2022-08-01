# August 1 project call

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

- elliottt
- uweigand
- fitzgen
- jameysharp
- afonso360
- bnjbvr
- akirilov-arm
- cfallin

### Notes

- Updates
  - elliottt
    - porting x64 lowerings to ISLE
  - abrown
    - meta-fuzzer target: able to compare N backends, and does
      single-instruction fuzzing mode with multiple input parameters (multiple
      executions) per compilation
    - working on it, fixing some bugs in bindings, looking good
    - cfallin: mode to run all backends vs just pairwise? efficiency overall (n
      vs n^2) vs getting further with a fast pair to get more coverage
    - fitzgen: timeouts in oss-fuzz; pairwise may be better
    - abrown: ok to start pairwise, can bring other modes online later, the
      target is parameterized to make that easy
  - uweigand
    - s390x: SIMD support landed!
      - two qemu bugs require some disabled tests for now but qemu 7.1.0 will
        fix so our CI can run all lowerings
    - frame-pointer mode for supporting stackwalking PR
    - looked at i128 --> started to look at cg\_clif in general
    - have a set of patches so that cg\_clif testsuite passes!
      - TLS, object file support, ...
    - endianness of vectors
      - s390x has only big-endian vector ops, so current scheme is to byteswap on load, then reinterpret vector lanes (0,1,2,3 -> 3,2,1,0)
      - however, raw_bitcast and ability to change type breaks this
      - make endianness explicit: on function? as ISA flag (akirilov)? as part
        of the vector types, e.g. `I32x4LE` vs `I32x4BE` (cfallin)?
      - abrown: want to fix raw\_bitcast, make a native CLIF type for Wasm's
        `V128`
      - uweigand: still wouldn't fix ...
      - more discussion in issue #4566
  - fitzgen
    - fast stack unwinding landed!
    - generate trampolines from CLIF: need intrinsics to get FP, SP, ...
    - DHAT allocation profiling led to easy 25%-30% compilation speedup with a
      five-line patch (nice!)
  - jameysharp
    - DHAT profiling, another compiler speedup
  - afonso360
    - fuzzer/interpreter work to continue to support more CLIF ops in direct
      CLIF fuzzer
    - cg\_clif support on Windows works now (PR pending)
  - bnjbvr
    - incremental compilation
      - single module from scratch speeds up by 15% due to redundant function
        bodies -- unexpected but great!
      - more gains expected on recompilations of slightly-modified modules
      - working now on test mode, fuzzer, make sure fuzzer catches bugs
      - refactoring per cfallin's suggestion of type-level separation between
        cache-key-included state and not (and let compiler see only the former)
      - deserialization perf tends to matter a lot, and comparison cost.
        alexcricton's suggestion of using a stronger hash (SHA?) and just
        comparing that, not comparing whole function body in key to speed up
        the hit case
  - akirilov-arm
    - ISLE porting work on aarch64
    - stackwalking PR review for aarch64
      - followup: use BTI, PAC; have some ideas
    - PR to harden Spectre conditional-move mitigation further on aarch64:
      value-speculation barrier to guard against future microarchitectures'
      value speculation through the cmove
  - cfallin
    - work on improving performance of egraph-based mid-end rewrite framework
      - came up with a new data structure, "acyclic egraph", which should help
        by letting us avoid a fixpoint loop; more details to come
