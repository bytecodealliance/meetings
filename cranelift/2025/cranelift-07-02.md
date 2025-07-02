# July 02 project call

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

* fitzgen
* acrichto
* erikrose
* uweigand
* cfallin

### Notes

#### Status Updates

* uweigand: not much but looking again into the crash for s390x with inline
  assembly change. Wasn't able to reproduce natively, trying in QEMU. So far
  unable to do so. Same ubuntu/qemu/etc.
* acrichto: maybe compile settings?
* uweigand: thought I got those, will recheck.
* fitzgen: deterministic?
* acrichto: seems so so far.
* erikrose: none yet but going to start looking into VM-based epoch switching.
* cfallin: no updates.
* fitzgen: not much too cranelift-related but looking to ramp up on compile-time
  builtins.
* acrichto: no updates, but in lieu of abrown not being here all the x64
  instructions are migrated now except conditional jumps and EVEX. EVEX is
  underway too!
* acrichto: conditional branches only affect `emit.rs` right now and not much
  else so not too too pressing.
* cfallin: would be nice to fill out though to have a complete assembler, and it
  might also be able to delete the `WinchJmpIf` pseudo-instruction in ISLE
  perhaps?
* fitzgen: no more updates, have a good week all!

