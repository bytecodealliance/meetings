# November 30 project call

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
    2. fitzgen: Can we remove `uload8` et al instructions and replace them with `(uextend (load.i8 ...))` lowering rules in our backends?

## Notes

### Attendees

- fitzgen
- elliottt
- afonso360
- bjorn3
- abrown
- jameysharp
- cfallin
- uweigand

### Notes

- fitzgen: can we remove uload8 etc and replace with (uextend (load ...))?
  - semantics exactly the same
  - needed cleanup for heap_load / heap_store (to avoid heap_uload8 etc)
  - cfallin: sounds good

- status
  - afonso360: nothing
  - elliottt:
    - finished merging last bits to get Cranelift produce SSA VCode
    - exposed RA2 SSA validator, have a branch that turns this on for fuzzing,
      want to get it for clif-util compile tests as well
    - consolidating branch instructions into a single `if` branch rather than
      cond + uncond
  - bjorn3
    - working on the build system of `cg_clif` to make it possible to integrate
      it with rust's `./x.py test`.
  - cfallin
    - egraphs-in-DFG progress: pair-programming with Jamey, very productive;
      refactor of egraphs infrastructure to use DataFlowGraph / existing CLIF
      data structures is going smoothly and simplifying a lot of things. Expect
      to be done this week or next, hope will reduce compile-time overhead of
      egraphs approach to zero or very close
  - abrown
    - x64 leaf functions without prologue/epilogue
      - test failure, any memories of what was issue before?
        - cfallin: nope, Anton ran into this when doing aarch64 a while ago
  - jameysharp
    - new ISLE compiler backend strategy
      - "unreachable" rules cause issues; making islec detect and error on
        this, and fixing backends to not have unreachable rules
      - most of the way to generating valid Rust-ish output
        - close to getting output better than current islec's output, and some
          optimizations still in mind
  - uweigand
    - nothing, just help with ISLE unreachable rules stuff
    - quick Q: are we now enforcing SSA?
      - elliottt: asserts for no pinned vregs, not full SSA yet; need to add
        RA2 SSA checker to enforce
    - and reason is faster RA2?
      - cfallin: indeed, SSA allows multiple overlapping allocations for a vreg
        (because we know which way to do the copy when a new overlap starts and
        don't have to track "latest version"), and this simplifies a lot of
        other things / allows less splitting
