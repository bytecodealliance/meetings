# November 20 project call

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

### Notes

Updates
- alexcrichton: looking at Pulley
- abrown: new assembler. Working through integration with ISLE. Also nervous
  about amount of work.
  - Might need more help; from verification folks maybe?
- avanhatt:
  - verification project did not submit to PLDI; thinking through next
    steps.
  - Might be interested in helping with assembler work.
  - Have a student interested in working on mid-end verification. Mismatch: mid-end CLIF terms
    have type as argument. Can we update ISLE backends to include this? (Yes, in
    favor).
- cfallin: isle-in-source-tree feature
  - https://github.com/bytecodealliance/wasmtime/issues/9625
  - per bjorn3: was enabled by Wasmer unconditionally, caused folks' cargo cache
    directories to get this ISLE source path that was only ever supposed to be used in
    a local dev tree
  - let's remove the feature

- alexcrichton: in Pulley, thinking of using macro that generates instruction
  definition to generate ISLE too
