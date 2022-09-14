# September 14 project call

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
    1. CET

## notes

### CET support

* abrown: entire binary needs to be compiled with CET
  * must be run in a CET kernel
  * any one object file not being CET enabled will cause the whole program to
    not be CET enabled
* abrown: is there a cargo feature flag that would control adding the bti instructions?
  * anton: it's not enabled by default, and not in cranelift or llvm
  * there's an aarch64 specific flag called `use_bti` that's disabled by default
  * the flag stays false if the platform doens't support `bti`
  * on macos, bti support is enabled by default
* abrown: CET offers protection against speculative execution, does bti also
  protect against speculative execution?
  * anton: assuming we've correctly identified all cases of bounds checking, we
    shouldn't need additional work
  * anton: bti doesn't address spectre-v2
  * there is a 64-bit register that can be used to distinguish between software
    contexts
  * branch predictions consider the software context number in that register

### Attendees

* abrown
* afonso360
* akirilov-arm
* bjorn3
* cfallin
* elliottt
* fitzgen
* jameysharp
* jlbirch
* tshepang

### Status

* afonso360
  * performance on cranelift-fuzzgen
* elliottt
  * continuing to work on the isle overlap checker
* abrown
  * differential fuzzing target is now done, found some cranelift bugs
  * it should be open season for removing instructions used elsewhere
* jameysharp
  * working with elliottt on overlap checking performance
  * working with afonso360 on fuzzing
  * working on a metrics collection framework
* akirilov-arm
  * researching spectre-v2 and seeing what we can do about it in
    wasmtime/cranelift
  * there are PRs incoming that finish the isle port for aarch64
* fitzgen
  * wasmtime security and correctness post is up
  * https://bytecodealliance.org/articles/security-and-correctness-in-wasmtime
  * investigating perf: switching regalloc2 to using an arena allocated btree
* jlbirch
  * continuing to work on testing infrastructure
  * there's a little work remaining for how we present speedups
* cfallin
  * regalloc2 semantics cleanup: we're close to removing all pinned vreg uses
  * pending patches for caller-site of the ABI
  * starting to pull patches out of the mid-end work
  * longer-term, investigating compiler performance
