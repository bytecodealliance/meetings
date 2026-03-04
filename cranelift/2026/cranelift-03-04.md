# March 04 project call

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
- fitzgen
- bjorn3
- uweigand
- jlbirch
- cfallin

### Notes

- Updates
  - uweigand: reviewing Jimmy's z17 patch
  - alexcrichton: no updates
  - cfallin: answering questions on Zulip about adding memory-ordering to
    atomics; no other updates
  - bjorn3: no updates
  - fitzgen:
    - using Cranelift bforest arena-based BTrees for fallible-allocation data
      structures in Wasmtime; extended with range queries, etc per those needs
    - thinking about mid-end optimizations: dead-store elimination, alias
      regions, bounds-checks, ... -- motivated by compile-time builtins in
      Wasmtime
      - bits for alias region
        - cfallin: we could get rid of load/store offset forms if we really
          needed to; in the mid-end and/or front-end we often go through a form
          with separate `iadd`s anyway
  - jlbirch: no updates
