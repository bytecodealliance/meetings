# August 13 project call

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

- bjorn3
- erikrose
- fitzgen
- cfallin
- abrown
- uweigand
- alexcrichton

### Notes

- Updates
  - abrown: thinking about APX enablement (new x86-64 extension, to be released
    soon: provides new versions of scalar instructions, more GPRs, etc)
  - erikrose: working on reducing epoch overhead in Wasmtime; no CL updates
  - cfallin: no updates
  - bjorn3: miscompilation mentioned before due to object crate; new release
    now fixes
  - uweigand: no updates
  - fitzgen: fixed a few bugs in inliner: removing blocks only if present; and
    making function references absolute rather than relative to module

- abrown: APX
  - still "thinking it through" -- need to decide general approach
  - recall: REX prefixes (for x86-64) are one-byte prefixes; APX adds REX2
    prefix that is two bytes; this lets us address 32 registers rather than 16
  - also, new EVEX extension (four-byte prefix) that can be used for scalar
    instructions, and also lets us have three-register form ("new data
    destination", NDD)
  - a bit to avoid modifying flags; a bit to zero or not zero upper bits for
    narrow instructions
  - fitzgen: we don't want to use unless we have to, because of code size
  - cfallin: leave MachInst unconstrained, for regalloc to have more freedom
    and do better; then emit shorter form
  - add a bool flag? do we have access to ISA flags?
  - alexcrichton: could we do what we do in ISLE today for AVX vs SSE?
  - cfallin: depends on regularity: if fully regular maybe plumbing through a
    flag is better?
  - alexcrichton: assembler generates insts so maybe not so bad
  - regalloc constraints: will want a "lower half of register class" bit in
    operand constraints
  - cfallin: PUSH2/POP2 for ABI code -- slightly faster prologues/epilogues too

- alexcrichton: small update on fuzzing: relatively large number of
  fuzzbugs+timeouts, looking through it
