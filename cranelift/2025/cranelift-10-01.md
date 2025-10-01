# October 01 project call

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

- bjorn3
- alexcrichton
- fitzgen
- Rahul
- cfallin
- Amanieu
- jlbirch

### Notes

- Amanieu: no updates
- Rahul: working on PR to use SDE (Intel's emulator for recent ISA
  enhancements) into CI
  - fitzgen: compare runtimes vs. qemu?
  - Rahul: longest workflow is around 25 minutes, most are 10-13 minutes; will
    get more data later
  - alexcrichton: starting point would be just filetest tests
- alexcrichton: no updates
- cfallin: debug-tags support in Cranelift in support of instrumentation-based
  debugging in Wasmtime
  (https://github.com/bytecodealliance/wasmtime/pull/11768)
- bjorn3: landed + backported fix and updated `cg_clif` to latest Cranelift;
  now cleaning up instruction-feature flags (thanks!)
- jlbirch: no updates
- fitzgen: no updates

- debug tags
  - (lots of discussions of #11768)
  - TODOs:
    - put sequence point instructions back -- clarifies semantics on which
      insts can carry and preserve tags
    - single u64 identifiers for frame layouts rather than blob of bytes
  - discussion about breakpoint options -- patchable code vs. redirectable
    branches; don't tempt concurrency bugs by modifying one copy of code, use a
    private copy and patch
