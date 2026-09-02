# September 02 project call

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
    1. Improving reuse of allocations between functions, reducing large struct movements (@alexcrichton)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- adamrk
- thejimmybrisson
- fitzgen
- cfallin
- alexcrichton
- avanhatt

### Notes

- Updates
  - adamrk: no updates
  - thejimmybrisson: no major updates; working on updating Capstone to support
    new s390x instructions
  - alexcrichton: no updates
  - avanhatt: reviewing PRs with new verifier; planning to print
    counterexamples in CI
  - fitzgen: we discovered that alias analysis state is not a true lattice;
    merge points froze an identifier based on first path taken. Also have a
    patch to limit number of alias regions to keep costs down.
  - cfallin: we have verification at home! (on by default in CI). Thanks to all
    collaborators for writing the thing, I just turned it on in CI. Also
    uadd-overflow / umul-overflow and opportunistic defs PRs; closes the thread
    on discussion from a month or two ago.

- alexcrichton: looking at improving compilation time in general. MachBuffer --
  very large, moved because of typestate.
  - discussion about where the costs are; memcpy (from data movement) vs
    allocator. Alex indicates profiling shows ~all the cost is in memcpy.
  - should we do a refactor and use dyn tricks to hide the VCode type
    parameters and typestate parameters behind something that can reuse
    allocations? Or just ensure that we don't move the MachBuffer (box the
    parts that are invariant across typestate)? Seems let's try the latter for
    now -- will likely get 99% of the benefit (according to profile) and
    simpler
