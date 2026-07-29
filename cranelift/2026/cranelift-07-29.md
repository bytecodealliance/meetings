# July 29 project call

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
    1. fitzgen: sightglass and PCA presentation
    1. cfallin: instruction selection/fusion for add-with-overflow + branch
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- fitzgen
- alexcrichton
- erikrose
- Adam Bratschi-Kaye
- cfallin
- avanhatt
- cpetig
- jlb6740

### Notes

- fitzgen: Sightglass PCA minimization
  - slides: https://docs.google.com/presentation/d/1Lf3eHpyrgOzn-7BqhChe5UepGZIjc98WuuKahBB3Eb8/edit?slide=id.p#slide=id.p
  - seems to capture direction of compile-time changes accurately
  - lots of discussion about mixed execution-time approximation
  - end conclusion: the current suite's weighting of each principal component
    is arbitrary anyway; we take eigenvectors not eigenvalues so our signs may
    be different but the important thing is that we span the same space
  - how do we choose what we weight? ultimately human judgment
  - alexcrichton: is this a one-time decision or do we re-weight/re-subset periodically?
    - fitzgen: only will really change if we add/remove benchmarks from the suite
    - cfallin: stability for decision basis -- important that it only really
      changes if the thing that we value and optimize for changes
  - what about new benchmarks?
    - cfallin: "orthogonality" test: show that it's distinct from exsting ones
      when adding
  - fitzgen: propose that we don't weight clusters (because it's arbitrary) and
    we do a manual review of each cluster. schedule a meeting to do this among
    interested parties
    - alexcrichton: not sure about "no weights" -- may want to change
    - fitzgen: if you ignore the average, there's no weighting; only comes up
      with mixed results
    - alexcrichton: another test for buckets: does everything in a bucket move
      in the same way
    - folks interested: cfallin, alexcrichton, fitzgen

- fitzgen: discussion about dead-store elimination bug

- alexcrichton: arm32 backend
  - cpetig: working on integration of work into Obei's branch; qemu testing
