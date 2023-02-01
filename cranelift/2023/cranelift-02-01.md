# February 01 project call

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
- cfallin
- uweigand
- abrown
- avanatt
- elliottt
- jameysharp

### Notes

- mid-end optimization rule caused regression (5682)

- status
  - cfallin
    - RFC for 2023 roadmap:
      https://github.com/bytecodealliance/rfcs/pull/30
  - abrown - nothing
  - uweigand
    - read over tail-call RFC
      - will comment from an s390x perspective
  - jameysharp
    - helping on tail-calls, egraphs, etc
    - on 5682 regression: not masking upper bits?
  - avanhatt
    - verifier: switched to `easy-smt`
    - bitwidth-independent shifts, etc; ongoing work
  - elliottt
    - removed brz, brnz; just brif now (!!)
    - working on `br_table` to use `BlockCall` (with args in target) as well
  - fitzgen
    - working on using Souper to generate mid-end optimization rules
    - working on tail-calls implementation
