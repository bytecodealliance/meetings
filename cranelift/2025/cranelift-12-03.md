# December 03 project call

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
    1. `patchable_call` and future directions (@alexcrichton)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- erikrose
- fitzgen
- bjorn3
- abrown
- alexcrichton
- cfallin
- Rahul Chaphulkar
- jlbirch

### Notes

- `patchable_call` extensions
  - alexcrichton: can we use patchable instructions from signals to "inject
    calls"? better way than stackframe call injection
    - idea would be to do a post-fixup pass where we change patchable calls to
      a load-that-traps-on-epoch-end (unmapped page trick)
  - fitzgen/cfallin: concerns about using an instruction for something it's
    not meant for; source of bugs and confusion just to avoid adding another
    CLIF instruction
  - reasoning is that we want the fixed-reg constraints to communicate vmctx to
    the runtime/signal handler; patchability not used at runtime, and not a call
  - alternatives: that's not "patchable" (at runtime) anymore and not a call;
    why `patchable_call`? Why not make another CLIF opcode whose semantics are
    "dead load with context in reg(s)"?
    - cfallin: another alternative is to go in the other direction and make
      fully general: any instruction (without defs) can be patched out; any
      instruction can carry extra state in reg(s)
    - (discussion about generality and testability maintainability)
  - conclusion: `dead_load_with_context` for now; adding a CLIF opcode is not
    that bad

- ISLE mid-end regression with associativity
  - rule has had bad effects in the past as well
  - rule has no context about bigger shape, or available registers
  - cfallin: takeaway: avoid rewrites unless/until we have a framework to
    reason about register pressure and amortized cost of reused values
  - fitzgen: problem is more specific here -- rule is trying to use information
    we don't have in the mid-end
  - fitzgen: we should run benchmarks more frequently. For regressions like
    this instructions-retired would have found it and is much faster to run
  - we should have a comment bot leave a comment when we touch files in the
    mid-end

- Updates
  - jlbirch: no updates
  - Rahul: no updates
  - alexcrichton: no updates other than above
  - abrown: leaving Intel and Cranelift :-(
  - bjorn3: no updates
  - erikrose: thinking about signal-based epochs
  - cfallin: debugging on Wasmtime side; `patchable_call` in Cranelift added
  - fitzgen: OOM handling may cause some changes to Cranelift data structures,
    but likely minimal
