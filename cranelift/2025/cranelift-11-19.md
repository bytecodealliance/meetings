# November 19 project call

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
- fitzgen
- Amanieu
- cfallin
- jlbirch

### Notes

- Updates
  - alexcrichton: no updates.
    - Q: has anyone used Cranelift Bugpoint? Unmaintained dependency of its
      progress-bar (we should remove).
      - updating deps in general: should we `cargo update` regularly?
        - cfallin: how do we prefer others consume our crates? keep up to date,
          no?
        - fitzgen: cargo-vet burden
        - alexcrichton: good to exercise the muscle; also we're a dependency of
          others so it's more on us to keep our deps up to date; also sometimes
          hit bugs fixed in newer versions of deps
  - cfallin: a little time on debugging -- private-copy-of-code
    - also ported four benchmarks from JetStream to Sightglass
  - Amanieu: fuzzbug on RA3
  - jlbirch: no updates
  - fitzgen: blog post about function inlining published on BA + personal blog
