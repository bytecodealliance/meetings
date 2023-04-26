# April 26 project call

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
    1. Cranelift website (cfallin)
        - Cranelift should have its own website, now has `cranelift.dev`
        - Plan is for a few simple static pages that then link to the repo. 
        - Tell cfallin if you have ideas for what else to put there.

## Notes

### Attendees

- afonso360
- cfallin
- elliottt
- alexcrichton
- uweigand
- jameysharp
- avanhatt
- abrown

### Status

- afonso360: SIMD stuff, initial part landed last week. Arithmetic ops. Some additional extensions: can lower all SIMD types with essentially the same code. Now trying to get the Wasmtime test suite working. Recent issue has been fighting with is clobbers. Implementation does not yet save and restore vector registers. Floating point register file and vector register file are separate on the backend. 
    - cfallin: regalloc2 now has another bit for classes (4 instead of 2). Suggested doing a PR to add a vector register class. 
- cfallin:  Code review, discussions. 
- elliottt: Regalloc2 work. 
- alexcrichton:  No updates. 
- uweigand:  No updates. 
- jameysharp: Writing up issues, no recent code updates. 
- avanhatt: Paper things, moving annotation language to ISLE parser.
- abrown: <I missed this update, sorry!>

