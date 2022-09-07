# September 7th project call

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

* avanhatt
* cfallin
* fitzgen
* akirilov
* abrown
* jsharp
* afonso360
* bnjbvr

### Notes

No agenda.

- avanhatt: type inference in verification / annotation engine
    - cfallin: aarch64 only, or x64 too?
    - avanhatt: 4 individual example rules, not entire backend wide yet
- bnjbvr: re: incremental cache, no issues after using it internally for a week or so
    - the less inlining, the more cache hits
- fitzgen: ABI signature refactoring, speedup (fewer allocs). Plan to move params and returns in a single allocation.
    - arena based btree for regalloc2
    - wasmtime 1.0 performance went out yesterday, relevant for folks here because it talks about cranelift a lot
    - upstream arbitrary crate shouldn't be updated yet, because that broke signed int generation
- jsharp: reviewing work from afonso, thanks very much!
    - remove Overflow/NotOverflow condition codes, is that ok?
    - no objections
- akirilov: fixed implementation of get_return_address CLIF + forward-edge CFI
- afonso: clif fuzzing is not great, 50% of inputs rejected. 50% that survive, mostly timeouts. 10% end up in division by zero. 50 executions/sec is surprising. Measured that and trying to improve.
    - timeouts fixed
- abrown: added single instruction SIMD instruction in fuzzing framework
    - removed `XmmLoadConst`
    - looking into using CET (Control-flow Enhancement Technology) to mitigate against spectre mitigations. Would like to find interested people to discuss.
    - cfallin: interested, could discuss here or wasmtime meeting. For spectre variant 2, things like BTI? already done for aarch64, should do it too for x64
    - abrown: all of wasmtime should be compiled with certain flags
    - fitzgen/akirilov: interested too
- cfallin:
    - catching up on reviews
    - work on regalloc2: removing reg mod, use operand constraints instead
        - regalloc2 has compat layer with regalloc.rs, can be removed
        - long term goal is to use only constraints, use SSA form
        - discussion of a better API for modified (input/output) registers
    - pushing migration of aarch64 to ISLE
    - middle-end optimizers: merge RFC Soon 🔜
        - cleanup integation into cranelift codegen + real application algorithms
        - need to rewrite algorithms using non-recursive form (b/o user-provided inputs) using stacks
        - avanhatt: could use a max recursion depth param?
        - cfallin: good solution if nothing else works
        - cleanups and refactorings, PRs will come

Other questions:

- afonso: updates on aarch64 fuzzing on oss-fuzz?
    - fitzgen: issue where oss-fuzz maintainer mentioned it's in progress a month ago, no motion since then
    - afonso: fuzzing necessary for tier 1 target
    - fitzgen: basically wait for oss-fuzz to support it (could set up own infra, but lots of work)
    - afonso: do test sometimes manually on aarch64