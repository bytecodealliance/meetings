# June 10 project call

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
- posborne
- thejimmybrisson
- bjorn3
- cfallin
- Jacob Denbeaux
- fitzgen

### Notes

- Updates
  - alexcrichton: no updates
  - posborne: reviewing PCA changes on Sightglass
  - cfallin: no updates
  - thejimmybrisson: no updates
  - Jacob Denbeaux: no updates
  - bjorn3: no updates
  - fitzgen:
    - removed all `*_imm` instructions and added `InstBuilder` auto-expanding
      builder magic to replace them, and `b{and,or,xor}_not` instructions. Goal
      is to remove legalization pass entirely. Only thing left is global-value
      instruction and the branch-to-trap-block rewrite.
      - some discussion about branch-to-trap-block, where it comes from (Wasm
        shapes), how to opt it -- should remain a separate pass
    - alias regions generalization landed
    - Sightglass: added a measure for Callgrind; should give a
      deterministic-ish number for performance (simulating a cache, branch
      predictor, etc)
    - Sightglass: put up a PR to do principal component analysis (based on
      features: static/dynamic instruction mix and other stats) to narrow down
      benchmark suite

- no-std for rest of Cranelift -- opinions? (incoming PR)
  - fine if it doesn't create too much pain
  - cranelift-jit (OS interface) might be the only weird part
