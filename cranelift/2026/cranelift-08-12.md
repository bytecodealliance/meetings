# August 12 project call

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
    1. cfallin: instruction selection/fusion for add-with-overflow + branch
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- fitzgen
- alexcrichton
- adamrk
- cfallin
- jlb6740

### Notes

- cfallin: isel/fusion on add-with-overflow + branch
  - two basic approaches: peephole after-the-fact, or some value-lowering
    changes to fuse the add-with-overflow to branch and say "opportunistically
    we have this other value too"
  - alexcrichton: peephole worrisome because of tracking uses of values
  - fitzgen: you need predicates for this but you can reason about it (no other
    uses of flags boolean, etc)
  - (lots of discussion about the details of how peephole might work)
  - ultimate issue: peephole has issues with instructions in between, whether
    or not they use the sum output of add-with-overflow; and we suspect code
    motion might even be likely to produce such instructions, e.g. because of
    demand-based materialization on block args of the branch, or LICM, or ...
    So: cfallin will prototype the value-lowering thing to see how it is.

- Updates
  - adamrk: no updates
  - alexcrichton: no updates
  - fitzgen: a few PRs up to do adapter optimization
    - dead-store elimination, ...
    - playing with regalloc -- region-based, from cfallin's ideas
  - cfallin: code-review
  - jlb6740:
    - resolved MPK bug (Wasmtime), restarted work on APX support, expect add
      instruction this week
