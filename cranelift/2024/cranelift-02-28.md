# February 28 project call

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
    1. [wasmtime issue #7999](https://github.com/bytecodealliance/wasmtime/issues/7999)

## Notes

### Attendees

- cfallin
- abrown
- fitzgen
- alexcrichton
- avanhatt
- elliottt
- saulcabrera

### Notes

- elliottt: Cranelift issue with miscompilation of ireduce, imul, ishl
  - https://github.com/bytecodealliance/wasmtime/issues/7999
  - PR 7719 tries to push ireduces into args
  - rule LHS was befuddled by multi-match semantics: "any node in eclass"
    rather than "this specific node"
  - was surprising; hope to come up with guidelines to avoid this in writing
    rules
  - basic principle: match on an enode and have conditions on pieces extracted
    from it, rather than separately on the `Value` (that represents whole
    eclass)
  - abrown: why didn't fuzzing catch this?
    - alexcrichton: specific constants, hard to find
    - avanhatt: have a student starting to look at smarter fuzzing for this

  - fixes long-term for the footgun?
    - name specific enode?
    - stick to simple explicit patterns; be careful about higher-order helpers
      and predicates
    - OK to introduce some enodes that won't be used; better than trying to
      explicitly bound things a-priori with predicates
    - (Jamey's idea from yesterday) ban as-patterns on multi-extractors?
    - will add a note to opts/README.md about patterns to be careful about

- Updates
  - fitzgen: helped debug 7999; added tweaks to egraph logging
  - elliottt: working on egraph-related things; trampolines in Winch for
    component model
  - avanhatt: getting undergrad projects going; rule-chaining for x86 support
    in verifier
  - saulcabrera: fixing fuzzer issues in Winch
  - alexcrichton: no updates
  - abrown:
    - gave a Sightglass presentation at Wasm benchmarking subgroup
    - issue with MPK and CoW
  - cfallin: PCC -- worked out better approach, left note, context-switched
    away; helped with egraph stuff
