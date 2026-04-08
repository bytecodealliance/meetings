# April 08 project call

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

- erikrose
- uweigand
- bjorn3
- alexcrichton
- fitzgen
- cfallin
- avanhatt

### Notes

- Updates
  - erikrose: quicker epoch interrupts in Wasmtime+Cranelift
  - uweigand: no major updates; reviewing Jimmy's patches for s390x-z17 support
  - bjorn3: no updates
  - alexcrichton: miscellaneous bugfixes
  - fitzgen: no updates
  - cfallin: no major updates
  - avanhatt: no major updates

- Epoch interruption in Wasmtime, and Cranelift supporting features
  - analysis -- how good are dead loads of (to be unmapped) stack vs. no epochs
    whatsoever?
  - efficiency of TLB shootdowns? one shootdown per stack vs. single store to
    shared memory?
    - variant that changes a pointer-to-load-from to NULL?
