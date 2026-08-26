# August 26 project call

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
- adamrk
- fitzgen
- alexcrichton
- avanhatt
- uweigand
- Yage Hu
- cfallin
- jlb6740

### Notes

- Updates
  - erikrose: aarch64 implementation of dead-load-with-context to come
  - adamrk: no updates
  - alexcrichton: no updates
  - avanhatt: reviewing verifier-related PRs
  - uweigand: no updates
  - Yage: PhD student working on Cranelift, no updates
  - jlb6740: flipped APX add instruction patch to non-draft; waiting for review
  - cfallin:
    - verifier in CI; 3h58m
    - alias analysis issue this morning -- push for topics below
  - fitzgen:
    - working on a new register allocator, in the core of the allocation loop
      right now

- APX
  - fitzgen: how are we representing registers? Limit constraint? (cfallin: yes)
    - jlb6740: initial patch doesn't use extended registers; need to update
      more

- alias analysis
  - 14210, 14211
  - two issues, describe slightly different things
  - density thing: sparse data structures for regions?
    - but they flow all the way to the end
    - make sure we remove regions in DCE
  - limit number of regions we generate? or hash down to fixed 2^N regions?

- alexcrichton: fitzgen: describe a bit more the motivation for new regalloc
  project?
  - regalloc is still the biggest lever for both compile time (still ~70%) and
    codegen (spills are important in lots of real code)
  - cfallin's earlier ideas about a region-based register allocator: allocate
    inner loops in isolation (inspired by gcc's region-based alloc)
  - based on a mix of regalloc2, regalloc3, and LLVM within a region
  - mostly a project to try to explore the "regions" idea -- should help both
    runtime and compile-time, hopefully against all of these datapoints
