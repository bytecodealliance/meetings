# November 06 project call

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

- cfallin
- alexcrichton
- avanhatt
- mmcloughlin
- fitzgen
- abrown
- Rahul

### Notes

- Updates
  - mccloughlin: new verification effort found a bug! trapping on `INT_MIN` for
    sdiv 8-bit case on aarch64. Not reachable from Wasmtime or `cg_clif`. Not
    security issue. Shows verifier can reason about traps, flags, etc.
  - avanhatt: that bug, and also getting FP verification to work
  - abrown: no updates
  - Rahul: no updates
  - alexcrichton: no updates
  - cfallin: no updates
  - fitzgen: no updates
