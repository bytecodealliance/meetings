# April 22 project call

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
- thejimmybrisson
- alexcrichton
- bjorn3
- uweigand
- cfallin
- avanhatt
- jlbirch
- fitzgen

### Notes

- Updates
  - erikrose: no updates
  - thejimmybrisson:
    - refactored s390x to take into account types in CLIF terms
    - ISA flag checks built in to LHS? --> use if-let instead; that idiom
      predates if-let
  - alexcrichton: no updates
  - uweigand:
    - reviewing Jimmy's patches, look good
    - reviewer setup to get reviews for s390x
  - avanhatt: continuing to work on upstreaming verification work; adding specs
  - bjorn3: no updates
  - jlbirch: no updates
  - fitzgen:
    - thinking about load/store optimizations: we could do a subset of
      dead-store elimination. We don't do the full thing today because complex
      -- need to track if postdominated by another store. But a specific case
      we can handle comes up in component-model adapters where we have two
      stores with the same data (idempotent stores).
  - cfallin: no updates
