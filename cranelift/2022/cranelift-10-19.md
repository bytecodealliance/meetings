# October 19 project call

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
    1. Removing redundant branch and select instructions

## Notes

### Attendees

- elliottt
- fitzgen
- avanhatt
- uweigand
- afonso360
- abrown
- saulcabrera
- jlbirch
- cfallin

### Notes

- elliottt: redundant branch and select instructions, remove?
  - bricmp, brfcmp, selectif, selectff, ...
  - any reason to keep?
  - ulrich: heap expansions use them in legalization
    - we can change these though

- cfallin
  - making egraphs faster: compile-time regressing a bit for some reason
    (didn't happen in prototype branch, appeared after rebase)
    - tried some things; PR to do a few misc optimizations (for 2%)
    - tried to make egraph not copy InstructionData out and back (just refer
      back to DataFlowGraph); overhead of indirection seemed to cancel out
      gains
    - some more ideas, will try a few more things

- elliottt
  - removing bools (!!)
  - s390x backend producing SSA (`reg_mod` removal)
    - some more to do after

- afonso360
  - PR to remove iconst.i128; found during fuzzing; running fuzzing again

- saulcabrera
  - initial Winch (baseline compiler) skeleton ready for review! cfallin will
    look
  - starting to extract Cranelift assembler level to `cranelift_asm` to share with Winch

- avanhatt
  - verification engine: Wasm MVP minus floats, on aarch64, is initial goal;
    making good progress

- abrown
  - back to flags: in favor of simplicity; thinking back to prior conversations
    about why they're there 
    - need to express flags use for performance reasons?
    - need to make explicit (no implicit dataflow)
    - will we need to rematerialize value in an i8 etc?
  - cfallin: we match on whole pattern already (br of icmp, select of icmp); no
    change to generated code
  - abrown: safety/correctness? iflags only one live etc
    - elliottt: we regenerate cmp at each use (already)

  - PR from contributor for band, bor, ... for scalar FP types on x86: mostly
    looks good, abrown will merge after some additional thoughts (?)


- uweigand
  - review of Trevor's s390x patch for SSA-ification
    - r0, r1: unfortunate we can't use either.
      - r0 is special (no amode use); r1 is spilltmp
      - really we need three things from RA: r0 is special (overlapping class);
        no spilltmp need; ability to allocate pairs

- jlbirch
  - benchmarking PR
    - making results nicer
    - trying to make results more stable
    - get PR in and do other improvements incrementally?
      - cfallin: sure thing, onboard with that
      - stability/confidence interval comments needed for basic use, but we can
        keep working on it
  - difficult to iterate on this (takes 50 minutes to run)
    - abrown: something fitzgen suggested: most of the report-producing code
      should actually go in a Sightglass crate; then we can iterate on it
      locally
      - jlbirch: still takes time to run locally
      - abrown: fewer iters locally too (when testing)
  - abrown: correct understanding fitzgen's comments that most should be in
    Sightglass?
    - fitzgen: yes, GH Action should just be the plumbing needed to run
      Sightglass (auth etc). Not good if half of benchmarking logic is in GH
      Action
    - cfallin: useful to enable "run the same thing locally" as well
    - fitzgen: can we just emit the standard Sightglass output (stdout/stderr)?
      Then later we can evolve a new output mode
    - abrown: we can have issues on Sightglass issue about output format

- fitzgen
  - perf experiments with arena-btree in RA2
    - larger context: profiling with DHAT (allocation profiler), finding excess
      allocations, etc
      - top of list was btree nodes for RA2's liverange maps
    - after lots of measurement, seems there isn't actually much to gain
      - lots of allocs, but all at start of regalloc, and stable; and cost of
        alloc/free is much less than actual work
        - reuse (free to another alloc) is only about 5% for btree nodes
      - given all that, arenas wouldn't be expected to have an impact
    - next: ABISig optimizations, replace many small SmallVecs with one big Vec
    - next next: bitpacking hacks, make data structures smaller, fit more in
      cache
    - abrown: other higher-level RA2 work? inefficient algos etc?
      - fitzgen: cache efficiency
      - fitzgen/cfallin: segmented linked list to let LR/Use lists be built on
        arena but still built all in parallel (vs. single large Vec)

- elliottt: s390x, r2/r3, thoughts from Ulrich
  - Ulrich: main issue is that it still hardcodes a pair; but we still do that
  - Ulrich: not using r0/r1 increases reg pressure, but in the big picture
    maybe not a huge deal
  - Ulrich: totally fine to merge for now
  - Ulrich: callsite remove uses of pinned vregs next?
    - elliottt: yup
    - Ulrich: will take a look
    - elliottt: thanks!
