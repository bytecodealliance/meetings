# June 07 project call

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
- abrown
- cfallin
- uweigand
- jlbirch

### Notes

- afonso360: RISC-V SIMD; about 50% done (!).

- abrown:
  - SSE2 PRs from Alex, looking into best instructions to use. mem->XMM,
    movddup; XMM->XMM, pshufd

- uweigand:
  - rustc 1.70 -- could not reproduce CI crashes locally; double-check they're
    still an issue?

- jlbirch:
  - no updates

- cfallin:
  - discussions with fitzgen and jameysharp on their tail-call implementation
    work. s390x: any reason we can't do all-registers-caller-save (no clobber
    restores)?
    - uweigand: should work fine
