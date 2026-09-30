# September 30 project call

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
- fitzgen
- erikrose
- alexcrichton
- adamrk
- cfallin
- avanhatt
- bjorn3
- Jacob Denbeaux

### Notes

- Updates
  - erikrose: MMU interruption has `dead_load_with_context` instruction;
    looking for better name.
  - alexcrichton: no updates
  - adamrk: no updates
  - avanhatt: no updates
  - bjorn3: no updates
  - fitzgen:
    - interesting article: Nicholas Nethercote speeding up rustc compilation of
      `cranelift-codegen`
    - have new register allocator passing all WAST tests; will get initial perf
      numbers soon
    - improvement to alias analysis: blocks with single preds inherit state
      from actual end of predecessor block, rather than result of analysis --
      accounts for rewrites that happen
    - PR to do budget-based PCA on Sightglass
    - planning to do an AreWeFastYet-like website
  - thejimmybrisson: PRs to add events so Sightglass will work on s390x
  - Jacob: no updates
  - cfallin: no updates
