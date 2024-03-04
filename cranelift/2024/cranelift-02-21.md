# February 21 project call

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

- avanhatt
- cfallin
- saulcabrera
- fitzgen
- jameysharp
- elliottt
- alexcrichton

### Notes

- status updates
  - fitzgen: none
  - alexcrichton: none
  - elliottt: moved stack checks in Winch into function prologue
    - required MachBuffer primitives to backpatch constants, but all seems good
      now
    - outstanding issue: trampolines need stack checks still
  - jameysharp: working with elliottt, fitzgen, lafp on egraphs issue wrt
    domtree scoping
  - saulcabrera: fixing Winch issues from fuzzing and otherwise; main root
    cause seems to be ABI (PR up this morning)
  - avanhatt: Michael McLoughlin has been able to verify aarch64 rules against
    SAIL (formal ISA spec)
  - cfallin: PCC updates; egraphs
    - PCC: worked fine for unit tests; when optimization combines parts of
      multiple accesses, fact language was not expressive enough. Have a PR up
      that generalizes so every value is described by lower/upper bound and
      equality sets. Remaining issues with some quadratic behavior on GVN merge
      and with constant addresses; working through it now.
    - egraphs: talking through solutions with folks; seems we have a good
      answer re: domtree scoping (every enode has an "available scope")

- fuzzing: should we leave Winch enabled? (saulcabrera: offered to turn off if
  known failures are a problem)
  - only affecting throughput of differential fuzzer; ok to leave on

- fitzgen: Wasm GC discussion (not Cranelift but overlap with folks here)
  - how to represent GC refs? cloneable handle or owned type that precisely
    tracks when it drops?
  - landed on: something like HandleScope
