# May 03 project call

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
    1. Cranelift website
       - draft/prototype: https://github.com/cfallin/cranelift.dev
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- elliottt
- cfallin
- afonso360
- jameysharp
- alexcrichton
- fitzgen
- jlbirch
- avanhatt

### Notes

- cfallin: cranelift.dev draft
  - no objections; ship it!

- Status
  - cfallin: lots of code review
  - elliottt: RA2 changes. Currently 12% faster compilation on bz2
  - jameysharp: no CL stuff
  - afonso360:
    - PR for vconst on riscv64
    - calling convention for vectors is suboptimal -- everything on stack.
      Custom convention with args/rets in registers?
      - alexcrichton: probably doesn't matter too much for perf, SIMD is all
        about tight  loops without calls
  - alexcrichton
    - SSE2 stuff
    - tackling miri issues in Wasmtime
  - jlbirch: no CL stuff
  - avanhatt
    - future planning for verification
  - fitzgen
    - tail calls

- elliottt: roundtripping `table_addr` printing properly (PR); separately, are
  we going to remove it?
  - no one aware of recent progress on this but we likely will want to
    eventually
