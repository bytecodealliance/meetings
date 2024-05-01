# May 01 project call

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

* Nick Fitzgerald
* Jamey Sharp
* Andrew Brown
* Trevor Elliott
* Chris Fallin

### Notes

Status updates:

- Trevor: nothing from me
- Jamey: many Cranelift cleanups; most already reviewed, e.g., linear time sort; want to discuss the pretty-printing bug
- Andrew: no updates
- Nick: no updates
- Pretty-printing, https://github.com/bytecodealliance/wasmtime/pull/8508:
  + Jamey: there's a difference between how Cranelift's pretty printer and capstone are printing `vpsraq`
  + discussion concluding that the first operand of `vpsraq` in AT&T syntax should probably be `src2` (the shift operand)
  + Nick: quickly dumped some instructions through `objdump`, https://gist.github.com/fitzgen/8ed7fb3236570bbfd377084a1c5b4f15
  + Jamey: might be nice if we had tests that compared our pretty-printer disassembly to capstone; will add some tests
  + Andrew: I'll review it!
- Chris: close to merging call-indirect optimization
  + Nick: limiting it to as certain number of call sites? Where is that?
  + Chris: will take a look, it was 50k at some point
