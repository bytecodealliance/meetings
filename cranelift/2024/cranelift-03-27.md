# March 27 project call

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

- jameysharp
- fitzgen
- alexcrichton
- elliottt
- uweigand
- abrown
- cfallin

### Notes

- Updates
  - fitzgen: no updates on Cranelift; working on Wasm-GC, no real compiler
    changes required
  - elliottt: got callee-saves working with tail-call convention for x64! also
    working on fuzzbugs on Winch calling convention
  - uweigand: no updates; will get to looking at s390x tail-call ABI
  - alexcrichton: fiddling with disassembly tests; moved Winch tests over to
    toplevel disas tests; may remove toplevel Winch binary as it's no longer
    needed now?
    - elliottt: trampolines not there yet on aarch64, will this be ok for
      running tests there?
    - alexcrichton: stubbed them out for now
  - abrown: working on new wasm threads proposal "shared-annotations"
    implementation; will impact Cranelift at least in the Wasm translator
    - should we have new `global.get` for atomics, etc?
    - no: Cranelift's version of global values is different than Wasm globals
      (former are used for latter in readonly case); in general we're trying to
      keep Wasm primitives out of CLIF and move that way
  - jameysharp:
    - tail call callee-saves on x64!
    - optimization for `select_spectre_guard`
    - deleted cranelift-wasm's "dummy environment"
    - getting rid of value aliases in disas tests and in printed CLIF
    - optimizing Cranelift lowering for Wasm tables -- base address readonly
      when possible, lots of other thoughts
  - cfallin:
    - worked on PCC fuzzbugs; resolved a few oss-fuzz ones that came in; found
      that the target I was using out of habit (`differential`) was very bad at
      finding them, more found with `instantiate`; turned off PCC fuzzing for
      now and will address later
    - filed issue about how to avoid dynamic bounds-check in fallback case with
      static memories (offset too large); thanks Jamey for discovering that we
      need to set static limit lower and guard region to 4GiB instead (should
      we do this by default?)

- jameysharp: tail calls! working callee-saves on x64!
  - original tailcall implementation: build a new stack frame as if for a
    normal call, below existing frame; then memcpy frame up on top of current
    frame; needs some temp registres, etc
  - key idea instead: move *current* stack frame instead to make the argument
    area the right size; in the common case, they're the same size, so no move
    is needed at all
  - this now allows reusing epilogue-generating code for restoring clobbers and
    popping stack frame
  - a little extra logic needed in `gen_arg` to rework argument setup to use FP
    rather than SP to reach argument area
  - elliottt: coolest part was once plumbing was in place, just changed
    callee-saved set and it worked!
  - fitzgen: use parallel move resolver eventually?
    - through regalloc2 constraints (stack constraints), not directly from
      Cranelift
    - doing this does mean that we'll need to relax an invariant we have right
      now: after args pseudoinst, we don't read from arg area
    - eventually, use regalloc2 with FP-offset constraints to do all the moves
    - eventually eventually, can translate FP-offset Allocations to SP-relative
      once we know the size of the frame
    - that translation implies a back-and-forth: Cranelift needs to say how big
      the frame is, but needs first half of regalloc2 results (clobbers, number
      of spills)
    - can we push more ABI logic into regalloc2? probably not, hard to specify
      constraints that give exactly the right instruction sequences
  - s390x:
    - uweigand: can we allocate extra arg space in prologue, then move stack at
      very end (on all architectures)?
      - implies that we have some extra gap we need to account for when we
        access our own stack args
    - may work and allow us to not move whole frame
    - we can land existing PR and then come back and optimize
  - after landing, let's benchmark, let it fuzz, turn it on by default!

- cfallin: CLIF global values (came up above)
  - we should keep them, they serve a specific purpose: "timeless" constant
    values that can parameterize other kinds of CLIF entities, e.g. dynamic
    vector length
  - fitzgen: indeed; we can use them for that, when we have a kind of
    stratification, let's not rely on them to implicitly encode constantness
    for optimization though (e.g. read-only globals)
    - no argument there!
