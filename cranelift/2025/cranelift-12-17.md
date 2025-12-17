# December 17 project call

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

- erikrose
- bjorn3
- fitzgen
- cfallin
- uweigand

### Notes

- Updates
  - bjorn3: no updates
  - erikrose: no updates, starting to work on dead-load-with-context instruction
  - cfallin: debugging work: patchability as an aspect of all calls
  - fitzgen: thinking about egraph extraction and handling shared structure
    - remove fixpoint loop on cost computation / extraction by doing a toposort
      first is cost neutral -- why not do this?
    - then once we do that, can we have incremental cost updates once we use a
      value (so further uses are cheap/free)?
    - cfallin: stats on out-of-order values / multiple fixpoint loop passes?
      - fitzgen: can look at it; expect more than one even in relatively common
        cases because of legalization
  - uweigand: no updates
