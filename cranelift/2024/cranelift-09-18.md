# September 18 project call

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
- elliottt
- avanhatt
- alexcrichton
- cfallin
- uweigand

### Notes

- fitzgen: fuzzing ideas (Alex's issue writeup)
  - alexcrichton: idea is to improve coverage feedback back into fuzzer from
    instruction selection rules
    - https://github.com/bytecodealliance/wasmtime/issues/9255
    - add instrumentation code at runtime that increments counters on vmctx
      when lowering rules are hit
    - cfallin: potential complexities when emitting new instructions in middle
      of instruction emission path; static instead?
    - alexcrichton: needs to be runtime -- fuzzing coverage already covers
      static side pretty well
    - fitzgen: new global like stacklimit checks to indicate this mode in CLIF;
      then, new method in trait invoked by ISLE code
  - avanhatt: only lowering rules, or other intermediate rules too?
    - e.g., CVEs in amode computation
    - alexcrichton: in theory these are discoverable with more interesting
      inputs in general
    - maybe a hash function that combines all rules that were used?

- Updates
  - uweigand: no updates
  - alexcrichton: no updates
  - avanhatt: finding time to get back to landing isle-veri PR
  - cfallin: no updates
  - fitzgen: put up PR to implement Wasm GC in Wasmtime; optimizing Wasm GC
    code is going to be very interesting (bounds-checking, etc)
    - GC arena is slightly different than Wasm memories: traps are not
      semantically visible so we can combine more bounds-checks
      - alexcrichton: trick with null GC refs by unmapping first page, too
  - avanhatt: fuzzing that specifically targets memory accesses
    - alexcrichton: do we already have a fuzz target for this? `memory_accesses`
  - avanhatt: rule-directed fuzzing input generation
    - use shape of rule LHSes to generate inputs
    - use rules to come up with constants
  - fitzgen: related: accepted paper on RGFuzz in IEEE S\&P 2025 for
    "Rule-Guided Fuzzer..."
