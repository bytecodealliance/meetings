# April 16 project call

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

- fitzgen
- abrown
- bjorn3
- alexcrichton
- cfallin

### Notes

- Updates
  - alexcrichton: no updates
  - bjorn3: working on getting `cg_clif` to compile using `try_call`
    - generating exception tables for system unwinder; unwind works
    - finding a miscompilation somewhere with normal return path, working on
      debugging
    - PR up to make `try_call` work with cranelift-frontend
  - abrown:
    - worked more on assembler; adding instructions that handle flags
      (`ProducesFlags`/`ConsumeFlags`)
    - found differential fuzzing crash, need to go back and find it
      - (probably related to float helpers changing on nightly?)
  - cfallin:
    - fixed RA2 infinite loop related to try-call constraint handling
      - simplified minimal-bundle logic and splitting a bit
    - fixed RA2 checker to understand try-calls
    - fixed egraphs blowup with exponential chain of selects: limit size of
      eclasses directly, and add a strategic `subsume`
  - fitzgen:
    - added an optimization for memories: when min and max size are the same,
      it will never move, so make base/size loads readonly

- alexcrichton: more timeouts on OSS-fuzz; take a look?
  - cfallin: can take a look at some point
  - alexcrichton: wasm-smith tends to create very high levels of live values;
    may not be too surprising, not high priority
  - (some discussion about regalloc, different options)

- abrown: new PR for flags-producing insts in x64 assembler
  - still handwriting which instructions produce/consume
  - alexcrichton: branch somewhere trying to model eflags and generate so it's
    known correct
  - abrown: some instructions modify some but not all flags ...
    - we could model individual flag bits, or groups of them; maybe too much
      complexity
  - eventually clean up flags handling in general:
    https://github.com/bytecodealliance/wasmtime/issues/10298
    - probably hold off smaller refactors to `ProducesFlags` mechanism to see
      where the overall direction here goes

- alexcrichton: rename x64 to x86, `x86_64`, or something else?
  - cfallin: history -- old x86-64 backend was `x86`, but now it's gone
  - cfallin: x86 has a "32-bit flavor" to me
  - will we have a 32-bit x86 at some point?
    - abrown: new assembler does support noting which instructions support 32
      bits; then a matter of different register env and lowering rules (and
      ABI, ...)
  - abrown: how to handle different sets of regs? 8 32-bit regs, or APX 32
    regs, or ...?
    - constraints, or different environments for different modes
  - (side discussion about x86-32, segmentation, Wasm bounds-checking)
  - end result: no one feels strongly enough about backend name to make a
    change here
