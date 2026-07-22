# July 22 project call

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
    1. fitzgen: [dead-store elimination](https://github.com/bytecodealliance/wasmtime/pull/13806)
    1. alexcrichton: [maximum stack limit sizes](https://github.com/bytecodealliance/wasmtime/pull/13783)
    1. cpetig: [arm32 backend](https://github.com/bytecodealliance/wasmtime/pull/13815)
    1. fitzgen: [ghc calling convention](https://github.com/bytecodealliance/wasmtime/pull/13913)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- Adel Prokurov
- erikrose
- cfallin
- fitzgen
- avanhatt
- cpetig
- alexcrichton
- bjorn3
- thejimmybrisson
- uweigand
- Obei Sideg
- jlb6740

### Notes

- fitzgen: Dead-store elimination: PR 13806
  - mean 2% slowdown on compile time
  - no speedups on benchmarks, but improved compile-time builtins in Wasmtime
  - we need to decide if we take this
  - at the same time, landing some other opts -- gotten around 2% speedup
    through these
  - cfallin:
    - speedups to pay for slowdown are great, but we could take just the speedups
    - is there a way to separate this / make it optional?
    - most of the slowdown seems to be in postdom -- could we not do the
      analysis and just not say any store postdoms another?
    - config-space increase, unclear heuristics when to enable, ...
    - decided to keep it for now; but we should make sure we understand for
      future opts

- alexcrichton: 1GiB stack-frame size limit

- arm32 backends
  - two PRs! perfectly aligned, non-overlapping: thumb2 (Obei's PR) and full
    arm32 (Christof's PR)
  - thumb2 is a different encoding, but equivalent otherwise -- so one backend,
    with a flag
  - cfallin: history with old arm32 backend -- want to make sure we'll have
    maintainers that stick around and finish, up to ideally full Wasmtime
    support (alexcrichton: or maybe just Wasm-MVP at first)
  - cpetig: basic qemu tests work with i32 on Obei's branch
  - cpetig: propose to merge Obei's backend first; much more complete, much
    easier to test
  - cfallin: testing thumb2: is there a full Linux triple or ...?
    - cpetig: full arm32 can jump into thumb2 mode; arm32 Linux binary with
      trampolines
  - cfallin: overlapping FPU registers; will need to support this in RA2 in
    fullness of time, but generate naive code first by having largest
    allocatable unit managed by RA2
  - alexcrichton: typed registers, and separable assembler?
    - fitzgen: land first, then improve

- GHC calling convention?
  - cfallin: asked a followup in PR -- do you want this, or just more regs for
    args? Seems like the latter -- we can do that more easily than a bunch of
    additional new code with more impact
  - fitzgen: do we want to guarantee support? unsure what burden that would be
  - fitzgen: differences in limits based on callconv is weird in CLIF
    (architecture-independent)
  - Adel: goal is not to have stack frames for efficiency
  - cfallin: we need to always reserve the right to have a frame, for regalloc
    spills. better to architect this as an optimization -- elide frames when
    not needed
  - fitzgen: once we have FP elision then the only difference is number of
    callee-saves; that's more maintainable

- Updates
  - Adel: instruction fusion for overflowing ALU ops + branches
  - erikrose: MMU interrupt functionality in Wasmtime
  - avanhatt:
    - looked at cfallin's caching PR
    - put up a PR to verify midend
  - alexcrichton: no updates
  - uweigand: qemu issue with s390x crashes has been fixed and accepted
    upstream
  - cpetig: no other updates
  - bjorn3: updated Cranelift in `cg_clif`
  - thejimmybrisson: will review arm32 backend as well
  - Obei: no updates
  - jlb6740: restart APX support efforts in x64 backend. Working on a means of
    testing: adding XED as a fuzzing oracle
  - cfallin: Cranelift verification in CI via SMT-query caching
    - some discussion about CI-side cache vs. checked-in and tradeoffs
