# April 24 project call

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

- saulcabrera
- alexcrichton
- avanhatt
- jameysharp
- cfallin
- fitzgen

### Notes

- Updates
  - avanhatt: preparing for ASPLOS talk on veri-isle next week
  - alexcrichton: no updates
  - saulcabrera:
    - some Winch fixes on aarch64, and on trampolines
    - should start testing Winch via integration tests rather than its own test
      suite
      - fitzgen: similar discussions around multiple configs/GC backends;
        custom proc-macro for `#[wasmtime_test]` that takes a `Config`
      - alexcrichton: make separate CI jobs for each configuration?
      - cfallin: what's the breakdown of build time vs. test time? tests used
        to be pretty fast at least
      - (lots of discussion about test matrices, avoiding combinatorial blowup,
        having fast feedback loops)
  - cfallin: no Cranelift updates
  - fitzgen:
    - talked with folks about putting safepoints in CLIF directly
    - probably necessary to be more explicit because of interior pointers into
      objects -- e.g., GC'd arrays
  - jameysharp
    - lots of optimizations and cleanups pieced out from a large PR: making the
      OperandCollector take mut borrows of registers and then repurpose it (as
      a functor/visitor, one source of truth for regs in insts, also lets us
      get various other wins e.g. resolve aliases right away)

- cfallin: Wasmtime optimization for fast indirect calls by caching one entry
  - commit: https://github.com/cfallin/wasmtime/commit/fcc6e573e575d2232cffa3b59c2f0096f3e66f64#diff-d454bf47bbfdc00a9cfb20187551c54c141866f3f44549f48dc4236496a6efe6
  - will put this up as a PR for discussion
  - (lots of discussion about other approaches to make indirect calls fast)
  - fitzgen: hoist all checks up top then have two versions of function

- jameysharp:
  - using `test` instruction when branching on bitwise and
  - generating boolean *and* testing: should we have an optimization to reuse
    flags that are already produced?
  - fitzgen: in general would be nice to have a peephole pass to clean up
    things like that; also ldp/stp opportunities on aarch64; also swapping args
    and avoiding moves on destructive-dest (x86) ISAs
  - (lots of discussion about the design of this; pre vs. post regalloc,
    fitzgen's string matching on opcodes idea, ...)
