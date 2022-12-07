# December 7 project call

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
- fitzgen
- uweigand
- afonso360
- bjorn3
- jameysharp
- cfallin
- jlbirch


### Notes

- Status
  - elliottt
    - SSA verifier enabled in RA2; fixing various issues discovered
      - yesterday, SSA verifier passing
  - cfallin
    - egraphs merged! rollercoaster of perf results; 7.7% on
      accidental-explicit-bounds-checks baseline, in the noise after merge, but
      now with more low-hanging fruit addressed, SpiderMonkey is ~16% faster
      runtime
    - added to fuzzing; watch to make sure stable; eventually on by default
  - bjorn3
    - Thanks to Afonso cg_clif now tests s390x support on CI:
      https://github.com/bjorn3/rustc_codegen_cranelift/pull/1304
  - afonso360
    - cg_clif working on s390x!
    - will fuzz egraphs
  - uweigand
    - no updates; thanks to others for keeping s390x happy
  - jameysharp
    - ISLE codegen revamp; 20% shorter generated Rust code, haven't run yet
      though (need multi-ctors, multi-etors)
    - term declarations: "partial" flag (#5392)
      - means that the term may not match
      - applies to internal and external constructors
      - if an internal ctor is not partial, it panics if no rule matches
      - means that return is not `Option` so more efficient
      - makes codegen revamp more feasible
    - cfallin: are `lower` entry points partial?
      - jameysharp: yes, and `lower_branch`
        - cfallin: makes sense; lower is partial because of migration to ISLE,
          but failure to produce a value in RHS of some other term should
          probably be a panic
    - uweigand: in s390x, some partial subterms?
      - jameysharp: yes but in LHS; failure in RHS is what this covers
  - jlbirch
    - no updates
  - fitzgen
    - cleaning up + optimizing explicit bounds checks
      - draft PR to remove concept of "heaps" from CLIF itself, pushes into
        Wasm translator
        - not ready to merge yet: need a way to port tests over (currently CLIF
          tests)
