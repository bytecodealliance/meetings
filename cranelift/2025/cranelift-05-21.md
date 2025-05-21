# May 21 project call

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

- erikrose
- Rahul Chaphalkar
- saulcabrera
- abrown
- alexcrichton
- cfallin
- jlbirch
- uweigand
- Arjun Ramesh

### Notes

- Updates
  - erikrose: no updates
  - alexcrichton: moved some x86 instructions to new assembler. A few small
    optimizations (enum size, etc).
  - uweigand: added support for VCodeConstant literal pool and f128 in s390x.
    f16 is next -- no hardware support. May not be necessary yet for rustc
    frontend. Did add to LLVM backend for Rust support.
    - alexcrichton: not likely to be needed for Rust soon -- more of a
      long-term desire.
    - `*_overflow` instructions -- PR came through at some point (#9214) -- do
      we need this?
      - cfallin: not at all in short term, these were just "nice to have"
        additions
    - Planning to add support for z17 features (newest s390x) -- i128 bit
      arithmetic, multiplies, a few other things
  - saulcabrera: working on OSS-fuzz issues related to `br_table` and
    multivalue in Winch. aarch64: stack checks contributed last week, masm now
    complete, now hunting down last issues.
  - Rahul: no updates
  - abrown: working on new x64 assembler, things going well; can do muls/divs,
    good sign (weird fixed reg constraints etc).
    - question about where to put SSE vs AVX switch -- inside some instruction
      that chooses at emit, or generated constructors in ISLE, or manually in
      handwritten ISLE as now?
      - probably best not to make assembler too magic -- keep it 1-to-1; but in
        generated ISLE seems reasonable
    - perf? we should measure, but we don't expect too much delta from existing
      - alexcrichton: two-level match ("external inst") may add some overhead;
        not sure
  - jlbirch: sqrt instructions in x64 assembler
  - Arjun: welcome! summer intern, working on record/replay in Wasmtime,
    invited to Cranelift meeting as well
