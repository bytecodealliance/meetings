# May 28 project call

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
- erikrose
- fitzgen
- alexcrichton
- abrown
- Rahul Chapulkar
- cfallin

### Notes

- cranelift-{object,jit,...} -- maintainership?
  - `cg_clif` maintains -- mostly not a burden; useful to have in-tree, good to
    keep up-to-date alongside (monorepo)
  - this topic spawned by someone opening security issue on internal Wasmtime
    crate -- worth talking about "internal-ish" crates
  - we can add some notes to readme

- Updates
  - Rahul: moving instructions to new assembler
  - abrown: likewise, working on x64 assembler
  - erikrose: no updates
  - cfallin: no updates
  - alexcrichton: no updates
  - bjorn3: exception handling branch for `cg_clif` is ready to merge; working
    on blog post about it
  - fitzgen: no updates

- alexcrichton: x64 assembler: build a parser that parses literally the
  hex/encoding string and drives the DSL?
  - fitzgen: downside is we'd have to do this at build time
  - maybe fine? we can try it
  - cfallin: maybe focus more energy on this once we have the whole assembler
    migrated?

- cfallin: generate a dissassembler too?
  - remove Capstone dependency for Wasmtime (still fuzz against in fuzz targets)
  - would reduce build time for x64-only (but we'd need assembler for all
    targets before removing the dep overall)
  - nice to have eventually

- does new assembler approach make sense for new architectures
  - abrown: not sure about complexity in other ISAs -- do they need it?
  - cfallin: yes -- aarch64 has lots of inst formats, and complexity, too, for
    example; s390x is variable-length; ...
  - alexcrichton: fuzzing etc is nice too
  - cfallin: what can be generalized if anything?
  - fitzgen: put the gpr/vec newtype split in other backends as part of this
    transition?
    - cfallin: big refactor, 10k lines of ISLE etc
    - fitzgen: maybe gradual with unsafe coercions

- abrown: thoughts on Gpr/XMm refactor?
  (https://github.com/bytecodealliance/wasmtime/pull/10775)
  - push distinction into MachInst/machine-independent layer? maybe not
  - push more into Rust? All of x64 backend use newtypes ideally
  - (discussion about cleaning up APIs and pub exports, etc)
