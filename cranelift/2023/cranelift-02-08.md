# February 08 project call

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

- afonso360
- cfallin
- fitzgen
- elliottt
- avanhatt
- uweigand
- bjorn3
- jameysharp

### Notes

- cfallin: commutativity annotation in rules?
  - uweigand? rule blowup?
    - cfallin: observation (due to fitzgen) that rules are much cheaper than
      rewrites
  - avanhatt: subtle action-at-a-distance
    - fitzgen: probably ok? we're likely only going to mark things that are
      actually commutative
  - avanhatt: don't duplicate if rule is already symmetric (e.g. `(iadd x y)`)
  - avanhatt: how to see duplicated rules?
    - we could have a "desugar" / "expand" feature; would be generally useful,
      this is good motivation for it
  - uweigand: most backends have these written out manually; will be overlap
    until we fix

- iconst and narrow types

- updates
  - afonso360 -- none
  - cfallin -- fuzzbug fixes, codereview, nothing on CL otherwise
  - avanhatt
    - iconst and narrow types
      - typesafe `imm` that only gives value given type, and does zero-extends
        in central place?
      - does verification project depend on this or just something we noticed?
        - a little of both; right now some bugs we couldn't reproduce but
          annotations could change
    - verifier found a mid-end bug!
  - elliottt
    - making br_table use BlockCalls, debugging now
  - bjorn3
    - working on allowing custom profiler to be plugged into the timing
      infrastructure of cranelift with the aim of integrating with rustc's
      measurement profiler
  - uweigand
    - looking at tail calls + s390x
      - looking like non-tail-calls will take a small regression -- more SP
        manipulation on calls. Probably not large
      - fitzgen: did see comment (thanks!), will dig in
  - jameysharp
    - codereview, mid-end optimization rules, working with fitzgen on tail
      calls 
  - fitzgen
    - tail calls implementation progressing with jameysharp
      - `return_call{,_indirect}` in CLIF now, but no lowerings
      - next: overhaul trampolines as per RFC
    - BA TSC, etc
      - new Debugging SIG
    - running Souper to get new optimizations
      - a bit more challenging than expected!
        - cfallin: backends don't have full coverage of lowerings, and not all
          invariants are encapsulated, any others?
        - jameysharp: some other misc limitations as well, and things that make
          things difficult: subsume is hard to think about, integer vs not,
          iconst.i128, ...
        - cfallin: fuzz locally first?
          - afonso: run in CI for 5 min?
          - elliottt: CI already overloaded, probably not
        - uweigand: do we know if rules make things better?
          - fitzgen: harvested so apply by construction
          - cfallin: is result faster?
          - fitzgen: plan to measure once all planned souper opts are in
          - uweigand: important to measure, unexpected effects (reg pressure,
            missing special lowerings, ...) can happen
          - fitzgen: good point! many of the added rules so far have been
            unambiguous improvements (const-prop shifts, ...) but good to keep
            in mind

- cfallin: cargo-vet discussion in next Wasmtime biweekly (next week), could be
  relevant to folks in CL too
