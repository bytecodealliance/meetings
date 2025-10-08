# October 08 project call

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

- erikrose
- bjorn3
- fitzgen
- alexcrichton
- Rahul
- cfallin

### Notes

- Updates
  - Rahul: PR to merge SDE (emulator for new Intel instructions for CI)
  - Rahul: no updates
  - alexcrichton: no updates
  - cfallin: merged debug tags in Cranelift; working on Wasmtime side of debug
    now (PR up for instrumentation, currently traps-into-yields)
  - erikrose: no updates
  - jlbirch: no updates
  - fitzgen: working on finishing compile-time builtins
  - uweigand: Wasmtime repo enabled for GitHub Actions with native s390x
    runners

- (discussion about moving Wizer into Wasmtime)

- fuzzing issues where CLIF interpreter's libcalls don't match Wasmtime's
  - NaN canonicalization should always run -- seems like part of the frontend
  - use the same libcall impls in all places (`wasmtime-math` crate)
