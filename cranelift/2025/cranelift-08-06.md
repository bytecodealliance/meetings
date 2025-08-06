# August 06 project call

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

* abrown
* alexcrichton
* fitzgen
* jlbirch
* bjorn3

### Notes

#### Updates

* abrown - CPU features are now more complicated! (thanks for review, fixed
  things in rebase) Should be better to express all the checks we need in the
  future directly with the assembler.

* bjorn3 - made branch to update cg_clif to the upcoming Cranelift release.
  Ending up with corrupt archive files, need to look into it.
* fitzgen - cranelift_{object,codegen}?
* bjorn3 - could be cranelift_object, could be crate itself, something about a
  symbol missing.
* fitzgen - regression?
* bjorn3 - yes

* jlbirch - no updates

* alexcrichton - no updates

* fitzgen - fuzz bugs for the inliner, figuring them out. Unsure how to fix just
  yet. Looks like Wasmtime layer not Cranelift layer. Haven't dug in too much yet.
