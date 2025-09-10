# September 10 project call

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
- abrown
- cfallin

### Notes

- Updates
  - abrown: no updates
  - alexcrichton: landed change to use exceptions rather than setjmp/longjmp
    - this does mean we're committing to exception ABI restoring all registers;
      all-clobbers may be seen as slow eventually
      - cfallin: two ABIs, one witih all-saves for entry and one for more
        efficient within compiled code (but this means we need two layers of
        trampolines?), or save-all-regs intrinsic in trampoline
      - fitzgen: two ABIs doesn't need two trampolines, just make the
        save-everything one compatible
    - alexcrichton: ideally, raise libcall also lets Cranelift do actual resume
  - fitzgen: working on the way we handle metadata for compiled functions in
    Wasmtime; use FuncKey uniformly; data structure tradeoffs on sparse vs
    dense FuncKey space in lookups
    - refactor makes lots of things more uniform when doing compile-time
      builtins; also may enable GC'ing of functions more easily later
  - jlbirch: no updates
  - cfallin: debugging RFC; starting work on instrumentation for value tracking
    - (more discussion about debugging adapter components; will bring
      conversation to Wasmtime meeting tomorrow)

- abrown: ISLE optimizer rule additions -- raises question of continuous
  benchmarking -- are we still interested in doing this?
  - yes, but no one has bandwidth to regularly monitor right now
