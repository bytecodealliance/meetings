# May 22 project call

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
    2. fitzgen: moving stack maps, safepoints, and knowledge of GC references into `cranelift-frontend`

## Notes

### Attendees

- elliottt
- fitzgen
- abrown
- jameysharp
- alexcrichton
- cfallin

### Notes

- fitzgen: moving stack maps, safepoints, and knowledge of GC moving out of
  core cranelift into frontend
  - work started with elliottt
  - current design needs reworking to support non-word-sized GC refs (e.g.
    Wasmtime's 32-bit indices/offsets even on 64-bit hosts) -- one bit in
    stackmap is one 64-bit word
  - want to be able to do certain operations on GC refs, such as adding to
    base, etc. Result is not a GC ref, but is derived from one. bitcasts are
    footguns because they can be dedup'd, and liveranges of resulting IR are
    not the same.
  - goal: move all GC knowledge into frontend; insert safepoints during IR
    finalization in frontend as explicit dataflow
    - will still compute and use liveness; otherwise we'll keep all GC refs
      alive all the time, major pessimization
    - moving GC is interesting: new refs are outputs of every safepoint
    - possible alternative: stackslot loads/stores non-side-effecting?
      - cfallin: could track perfect aliasing of stackslots if we give them
        names, then carry liveness through them
        - fitzgen: fair, but new complexity
        - jameysharp: if we extend alias analysis to have dynamically-indexed
          alias regions, we could reuse that, not much more complexity?
        - cfallin: still need dead-store elimination in midend, which would be new
        - clarification: explicit loads and stores inserted, not part of
          safepoint instruction
        - also we'd need to remove unused stackslots
        - cfallin: maybe better to do liveness in frontend; otherwise need more
          generic algorithms (dead-store, unused stackslot) and more work in
          midend over potentially many extraneous GC vals
    - jameysharp: need a "volatile" memflag? non-trapping, but still opts we
      can't do
      - fitzgen: distinction may not be much in practice because there will
        always be calls after stores and so every store is needed to be visible

- fitzgen: interpreter for Wasmtime involving new backend to Cranelift
  - using Cranelift to generate optimized bytecode that interpreter VM can run
  - have a simple prototype working with some arithmetic
  - will write an RFC
  - abrown: replace CLIF interpreter?
    - fitzgen: different purposes; CLIF interp is good oracle for fuzzing, this
      is for performance

- status
  - jameysharp:
    - inlining in ISLE rules, issue created with thoughts
      - relevant for verification folks, but also worth thinking about for
        codegen improvement in islec (more matches in same scope)
    - multiplication, `__multi3` optimization (LLVM compiler-rt helper for wide multiplies)
      - draft PR with rules that recognize lower half of its body and rewrites
        into a single wider imul; still TBD matching upper half, interesting
        discussion in PR
    - some students looking at instruction scheduling in egraph, built a
      prototype, implemented algorithm described a year ago in issue; will work
      with them
  - alexcrichton: no updates
  - abrown: no updates
  - elliottt: removed virtual SP adj and nominal SP from ABI code; SP always
    stays at end of outgoing args region, only modified in prologues and
    epilogues (nice!) and in tailcall sequence. Consistent view of frame,
    matches s390x now.
  - fitzgen: thinking about GC refs and safepoints; backend for interpreter
    bytecode
  - cfallin: no updates
