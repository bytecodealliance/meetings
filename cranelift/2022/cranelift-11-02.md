# November 2 project call

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
    1. fitzgen: feedback to users that something is happening when running sightglass
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- cfallin
- fitzgen
- elliottt
- avanhatt
- uweigand
- abrown
- saulcabrera
- jameysharp
- bjorn3
- afonso360
- jlbirch

### Notes

- Progress reporting in Sightglass
  - two PRs -- abrown's, fitzgen's
  - abrown: one-line patch (fitzgen's) is fine

- Status
  - abrown: nothing
  - saulcabrera:
    - work on cranelift-asm (splitting off assembler layer)
    - small blocker: how to handle cross-crate ISLE dependencies
      - https://github.com/bytecodealliance/wasmtime/issues/5178
    - for now, we can depend on cranelift-codegen in winch-codegen
  - jameysharp
    - ISLE compiler, codegen strategy
    - should sync up with verification re: use of sema IR in islec
      - avanhatt: isle-veri uses slightly modified version of sema
  - bjorn3: nothing
  - cfallin
    - lots of code review and guidance; IC work is mostly in another project at
      the moment; some ideas on egraphs, back eventually
  - elliottt
    - SSA purity in codegen -- return values
    - remove trapif / trapff
  - avanhatt
    - getting closer to Wasm-MVP-equivalent footprint in ISLE verified
    - some questions around value representations for enums
  - uweigand
    - bitcast vs raw_bitcast
      - merged raw_bitcast cases into bitcast in backends. Two cases: same
        regclass (no-op), different regclass (cross-regclass move)
      - one new case -- i128 <-> v128 -- now possible/allowed, but not
        implemented yet
      - fitzgen: update CLIF verifier as well?
        - uweigand: yes, updated
  - fitzgen
    - compilation-time speedups
      - 2% on removing ensure\_struct\_return...
      - RA2: iteration rather than indexing
  - afonso360
    - iadd_carry
  - jlbirch
    - ready to merge benchmarking format patch

- ISLE cross-crate dependencies (cranelift-asm needs ISLE enum for MachInst)
  - two approaches: ISLE compiler aware of imports, or we do it beneath its
    awareness and generate variant that has "extern" keyword
  - issue with source-level dependencies across crates
  - manual sync?
  - maybe better to depend on table generation to produce ISLE enum defs --
    then create two versions
  - fitzgen: create cranelift-asm now, re-export pieces of cranelift-codegen

