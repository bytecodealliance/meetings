# October 23 | Wasmtime Project Bi-Weekly

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
   2. instance/memory/table/etc allocation traits (fitzgen)
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)

## Attendees

* Alex Crichton
* Dan Gohman
* Chris Fallin
* Nick Fitzgerald
* Roman Volosatovs
* Joel Dice

## Notes

- Nick: want to brainstorm about instance, table, allocation, etc. traits
  - subsume current memory and instance traits
  - expose iterator-esque combinators to create allocators
  - motivations:
    - pooling allocator is not usable outside of common use case of having lots of virtual memory
      - idea of pooling useful when virtual memory not available
    - not all allocations would be relevant, but many
    - ability to use bump allocations for every store allocation
    - handle OOM in data plane
    - ideally move as much as possible into this trait: everything from the store down (e.g. instances, tables, etc)
    - Chris: want to handle major source of resource exhaustion
  - Alex: nothing to dispute at high level, but need more concrete proposal to really discuss
    - handling OOM and using a trait to allocate are different things
    - like idea of looking at profile of creating instances in loop
      - then use that to select allocations that should be falible
    - if we're not handling OOM yet, we should, e.g. anything over 500 bytes
    - infeasible to catch all OOM cases, but can handle most
  - Nick: don't need 100% coverage
    - if we move allocations into a trait, it forces us to pick appropriate signatures
    - whether 100% worth it vs. 95% and leaving long tail is not clear
  - Alex: concerned about long term maintenance (e.g. catching code that bypasses trait during code review)
    - Nick: could use clippy, e.g. write custom rule if necessary
  - Nick: once you start running wasm, not doing a lot of new allocations, so might not be too bad
    - Should make it easier to catch anomalies during review
    - Alex: e.g. PrimaryVec allocations during runtime
    - Alex: also, component model allocates a lot, especially with async (e.g. Box<dyn Future>)
    - Alex: allocation is so pervasive, would need a test with a failing global allocator
      - even that wouldn't guarantee full coverage
      - Nick: SpiderMonkey does that; it's super good at finding bugs; uses with fuzzing (i.e. under control of fuzzer)
    - Nick: could imagine manual allocation based PrimaryMap
    - Nick: anyhow error: hard to avoid boxing errors
    - Alex: agree with idea, but we're a long way from SpiderMonkey-level enforcement
      - Maybe ignore tiny allocations to start with (e.g. anyhow error)
      - Nick: probably would be a fuzz target
    - Alex: ideally would want this for component model, but have limited fuzzing right now
  - Alex: main opinion: hesitant to make halfway effort; needs to be concerted
    - If F5 wants to make small improvements, fine, but serious effort requires serious CI
    - Nick: yeah, aim for small improvements to start with, then focus on avoiding regressions
    - Chris: regarding F5 motivations: must not panic
      - production build elides asserts inside C-embedded thing
      - Nick: to be clear, it's an F5 problem
      - Alex: it's antithetical goal in wasmtime to never panic; it's the basis for security guarantees
      - Dan: if you elide C assertions, you're creating worse risk than if you aborted
      - Chris: practically speaking need formal, static proof
      - Chris: point is it's not just about OOM
  - Alex: should already be checking for OOM on any major allocation; if not, should fix
  - Nick: need new kind of PrimaryMap to use in this kind of situation
  - Alex: Rust has not standardized OOM recovery, but we can sidestep by controlling blast radius
  - Alex: worth looking at Rust for Linux; e.g. idioms and crates
  - Joel: Erlang-style supervision trees with panic-unwind for blast radius?
  - Nick: alloc crate doesn't expect allocator to panic
- Last old issue triaged: https://github.com/bytecodealliance/wasmtime/issues/1052
