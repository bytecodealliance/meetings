# December 06 project call

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

* afonso360
* elliottt
* alexcrichton
* abrown
* uweigand

### Status

* abrown
  * worked a bit on mpk stuff
  * may need help from alexcrichton with changing protection keys across async
    switches
  * ac: `tests/all/async_functions.rs` is the test to look at, there may already
    be one that executes, suspends and then resumes

### Notes

* afonso360
  * `fixed-late-use` constraints in regalloc2 would be useful in the context of
    the riscv64 backend <https://github.com/bytecodealliance/regalloc2/pull/168>
