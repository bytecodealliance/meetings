# May 20 project call

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

- Jacob Denbeaux
- fitzgen
- thejimmybrisson
- jlb6740
- cfallin

### Notes

- Jacob: no updates
- cfallin:
  - MachBuffer deadline fix: allow islands in middle of blocks, and reason
    about additional deadline pressure any single instruction can add; allows
    us not to have to do a big sort across island contents
  - aegraph fuel fix (prevent blowup when Cartesian product on LHS gets large,
    independently of individual eclass size limits and rewrite depth limits)
- thejimmybrisson: no updates
- jlbirch: Intel folks thinking about APX enablement; digging into plan in more
  detail
- fitzgen: landed branch simplification mid-end rewrite rules
  - initially tried to do things online during egraph build pass, tracking
    reachability; didn't work with irreducible CFGs
  - instead, now, quick pass after egraph pass and before elaboration to remove
    unreachable code
  - still hoping to do Sightglass PCA and trim down benchmark suite eventually
  - still working on alias regions, rebasing/fixing branch currently

- Discussion about mid-end opts and fuel
  - thejimmybrisson: does this affect backend at all?
  - no: extraction limits backend to single operators per value, not eclasses
  - discussion about longstanding idea to fuse mid-end and backend by letting
    backend see eclasses and be an "implicit cost function"
