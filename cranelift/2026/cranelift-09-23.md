# September 23 project call

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
    1. cfallin: Alias-analysis and semantics of regionless operations (#14323)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- thejimmybrisson
- fitzgen
- bjorn3
- cfallin
- adamrk
- Jacob Denbeaux

### Notes

- cfallin: Alias analysis and region semantics
  - issue 14323: question is whether a load/store with no region can alias
    anything, or only others with no region.
  - original semantics: "no region" is a separate region
  - fitzgen: more recent work tried to do a conservative thing -- stores with
    no region are not opting into mechanism
  - consensus: let's do the simpler thing and keep the original "no region is
    separate region" semantics

- Updates
  - thejimmybrisson: done dependency work to get Sightglass to run on s390x; PR
    for perf events
    - have benchmark results now! s390x seems to have a factor of ~2 in
      compilation and ~4 in execution vs x86-64
      - cfallin: comparable cycles in general? compare native bzip2 or something
        across architectures.
      - thejimmybrisson: compilation should be that?
      - cfallin: almost but unless cross-compiling on one side, the actual work
        is still different
      - fitzgen: new regalloc may be able to encode some s390x semantics better
        too -- stack-to-stack moves directly, also register groups
  - bjorn3: issue with updating to new Cranelift in `cg_clif` on macOS; looking
    into it
  - adamrk: no updates
  - Jacob Denbeaux: no updates
  - fitzgen:
    - digging into CLIF output of Wasmtime compile-time builtins, found issue
      in LICM, patch pending. Stack of loop headers maintained during DFS in
      domtree in elaboration; we popped loop headers when reaching block not in
      loop. Wrong visit order (see exit blocks before rest of loop body) would
      exit loops early, missing opportunity.
      - "a wash" on Sightglass, movement below 1% generally, except regex (+4%
        perf), bz2 (-1.5% perf), latter likely due to larger liveranges
    - new regalloc: added as new RA2 backend; using RA2's fuzzer; "almost
      fuzz-clean"
  - cfallin:
    - swept the split-cap in RA2 after reported issue; may raise from 2 to 3.
      Making the allocator more splitty improves the reported edge case but
      also leads to worse code in general past a point -- more move traffic is
      worse than a single spillslot + reloads where needed.  Raising to 10 is
      clear loss. Raising to 3 is mostly a wash, 2% code size increase, but if
      it fixes a few edge cases maybe it's fine.
