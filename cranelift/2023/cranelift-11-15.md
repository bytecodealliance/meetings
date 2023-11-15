# November 15 project call

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
- elliottt
- fitzgen
- jameysharp
- saulcabrera
- ulrich
- (I may have missed some attendees)

### Notes

- saulcabrera
  - landed the first piece of multi-value
  - fixing some bugs today
  - second (final?) piece is blocks

- alexcrichton
  - did a little legalization work but we'll talk about it in a later meeting

- avanhatt
  - the artifact for the verifier won a "Distinguished Artifact" award (congratulations!)

- abrown
  - worked on VTune integration
  - worked more on MPK issues we had in CI
  - diagnosed those issues and added a vendor-string check for "is it an Intel CPU" AND "does it have MPK enabled"

- fitzgen
  - working on year-end round-up blogpost for both Wasmtime and Cranelift
  - if there's anything you want mentioned, send me a message on Zulip
  - new egraph rules for balancing commutative expressions turned up issues
  - we weren't actually creating "union" nodes in the egraph, creating aliases for the RHS instead, so we weren't actually doing the egraph thing at all
  - some other ISLE changes too: allowing callers to pass in storage to reduce the number of vectors allocated

We're canceling next week's meeting due to the US Thanksgiving holiday
