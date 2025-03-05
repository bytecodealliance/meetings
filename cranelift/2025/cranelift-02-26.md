# February 26 project call

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
- fitzgen
- alexcrichton
- Amanieu
- uweigand
- abrown
- cfallin

### Notes

- Updates
  - alexcrichton:
    - worked on refactoring some aspects of the assembler; good discussion on
      design
      - refactoring: https://github.com/bytecodealliance/wasmtime/pull/10276
  - fitzgen: no updates
  - uweigand: some progress in Rust on s390x -- feature-detection macros, can
    use once available
  - cfallin: participated in assembler discussions above
  - erikrose:
    - looked at RA3; overall seems like a wash; some better some worse
    - Amanieu: using `split` branch?
    - erikrose: whatever `regalloc3` branch of Amanieu/regalloc2 uses
    - Amanieu: just yesterday merged new liverange splitting; experiencing 5%
      speedup, have some compile-time regressions
    - erikrose: test failures from last time
    - Amanieu: some things (debug annotations?) not supported yet
  - abrown:
    - continuing work on assembler, discussed ISLE integration with Alex, ok
      with Alex's refactoring PR
    - need more of a layer between assembler and Cranelift; put this in
      cranelift-codegen-meta
  - Amanieu:
    - working on RA3 again; working on compile-time; currently 50% slower in
      compile time

- x64 assembler: discussed conclusions in
  https://github.com/bytecodealliance/wasmtime/pull/10276
  - `ProducesFlags`/`ConsumesFlags` is awkward -- better ways?
    - cfallin: flags-as-values -- idea mentioned at end of that thread. We
      could represent flags dependencies, and in the lowering panic if flags
      are clobbered (rather than try to regalloc/spill/reload/schedule
      intelligently)
    - don't particularly like the panic -- more error-prone than the
      correct-by-construction solution we have now
    - do instruction scheduling? too complex
  - cfallin: pseudoinsts for all N combinations we have, and just emit those?
    simple, also correct by construction
     - alexcrichton: potentially less flexible if anything is done
       programmatically (dynamic sequence of `adc`s for example, or plugging in
       one half from one source to another elsewhere -- we'd have to enumerate
       all the combinations)
    - cfallin: sub-idea: generic list of insts (as in s390x); emitted
      atomically. We can have helpers that carefully put consumers and
      producers together; then the rest of the ISLE treats this as one unit
