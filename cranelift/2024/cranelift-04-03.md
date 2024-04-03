# April 03 project call

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

- fitzgen
- alexcrichton
- avanhatt
- jameysharp
- cfallin
- uweigand

### Notes

- fitzgen: no Cranelift updates
- jameysharp:
  - fix tail calls on aarch64 and riscv64
  - minor cleanups
  - thinking about improvements of tail-call scheme; uweigand's suggestion from
    last week about pre-reserving arg space
    - thinking through interactions with stack-limit checks, making sure these
      include outgoing argument area
- avanhatt: no Cranelift updates; prepping for presentation on Veri-ISLE work
- alexcrichton: no Cranelift updates
- uweigand: no Cranelift updates
- cfallin: no Cranelift updates

- alexcrichton: Wasmtime table.init panic bug; fuzzing never hit it because
  inputs are always OOB during fuzzing; can we make fuzzing more effective?
  - currently we only choose instructions in wasm-smith based on what's on
    stack and what that allows; instead we should choose a candidate
    instruction first, then generate what is needed on stack
  - (looking at fuzzing coverage)
  - cfallin: have good coverage of individual operator cases, not necessarily
    combination of ops. Can we build fuzzers that focus on runtime operations,
    "command-list pattern"-style?
  - jameysharp: use random bytes directly for wasm?
  - cfallin: chaos-mode for certain Wasm operations (semantics-non-preserving)
    in a mode to search for crashes? e.g. keep indices in-bound?
  - fitzgen: filed https://github.com/bytecodealliance/wasm-tools/issues/1484

- cfallin: any thoughts related to backdoor risk?
  - uweigand: no difference in source tarballs from git, right?
  - alexcrichton: cargo-vet metadata (missed some details)
  - most risk probably from upstream deps that run during our build.rs; could
    theoretically inject something
  - cfallin: wasmtime-cache has ability to turn filesystem write into code
    execution (we trust what we read from disk); is this a concern? (unsure)
  - jameysharp: zstd compression issues
  - alexcrichton: one takeaway: no binary blobs or generated code unless it can
    be verified/regenerated and is checked in CI
