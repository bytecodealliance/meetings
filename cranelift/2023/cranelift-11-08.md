# November 08 project call

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

### Notes

#### Updates:

- saul: More multi-value. Most implemented, but spec tests aren't that complete,
  so fuzzing to build confidence. Fuzzing found issues, which is good progress.
  Some issues are multi-value, some aren't. Working on this until fuzzing stops
  finding issues.
- alex: no updates
- jamey: no updates
- ulrich: no updates
- rahul: no updates
- nick: mostly on wasm GC stuff, also exploring ISLE rules. Wanted to make a
  rule for chains of adds to reassociate and expose instruction parallelism.
  Turns out egraph wasn't actually doing some egraph things, union nodes weren't
  encoded correctly. Instead of unions of multiple values we used an alias of
  the most recent rewrite. Various issues with blowup when fixing this. Some
  rules match 1000s of times. Issue detailing more.
- afonso: Egraph rules for shifts and rotates, some RISC-V instruction selection
  improvements.
