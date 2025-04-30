# April 30 project call

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
* bjorn3
* cfallin
* fitzgen
* jlbirch
* rahul
* uweigand

### Notes

* rahul: no updates
* andrew: assembler! Draft PR for removing "old ALU stuff", needs to remove
  things from winch and use the new stuff. Cargo features, dependencies,
  weirdness.
* alex: probably need to have an unconditional dependency here
* chris: would probably require too much surgery to have an optional dependency
* andrew: ok makes me feel better about this
* alex: no updates
* chris: no updates
* bjorn: working on a new progress report for cg_clif. Want to mention
  exception-handling support. Chris how much of this design was independently
  invented vs based on my work?
* chris: didn't look at your branch but sure I had your design in my head from
  historical discussions. More you built it and started discussions and we ended
  up coming up with final design later. Happy to credit you!
* johnnie: posted a patch for VEX encoding - broken into two PRs due to issues
  connecting with ISLE. Getting cleanup items done first and will follow-up
  after.
* ulrich: one patch for f16/f128 support and helped review. Just basic stuff
  though, what's the plan for these? There's no f16 support on s390x.
* bjorn: ABI handling and moving around is most important. Most of the rest can
  be done using intrinsics from compiler-builtins. Going to be slower but for
  cg_clif the important thing is that it works, not that it's fast.
* chris: to add, this is for cg_clif, not Wasmtime. General Cranelift support.
* bjorn: Rust is getting native f16 and f128 support. cg_clif needs to support
  in the future.
* uweigand: right that's why we added in LLVM but it does everything as f32 and
  calls library routine that does conversion. If this is only backend for
  existing cg_clif then would probably do the same? Ok!
* bjorn: I'm not driving this implementation but there is an open PR for the
  cg_clif side
* uweigand: for f128 we certainly want to implement, but shouldn't restrict what
  the instructions are in CLIF.
* nick: no updates
* nick: see y'all next time!
