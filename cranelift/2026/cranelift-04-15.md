# April 15 project call

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
- cfallin
- erikrose
- avanhatt
- alexcrichton
- fitzgen
- thejimmybrisson
- jlb6740

### Notes

- Updates
  - thejimmybrisson: merged s390x-z17 updates; fixed some security issues in
    s390x backend as well
  - avanhatt: work on upstreaming the backend verifier
  - alexcrichton: bugfixes from LLM-found bugs; thinking about riscv64
    refactorings long-term
  - erikrose: working on pagemapping-powered epoch switching in Wasmtime
  - cfallin: backed out an egraph blowup case in ruleset; not root-caused yet
  - bjorn3: no updates
  - jlb6740: no updates
  - fitzgen: have a branch generalizing alias regions, then using that in
    Wasmtime for each different memory, table, global, every vmctx field;
    somehow a 12% slowdown? haven't looked into it yet, but built a hotblocks
    tool in wasmtime to look at hot basic blocks

- avanhatt: question about integer type converters and codegen
  - fullness of time, we should use Imm64-plus-type stuff that Bongjun recently
    contributed

- cfallin: blog post about aegraphs, if anyone curious/wants more detail on how
  it works
  - https://cfallin.org/blog/2026/04/09/aegraph/
