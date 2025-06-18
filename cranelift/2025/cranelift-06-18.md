# June 18 project call

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

### Attendees
- Erik Rose
- Bjorn III
- Saul Cabrera
- Nick Fitzgerald
- Alex Crichton
- Andrew Brown
- Johnnie Birch

### Notes

- Updates
  - ER: no updates
  - B: no updates
  - SC: aarch64 is complete for core Wasm in Winch as of this week; next thing
    is to write a blog post (NF: TSC to review?)
  - NF: more lowering rules for x64 to use immediates directly; also working on
    a way to automatically generate the integer numeric functions for ISLE to
    avoid roundabout checking
  - AB: may start to look at EVEX in the assembler
  - AC: partially done with compares and tests in the assembler; curious how we
    plan to convert calls and jumps, they have relocations that must be tracked
    mid-emission; we probably want to keep the pseudo-instructions

- Assembler, calls and jumps
  - NF: we may not require assembler-level changes? Use helper functions during
    emission?
  - AC: want to avoid breaking assembler encapsulation, but would be similar
  - NF: we do know the exact instructions so introspecting its emission seems
    fine

- Assembler, what remains?
  - AC, AB:
    - `SETcc`, `TEST`, `LEA`, `CMOVE`
    - calls and jumps
    - a few SSE instructions
    - more AVX instructions
    - all of the EVEX-based instructions

- Assembler, what about `LEA`?
  - NF: do we use fancy LEA addressing modes?
  - AC: maybe we should keep a pseudo-instruction for add or lea?
  - NF: do not create ISLE constructors for LEA?
  - AC: we probably do want the ISLE constructors, though?
  - NF: do we want a custom emit?
  - AC: how about we swap out the add at the last minute with a peephole pass?
  - NF: kind of want a peephole pass at this level anyways for aarch64 paired
    loads and stores
  - AC: easiest path right now is using a pseudo-instruction
