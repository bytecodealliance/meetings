# August 21 project call

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

- abrown
- avanhatt
- elliottt
- alexcrichton
- fitzgen
- cfallin

### Notes

- Updates
  - alexcrichton: no updates
  - elliottt: no updates
  - avanhatt: no updates
  - abrown: no updates
  - fitzgen:
    - new (user) stack maps now in use by Wasmtime; PR up to remove old
      (regalloc-based) stackmaps; merging soon, just some miri issues
    - alexcrichton: CI failures on PR to remove old stackmaps might be
      out-of-memory issues
  - cfallin: no updates

- alexcrichton: heads-up: LLVM 19 has enabled reftypes and multivalue features;
  changes `call_indirect` to make a literal 0 (as of Wasm MVP) a LEB128
  instead; LLVM inflates all LEB128s to 5 bytes
  - mainly an issue when we disable reference types; disables feature in
    wasmparser
  - https://github.com/WebAssembly/tool-conventions/issues/233
  - question of what we do with tests and validation
    - upstream `main` and older branches don't agree, issue when we run on
      other branches / work on other proposals
  - Q: when do we remove support for disabling a proposal?
    - fitzgen: clarification: stage 4 proposals still have tests from upstream
      that are old?
    - alexcrichton: old proposals have old snapshots of upstream test suite
    - cfallin: feels like an issue with upstream organization; how does it
      interact with wasmtime use of spec tests?
    - abrown: test-ignores are tricky to keep up to date
  - cfallin: cleave the support flags in two, reftypes and GC separately?
    - LLVM is technically generating code that uses the reftypes proposal now
  - fitzgen: would prefer to fix the parser to be more permissive and ignore
    some tests
  - alexcrichton: invent a flag for just externrefs
  - fitzgen: (more discussion about idea above to split new tests out more
    cleanly)
