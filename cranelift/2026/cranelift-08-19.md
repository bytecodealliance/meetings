# August 19 project call

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

- fitzgen
- adamrk
- alexcrichton
- uweigand
- bjorn3
- cfallin
- avanhatt
- Yage Hu

### Notes

- Updates
  - avanhatt: no updates, happy to answer questions on ISLE verifier
  - bjorn3: no updates
  - uweigand: no updates
  - alexcrichton: no updates
  - adamrk: no updates
  - cfallin:
    - planning to do branch fusion thing we talked about last week
    - verifier: want to discuss integration strategy in CI
  - fitzgen:
    - fixed alias analysis assertion failure
    - new alias analysis bug -- fix later today. Endianness not part of key for
      store-to-load forwarding
      - cfallin: no correctness concern in practice in Wasmtime because always
        same endianness for given location?
    - building a new register allocator
      - starting with cfallin's idea to use regions: region tree, allocate the
        inner regions (loop bodies) first, push the moves out of loop bodies
      - cfallin: ability to switch over dynamically and have a few releases
        with old and new is important
        - fitzgen: interface bits that RA2 Function trait doesn't have, like
          remat
        - cfallin: we control RA2 interface too; we can add anything we need,
          then put new regalloc behind the trait so we can switch dynamically

- cfallin: verifier: do we want to have runs in CI and potentially have latency
  spikes as the cache grows? Or move back toward checked-in cache where all
  compute happens client-side?
  - alexcrichton: drive away contributors if we require folks to install the tools locally?
  - avanhatt: LLMs figure out how to install -- less of an issue?
  - bjorn3: multi-hour run to get a corpus locally?
    - alexcrichton: this is the point of downloading the release artifact
  - (lots of discussion about the tradeoff: having folks be able to run the
    tools locally versus an opaque backend error; on the other hand, we
    debug-via-CI anyway for e.g. platforms we don't have access to)
  - fitzgen: other requirements, e.g. ability to run verification completely
    locally e.g. on an airplane?
    - could still download corpus
  - tentative agreement to check in the cache
  - avanhatt: what about LLMs hallucinating cache files?
    - cfallin: initial thought was we'd reverify contributions from
      untrusted/new contributors but... hmm, that's why we have CI, to run
      checks for us
  - great point -- let's go back to CI job that does all compute in CI (fully
    trusted), with cache
