# May 29 project call

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

- alexcrichton
- fitzgen
- elliottt
- jameysharp
- abrown
- cfallin

### Notes

- abrown: no updates
- elliottt: nothing large; removed ArgumentPurpose::StackLimit (currently
  unused by Wasmtime/Cranelift)
- jameysharp:
  - hoping someone can take on #8690, need to pass it off
- alexcrichton:
  - fuzzbug with tail calls too, will file issue
  - looking at CI timing out since last night
- fitzgen:
  - split out some depth-first traversal reusable code from DomTree impl, for
    purpose of computing liveness for GC refs in frontend
- cfallin: no updates
