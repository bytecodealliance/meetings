# August 14 | Wasmtime Project Bi-Weekly

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
   1. [Internally using `async` in Wasmtime](https://github.com/bytecodealliance/wasmtime/pull/11430) (@alexcrichton)
   2. [PAGEMAP_SCAN](https://github.com/bytecodealliance/wasmtime/pull/11372) (@alexcrichton)
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)

## Attendees

* Alex
* Nick
* Chris
* Roman
* Pat
* Till
* Joel
* Andrew
* Victor
* Dan

## Notes

- Using async internally in Wasmtime
  - Alex: talked last week about cleaning up Wasmtime's Send/Sync
  - First step: https://github.com/bytecodealliance/wasmtime/pull/11430
    - Made stuff async that needed to be async
    - T in Store<T> must now be Send
  - Alex: worked out well overall; needed some tricks for async enabled vs. disabled
  - Alex: State machine overhead: 20% slowdown instantiating spidermonkey.wasm
    - Don't have a lot of experience profiling async code
    - Consider cranelift-compiling instantiation routine (initializing globals, etc.)
    - Slowdown less noticable with more threads
  - More steps to go
  - Biggest hack: wanted to use async closures, but needed to use a macro for now given state of Rust language
  - Nick: slowdown is unfortunate but not unexpected given async overhead
    - Given that it addresses the last major known unsoundness, seems worthwhile
    - Could talk with Rust lang folks about improving performance of async throughput
  - Alex: Got stuck on inlined frames when profiling
  - Chris: Curious about whether this is fixed cost vs. a multiplier
    - Alex: don't know and don't know how to answer the question
    - Chris: have you tried benchmarking other modules?
    - Alex: yes, but need to test again since other modules had noisy results
    - Chris: more acceptable if fixed overhead
    - Nick: main concern is "normal" modules with one memory, table, etc.
    - Till: might depend more on modules if there's a big difference for spidermonkey vs. others
    - Alex: might still be noise in both cases; need to try again
    - Nick: might try bumping iteration count for Criterion
  - Alex: another benefit: makes it harder/impossible to forget possibly-async things are async
    - Example: lowering values
  - Till: very in favor of landing this even if performance is 20% slower (i.e. wouldn't go from that the other way)
  - Nick: no need to wait for other work; can merge this as-is
  - Till: regarding cranelift generating instantiating code: why would that help?
    - Alex: const eval code is surprisingly slow due to handling general case; pushing it to cranelift to boil it away
    - Alex: likewise table initialization
    - Nick: remove loop dispatch overhead and conditionals
    - Nick: would simplify async-ification since it would be a single async call to wasm
    - Alex: had a PR to switch init from ValRaw to Val which helps; const eval _should_ help more
    - Alex: won't erase async overhead, but will improve baseline
  - Nick: we discussed this years ago
- PAGE_MAP_SCAN
  - Alex: heads up that it has landed in main; on by default; may add option to force off or force on
  - Chris: do CI machines have new enough kernels to exercise it?
    - Nick: and ossfuzz?
    - Alex: no assertion that it's on for CI, but I know that it _is_ on; don't know about ossfuzz
    - Nick: should default off for next release, at least
    - Pat: yes, and then default on for subsequent release
    - Chris: assert that it is tested in CI
      - Till: might be flaky on GitHub CI
      - Pat: ubuntu-latest has guaranteed kernel version, so should be reliable
    - Nick: similar issues for MPK, but sounds like it's moot based on Pat's comment
  - Nick: if you're touching random pages, will eventually all be resident
    - Till: newer ioctl allows more control
    - Nick: concern is that we'd be using as much physical memory as virtual memory
    - Alex: will try to bottom out concern with Paul; maybe about "why bother memset'ing 1MB?"
    - Till: might keep ever-increasing resident set
    - Nick: should just degrade to what we have now in worst case
    - Till: realistically, for a module that uses a downward-growing shadow stack, should always be better off (or at least as good in case where stack exhausted)
    - Nick: now satisfied that we'll hit point where we start madvising after threshold reached
    - Alex: still room for optimization (avoiding redundancy)
    - Till: that's what new ioctl helps with; in theory avoids TLB shootdown
    - Nick: if we start manually clearing dirty bits, will need metadata to track
    - Alex: could take full responsibility for this instead of making the kernel take care of it
    - Nick: right, that's what I meant
    - Alex: in that scenario, we'd decide when to memset
    - Till: but currently, must memset unconditionally
    - Till: instinct is that we'll reach steady state quickly
    - Nick: could add randomization, e.g. every N requests we madvise
    - Alex: will delay that until we have more info
    - Till: could track average dirty page delta per reset to avoid fine-grained tracking
  - Alex: had a bug with respect to fork; fixed with pthread_at_fork
    - Nick: need to test fork (and post-fork Wasm execution) if we're going to support it (and F5 wants to, at least)
    - Alex: have some coverage now
    - Nick: need more coverage to build confidence that others aren't lurking
    - Alex: known to not work for Mac (Machports)
    - Nick: WAST tests would give good coverage
    - Alex: not practical to run entire test suite
    - Nick: but can identify valuable subset
    - Alex: WAST tests wouldn't catch pooling allocator issues, though
    - Alex: using FDs for stuff is not robust for fork
    - Chris: could build wrapper around FDs that does PID checks
      - Alex: problem was that I wasn't even thinking about fork
      - Chris: requires being aware; maybe we can enumerate cases and address them
      - Alex: at least a debug assert would be appropriate
    - Nick: would it make sense to add these to rustix?
      - Chris: tricky part is that not all FDs are problematic
      - Alex: too low-level
      - Nick: hard to identify best level of abstraction
      - Alex: not inheriting would be worse because you might use some other FD accidentally
- Till: https://github.com/google/wasefire/issues/458#issuecomment-3188553942
  - project using pulley and Wasmtime
  - got speed improvements by _disabling_ SIMD in Pulley
  - overall improvements vs. their homegrown interpreter
- Nick: a year ago `candymate` found some fuzz bugs
  - Were secretive about how they did it, but now have released a paper about it
  - We rarely generate instructions which operate on rare operand types
  - Paper reverses that by starting with instructions and then produce operands
  - Also harvesting ISLE rules and feeding that to fuzzing for both instruction lowering and mid-level IR
  - Consider creating a corpus that we can feed to fuzzing
  - We had this idea earlier; not too late to do it
  - https://github.com/bytecodealliance/wasm-tools/issues/1484
  - https://insuyun.github.io/pubs/2025/park:rgfuzz.pdf
  - Alex: the harvested shuffle lowering for fuzzing to good affect, etc.
  
