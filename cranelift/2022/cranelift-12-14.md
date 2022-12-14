# December 14 project call

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

- elliottt
- cfallin
- fitzgen
- uweigand
- afonso360
- bbouvier

### Notes

- elliottt
  - brif: two-target conditional branch
    - can eventually allow us to not need to split edges out of `br_table`s
  - s390x regalloc issue
- afonso360
  - multiple functions and calls between functions to fuzzgen
- cfallin
  - egraphs: backend fuzzbug exposed by opt rule; fixed
- uweigand
  - removed iflags/fflags. Insns that consume these values already gone, just
    removing types and producers (ifcmp etc)
  - removed `MachInst::gen_constant`, and delegated to ISLE implementation of
    constant lowering
    - required some refactoring: lower impl returns result register, doesn't
      set up aliasing to inst output
    - backend glue/adapter code is now completely gone: LowerBackend::lower is
      just direct delegation to ISLE constructor (nice!)
  - regalloc rules written down somewhere?
    - cfallin: will do this
  - fitzgen: followup cleanup: can we remove difference between IsleContext and
    LowerContext?
    - uweigand: also has reference to LowerBackend; if we can unify these then
      maybe
- bbouvier
  - no updates, but!:
  - new contributor for: profiling, debugging?
    - debugging: no support right now on Windows, and on Linux hits an assert
    - cfallin: good to have discussion or RFC process on this: what are
      requirements? wasmtime under gdb vs gdbserver impl, how to handle
      Windows, modify codegen to expose more values (don't optimize away) vs
      not, ...
- fitzgen
  - removing heap support from cranelift-codegen (and lifting out to
    cranelift-wasm)!
    - separate tests out and merge first, so we can see changes from main PR
  - then opts on explicit bounds checking: ensure GVN works, then reason about
    value ranges
