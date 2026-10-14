# October 14 project call

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
    1. [Stores with later readonly loads](https://github.com/bytecodealliance/wasmtime/pull/14683) (@alexcrichton)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- theotherjimmy
- adamrk
- fitzgen
- alexcrichton
- cfallin
- Aelin
- erikrose
- jlb6740
- cpetig

### Notes

- Stores with later readonly loads (alexcrichton)
  - background: initialization of imported tables in Wasmtime is slow
    - initially tables were only funcrefs, then got generalized
    - right now for every element we need to do a ref.func then a set
    - libcall to do ref.func is slow; involves binary search to find a
      trampoline
    - should be simple to make ref.func compiled code!
    - except: VMFuncRef is tagged readonly; now we're writing
  - what should we do?
  - fitzgen: two things: what are actual semantics? and then implementation
    - weird semantics if we have "after first init" or something
  - cfallin: readonly but not can-move means that it's UB to hoist the load,
    but value can be dedup'd / GVN'd (won't change once initially loaded)

- Updates
  - theotherjimmy: Sightglass should now run on s390x on main branch
  - adamrk: no updates
  - alexcrichton: no updates
  - Aelin: playing around with s390x atomics, going well.
    - On s390x, 128-bit values are passed in memory; we only read into vec regs?
    - (discussion about atomics and acq/rel vs seqcst)
    - cfallin: a machine-model mapping question I think: i128s live in vec regs
      vs pair of GPRs
    - theotherjimmy: vec regs have one 128-bit lane versions of insts -- want
      to keep that in general
    - ultimately, vector store instruction from vec reg -- no need to move to GPRs
  - erikrose: no updates
  - jlb6740: working on APX enabling; pushed draft PR
    - RA2 patch, related, from Rahul
  - cpetig: no updates
  - fitzgen:
    - working on continuous benchmarking:
      https://bytecodealliance.github.io/are-we-fast-yet/
      - confirmed that it's filing issues based on observations, as expected
        from a recent lowering change
    - continued work on regicide (new regalloc). execution time now a bit
      faster (a few percent); compilation slower (200-ish percent), still
      pushing, may look at porting some wins to regalloc2 instead if that
      doesn't move
  - cfallin: lots of bugfixes from recent scans, nothing major to discuss
