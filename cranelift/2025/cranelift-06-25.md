# June 25 project call

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

- uweigand
- fitzgen
- alexcrichton
- Rahul
- Simon (simvux)
- bjorn3
- cfallin
- abrown

### Notes

Updates

- alexcrichton
  - some new assembler lowerings
  - refactored condition handling in x64 ISLE
    - multiple consumers (`brif`, `select`) and multiple kinds of producers
      (`icmp`, `fcmp`, ...) -- created a single type to meet in the middle.
      Enabled adding some new lowerings easily (e.g., branch of `band` -- using
      `test`'s flag-setting directly)
- abrown
  - new assembler reviews, and some pending work to move AVX instructions over
- bjorn3
  - experimenting with incremental caching, found some inefficiencies
- Simon: no updates
- Rahul: working on EVEX encoding in new assembler
- uweigand
  - s390x exception-resume assembly: patch looks good natively; maybe qemu
    issue?
  - native s390x runners are now a possibility -- will look at getting this
    enabled
- cfallin: working on Wasm exceptions using Cranelift exception support
- fitzgen
  - reviving magic-div-const optimization now that we can optimize
    side-effecting instructions again (!)
  - some thoughts on ISLE pattern-matching -- more general `match` construct
    able to embed within other rules without sub-rules?

- avoiding setjmp/longjmp in exception unwind -- don't unwind over Rust frames
  - instead, return "trap" state from hostcall and throw from within
    Cranelift-generated trampoline; and have native Cranelift inst for what we
    currently do with inline assembly
  - this removes the UB, also makes our `no_std` more dependency-free
    (Cranelift code is handling what we used platform setjmp/longjmp for)
  - cfallin: signal-based traps still longjmp or edit the frame? A: edit the frame
  - fitzgen: also means that we can finally avoid the all-clobbers behavior on
    `try_call`
