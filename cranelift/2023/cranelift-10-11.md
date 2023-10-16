# October 11 project call

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
- afonso360
- avanhatt
- cfallin
- elliottt
- jameysharp
- saulcabrera
- simonas
- ulrich

### Notes

- cfallin:
  - working on proof-carrying code
  - add "facts" about SSA values
  - annotate code produced by cranelift-wasm
  - working with fitzgen
  - hoping to have something that can verify at least static memories within a few weeks

- saulcabrera:
  - working on multi-value
  - refactorings from last week's work on tables

- ulrich:
  - before vacation, got two PRs merged for issues encountered while trying to implement tail calls
  - tail call convention needs a frame pointer register but other ABIs don't on s390x
  - separate planning of frame layout (`compute_layout`) from implementation
  - onward to more tail calls

- afonso360:
  - cg-clif issues with TLS values
  - existing issue that CLIF doesn't support specifying alignment of stack slots
  - jameysharp suggested we could pass heap allocations into fuzzer functions to test non-stack memory accesses
  - we had that for runtests and removed it because it was awkward

- avanhatt:
  - submitting our paper artifacts and working on getting stuff upstreamed
