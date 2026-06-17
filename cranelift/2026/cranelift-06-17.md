# June 17 project call

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

- thejimmybrisson
- alexcrichton
- erikrose
- bjorn3
- jlb6740
- cfallin
- fitzgen

### Notes

- Updates

  - thejimmybrisson: no updates
  - alexcrichton: no updates
  - erikrose: getting close to finishing Wasmtime MMU-based tricks for faster
    epoch checks, which includes a new CL instruction
  - bjorn3: no updates
  - jlb6740: no updates
  - fitzgen: whittling down uses of `global_value` instruction; want to remove
    it. Then finally we can remove legalization; call the "rewrite condbr to trap
    into trapz/nz" pass another pass. Maybe can also fold it in. Also have been
    adding alias regions to everything in Wasmtime; no CL-specific issues.
  - cfallin: no updates

- Alias regions
  - backporting commit series for alias regions to Wasmtime 46? Fixes inline
    memcpy (didn't use alias regions) -- 13610. May not fully backport, just
    fix bug on release branch.

- GVs and stack limits -- where else are GVs used?
  - dynamic types too (cfallin: but, could talk about removing this -- was
    never finished by the folks working on it)
  - in theory we definitely need GVs for anything needed in prologues
  - (lots of discussion about dynamically-sized vectors in the abstract)

- big diffs with snapshot changes -- anything to make review better?
  - smart matchers
    - but those are worse -- have to update manually
    - and judgment calls on whether diffs *are* accepted -- review needs to see
      diffs, can never make a perfect oracle that accepts acceptable changes
      only
  - normalization/canonicalization
    - "compacting GC", removing addresses, etc
