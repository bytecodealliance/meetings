# October 30 project call

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

- fitzgen
- alexcrichton
- avanhatt
- mmcloughlin
- abrown
- cfallin
- jlbirch

### Notes

- Updates
  - abrown: no updates
  - avanhatt and mccloughlin:
    - updating new isle-veri approach with auto-derived instruction specs to
      get parity with first paper's approach
    - verifying floating-point now!
  - alexcrichton:
    - fuzzing not happening right now, trying to work out oss-fuzz issues
  - cfallin: no updates
  - jlbirch: no updates
  - fitzgen: no updates

- mccloughlin: sdiv doesn't use `with_flags` in aarch64; should it?
  - yes! the abstraction was meant to be used everywhere?
  - is it a bug? not today, no; works because evaluation order pins things
    down; but footgun, and we should make it explicit

- alexcrichton: custom page sizes + shared memory has interesting fuzzbug
  - we should refine distinction from static/dynamic to const-base-pointer or
    not (new case: base pointer constant, but we still need to do bounds
    checks)
  - some other bits conflated as well, e.g. turning off signal handlers forces
    dynamic memories
  - in general: let's name things more consistently; const/not-const for base, limit
  - static and dynamic are confusing terms here too; "static" should mean no
    checks at runtime, but one can have a static memory today with dynamic
    checks for very large offsets
