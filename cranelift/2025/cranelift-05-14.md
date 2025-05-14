# May 14 project call

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

* Erik Rose
* Nick Fitzgerald
* Ulrich Weigand
* Johnnie Birch
* Andrew Brown
* Rahul Chaphalkar

### Notes

- Rahul: working on the assembler
- Ulrich: reworked `call` to remove the differences between s390x and elsewhere, now calls are
triggered in ISLE with Rust helpers; used constant pool in s390x to support f16 and f128; now
working on f128, can't do that easily on f16 because s390x has no instructions, could use a library
routine like LLVM does (how?)
  + Nick: could use libcalls that end up as relocations; Dan did some recent work to avoid some
    relocations up at the Wasmtime level
  + Andrew: any feedback to the constant pool refactoring?
  + Ulrich: `SyntheticAmode` tried to lower to label but don't have that until we have a sink; one
instance left of an inline constant to jump over, no way to put a relocatable constant in the
constant pool; can only put 64-bit large constants in, want to put 32-bit constants in.
- Erik: no updates
- Johnnie: no updates
- Andrew: need to contact Alex about reworking ISLE shift rules
- Nick: no Cranelift work, some prototyping on compile-time builtins that get auto-inlined; new
mini-language that would lower to a subset of CLIF; will be on vacation for a few weeks
  + Erik: why not inline?
  + Nick: we don't have an inliner
