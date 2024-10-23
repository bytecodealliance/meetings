# October 23 project call

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

- alexcrichton
- avanhatt
- abrown
- cfallin
- mmclouoghlin
- uweigand

### Notes

- Updates
  - alexcrichton: no updates
  - avanhatt: isle-veri updates
    - working on encoding rule priorities in ISLE in the verification SMT
      lowering
    - issues with isle-veri approach of not expanding internal extractors and
      seeing opcodes
  - abrown: new assembler
  - mmcloughlin:
    - starting to spec vector instructions on aarch64 too; needed for popcount
  - uweigand: no updates
  - cfallin: no updates

- abrown: new assembler for Cranelift
  - in the past, one giant Inst enum with classes of instructions corresponding
    to inst formats
    - but starting to break down: many many enum variants, and also not all
      opcodes are valid in all variants
    - also, new ISA extensions (APX and AVX10) add a lot of new encoding work;
      can we do better?
    - can we generate enum and assembler from a table in a text file?
    - better maybe: Rust DSL in meta
  - sketch of Rust DSL
    - gives encoding, pretty-printing, regalloc, ISLE bindings
    - test against an external assembler?
  - Q: gradual migration?
    - can merge enums at ISLE level; matching code we might be a little hacky
      (include! etc) but could work
  - cfallin: Qs:
    - pseudo-insts? sequences?
      - abrown: make one abstraction for this?
        - `Sequence` inst, derive metadata from that, put seq build in ISLE
    - attach other data -- specs?
  - mmcloughlin: can we reuse another assembler?
    - abrown: problem is we still have to generate Cranelift-specific metadata
    - cfallin: build times and dependencies are also a major issue; also slight
      semantic gaps (e.g. 3-op vs 2-op form)
  - does this work for isle-veri?
    - avanhatt: this format is much nicer to attach metadata/specs to
