# January 04 project call

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
    1. Fuzzing compilation on all architectures

## Notes

### Attendees

### Notes

* jamey
  * not much, holidays
  * thinking about translation validation in the ISLE compiler
  * check that generated rust code matches the ISLE semantics
  * catching up on PRs
* afonso
  * added a few more instructions to fuzzgen
  * only missing 5 from scalar instructions
  * got a riscv machine, going to fuzz with it, filing issues
* trevor
  * fixed a fuzz bug that afonso found (thanks afonso)
  * adding fuzzing support for cranelift cross compilation
  * could be a tsunami of fuzz bugs since these backends haven't been fuzzed in
    oss-fuzz
  * `cfg`s to turn things on/off in fuzzgen won't work for cross-compilation,
    need to have runtime checks based on runtime compilation target
  * figuring out how to make progress on yak stack of block-and-arguments and
    single block-terminator instructions
* alexa
  * refactoring
* bjorn3
  * no updates
* fitzgen
  * no updates
