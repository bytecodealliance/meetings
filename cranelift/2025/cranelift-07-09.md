# July 09 project call

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

- bjorn3
- fitzgen
- abrown
- cfallin
- Amanieu
- uweigand
- jlbirch

### Notes

- Updates
  - abrown: no updates
  - bjorn3: PR for adding support to cranelift-module; looking into incremental cache
  - Amanieu: regalloc3 has benchmarks, is done and ready for review
    - writing up design doc
    - cfallin: PRs for Cranelift, regalloc2 backend (already there), PR for
      regalloc3 codebase; will review
    - abrown: maintainership? Is there someone else who could step up to handle it?
      - cfallin: should be me, but not comfortable yet -- need to review codebase
    - cfallin: RFC?
      - alexcrichton: would be good to have to lay out the transition plan,
        more background on why and what the differences are, etc.
    - uweigand: would be interesting to know if there are additional
      capabilities -- register pairs, subclasses, etc.
      - Amanieu: yes, both of those are supported
  - alexcrichton: no updates
  - jlbirch: no updates
  - uweigand: tracked down s390x fiber inline assembly bug -- register class
    for argument (`reg` vs. `reg_addr`)
  - fitzgen: working on an inliner, have a new filetest mode for it, PR soon
    - bjorn3: what kind of API does this expose?
    - fitzgen: provides the mechanism to do an inlining operation at a function
      callsite; no heuristics. Hopefully usable by `cg_clif` as well
  - cfallin: no updates
