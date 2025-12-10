# December 10 project call

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
    1. `patchable_call` vs. patchability of all calls without returns, and
       patchable try-calls (cfallin)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- bjorn3
- fitzgen
- Amanieu
- erikrose
- abrown
- alexcrichton
- uweigand
- cfallin
- jlbirch

### Notes

- Updates
  - uweigand: looked at s390x patchable ABI PR
  - alexcrichton: no updates
  - abrown: wrote up list of hand-off items
  - erikrose: reading background for dead-load-with-context
  - Amanieu: no updates
  - bjorn3: no updates
  - cfallin: breakpoints and single-stepping working in Wasmtime; s390x
    patchable ABI fix, and thoughts about try-calls and patchability
  - fitzgen: looked at egraph extraction cost function saturation issues found
    by Bongjun; as we discussed in issue, optimal strategy is NP-hard, but
    maybe approximations could work

- cfallin: debugging
  - patchable any callsites?
    - realization that inlining `patchable_call` into `try_call` callsite needs
      a `patchable_try_call`, or else we make patchability an aspect of every
      callsite (orthogonal dimension)
    - does this work with tail ABI too, given the stack adjustments are
      non-symmetric (adjust down at callsite, up in func return)? Yes, because
      we patch out the adjustment and the call to the func that adjusts back
    - bjorn3: could store a bit in the extfunc data rather than on inst
    - we shouldn't patch actual `return_call`s (tail calls) -- nonsensical
      since the inst is a terminator and we would fall off the end
    - handling unwind info for s390x vector regs: DWARF doesn't have a way to
      describe saved whole vec regs today. Do we need this? Not for Wasmtime
      exceptions/unwinder; may be nice one day for perf but we'd invent our own
      register-restore unwind format for that
    - let's call the Patchable ABI something else, since it's orthogonal
      - Cold? No, unwind info issue is real because cg-clif uses it for cold Rust funcs
      - Amanieu: PreserveAll is LLVM's name for this convention
    - conclusion: make patchability an aspect of call/indirect,
      `try_call`/indirect; rename Patchable ABI to PreserveAll; all returns in
      PreserveAll; check no returns in patchable call insts

- Amanieu: regalloc3 chances currently?
  - main issue has been review bandwidth; not able to review new 25k line body of code
  - but maybe we could have it as a non-default option that we explicitly don't
    support at tier 1 and can turn off if broken (a la fastalloc)
  - conclusion: we'll do that

- abrown: handoffs
  - assembler, new APX instructions; Johnnie and Rahul interested in continuing
  - rest is maintenance of various other Wasmtime and WASI things (threads,
    profiling, MPK, ...)
    - MPK: #7942 and also differential fuzzing MPK conflict with V8
