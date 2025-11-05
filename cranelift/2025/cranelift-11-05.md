# November 05 project call

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
- bjorn3
- Amanieu
- uweigand
- alexcrichton
- cfallin

### Notes

- Updates
  - alexcrichton: no updates
  - uweigand: no updates
  - Amanieu: no updates
    - abrown mentioned when back from sabbatical may have time to review
      regalloc3
  - cfallin:
    - update on plan for debugging
      (https://github.com/bytecodealliance/wasmtime/issues/11964)
    - drafted a blog post on exception handling; posting soon
  - fitzgen: no updates, Wasm CG last week (reviewed exception handling post;
    thanks!)

- alexcrichton: debugging and threads
  - store will own a private copy; all's fine; stop-everything behavior is on
    the host if other stores/instances also have access to a shared memory

- Cranelift as a production rustc backend
  - main issue is performance with unwinding enabled
  - (discussion about performance implications of more blocks)
  - bjorn3: 7% compile-perf impact with unwinding
  - fitzgen: tried Cranelift inliner? bjorn3: no, haven't yet
  - bjorn3: rustc puts inlinable funcs in every codegen unit? possible to avoid
    maybe
