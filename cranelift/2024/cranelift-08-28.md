# August 28 project call

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

- avanhatt
- fitzgen
- elliottt
- alexcrichton
- cfallin

### Notes

- Updates
  - alexcrichton: no updates
  - elliottt: no immediate updates; maybe going to start removing safepoint
    support from RA2
  - avanhatt: no updates
  - cfallin: no updates
  - fitzgen: removed R32/R64 types and support for "old style" stackmaps using
    RA2 (nice!)

- alexcrichton: 128-bit ops
  - main question on spec issue is overflow ops vs. true 128-bit ops
  - [discussion
    comment](https://github.com/WebAssembly/128-bit-arithmetic/blob/main/proposals/128-bit-arithmetic/Overview.md#alternative-overflow-flags)
  - can we peephole? can we generate different code if one output not used?
  - cfallin: doable, probably have a new extractor in lowering to say if one
    output isn't used; to directly reuse flags, pattern-match the pair of
    producer, consumer
  - cfallin: also try in Winch -- how does a single-pass compiler do with it?
  - cfallin: why is presumption to do exactly what CPUs do (carry/overflow
    flags)? question should be which is easier on the engines
  - fitzgen: would be helpful/convincing/etc to show a summary table and give
    concise arguments
  - alexcrichton: generally benchmarks are 30-40% slower than native (much
    better than without these ops)

- alexcrichton: exceptions?
  - bjorn3 should be able to upstream Cranelift-side exception support at some
    point
  - new idea: use tail-calls to handle clobber restores in each frame; linear
    cost total; tail-call to handler that then jumps to parent frame
  - cfallin: why not tables of places to restore GPRs from? (i.e. still don't
    use DWARF for clobber-restores) don't want to give up on O(1) unwind overall
  - alexcrichton: register restore still O(n)?
  - cfallin: don't know how to get away from that; fundamentally need to see
    all frames in the middle
  - fitzgen: why this over the "calling convention" approach (implicit
    exception-throwing flag on every call)?
  - (discussion about tradeoffs of complexity)
