# January 14 project call

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

- uweigand
- erikrose
- alexcrichton
- fitzgen
- cfallin
- jlbirch

### Notes

- uweigand: reviewed a few of Jimmy Brisson's patches for s390x, adding new instructions
  - a few don't map well to IR -- e.g. vector integer divide (no strong need
    now, maybe add later)
  - "vector eval" -- any boolean function
    - cfallin: tedious to match in ISLE -- maybe an expression simplifier?
    - fitzgen: generate all 256 rules? similar to how assembler for x64
      generates matching rules
- alexcrichton: no updates
- cfallin: Dagstuhl on egraphs, more below
- erikrose: no updates
- fitzgen: looked at Sightglass and improved some UX -- ETA for run; also
  looked at PCA (principal component analysis) to characterize benchmarks
- jlbirch: no updates

- stack overflow in x64 backend -- see #12333. Recursive rule that goes
  arbitrarily high up expression.
  - let's disallow recursion in backend; add a check; maybe opt-in in some
    cases.
  - fuel as an integer in context; "stack limit"; incorporate in logging as
    well

- cfallin: egraphs updates from Dagstuhl
  - overview of presentation
    - evaluated different variants: classical opt pipeline with modern rewrite
      rules; no subsume; no remat; "simple rewriter" that eagerly picks best
      option
      - most of these are in the noise; average eclass size is 1.13 nodes; hard
        to say we're getting benefit *right now* from multivalue, more the
        single-fixpoint-loop aspect
      - cost function enhancements -- Alexa, Nick and Chris are all thinking
        about this -- handling overlapping subgraphs, setting committed cost to
        zero once elaborated, etc
  - fitzgen: lower inside the egraph? union of CLIF and insts
  - post presentation publicly
  - add knobs for simple-rewrite upstream
  - (discussion about egraphs fused with lowering or fused with frontend)
    - cfallin: this is the ultimate cost function: actual cost of instructions
      resulting from different expressions
    - would be a separate lowering environment
    - fitzgen: lowering as single-shot constructor rather than
      multiconstructor, with rule priorities breaking ties?
      - cfallin: maybe
      - (but what to do about non-overlapping same prio rules?)
  - alexcrichton: how does this relate to sea-of-nodes discussion?
    - sort of orthogonal -- still have skeleton
  - optimization opportunities for evaluation -- tweak settings of Sightglass
    compilation

- platform-defined intrinsics
  - to what extent should we have platform-specific intrinsics/operators vs
    uniformity?
  - fitzgen: wait till needed
  - uweigand: vector integer divide is distinct from specific platform things
    that are "just weird".
  - fitzgen: backend-specific legalizations are the right long-term answer
  - vblend is an interesting case: was just x86, but now s390x has it too
  - pulley has intrinsics via special calls too
  - two buckets -- operators that just aren't defined anywhere, we can define
    in CLIF and have shared legalizations; but intrinsics (as accessed by name
    from `cg_clif` etc) are always platform-specific
    - fitzgen: `call_intrinsic` instruction?
    - fitzgen: interesting target for superoptimization too -- e.g. "the gzip
      intrinsic" could have a really optimized version inlined
    
