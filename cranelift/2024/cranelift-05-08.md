# May 08 project call

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

- abrown
- uweigand
- alexcrichton
- fitzgen
- avanhatt
- elliottt
- jameysharp
- cfallin

### Notes

- Updates
  - abrown: nothing
  - elliottt: looking at refactoring away nominal-SP, since we now alloc max
    arg space and don't adjust at at callsites
  - uweigand: nothing
  - alexcrichton: `no_std` landed in Wasmtime; Cranelift next?
  - avanhatt:
    - isle-veri paper on Cranelift presented at ASPLOS last week! Folks liked
      it, lots of questions.
    - other interesting ASPLOS paper: LFI: https://dl.acm.org/doi/pdf/10.1145/3620665.3640408
  - jameysharp:
    - lots of cleanups to merge register operand logic for pretty-printing,
      emit, operand collection (nice!)
    - cleanups in lowering phase
  - fitzgen: nothing
  - cfallin: nothing

- elliottt: s390x tail calls? what is the remaining delta?
  - migrate s390x to common call infra? or implement separately?
  - uweigand: original idea of doing calls in ISLE was to share optimization
    opportunities (e.g. sign-extending args)
  - uweigand: also, currently implementing (only) system ABI; need to keep that
    regardless
  - uweigand: other difference: no frame pointer
    - two main purposes for FP on other platforms: referencing frame when doing
      alloca, and easier unwind
    - on s390x, there is a "frame pointer" when doing alloca but it points to
      bottom of frame, not top, so unwind is different (because positive
      offsets are preferred in ISA, at least originally)
    - on s390x, backtraces use a "backchain" mechanism: bottom-most word of
      stack frame is a linked list, like FP on other platforms (but no FP
      register). Need to handle on tailcalls by moving it like we do for return
      addresses on x86.
    - so we need a separate FP register on s390x for tail calls
    - jameysharp: unclear why we can't do tail calls in system ABI?
      - (answer: something about 160-byte region and its use as callee-save
        region for callees)
    - (more discussion about how to keep compatibility with backchain)

- jameysharp: sign- and zero-extending args (in context of s390x)
  - move sign and zero extensions earlier in pipeline in Cranelift? legalized
    into CLIF; allow optimizer to GVN these ops, ...
  - cfallin: old Cranelift worked this way; complexities around ordering
    constraints; we'd want to be careful not to walk back into that complexity
    (or overheads of rewriting)
  - (discussions about interaction of alias analysis with arg loads and
    arg-area stores in tail calls)
