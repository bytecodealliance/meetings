# January 08 project call

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

- cfallin
- amanieu
- alexcrichton
- abrown
- fitzgen

### Notes

- Updates
  - alexcrichton: lots of contributions to Pulley; SIMD now complete too; now
    looking at optimizations, macro-opcodes, especially for Wasm memory ops
  - fitzgen: no updates
  - abrown: worked more on Cranelift assembler; parameterizing assembler on
    different kinds of registers; got that working. Still having a few issues
    with ISLE
  - Amanieu: no updates
  - cfallin: no updates

- Amanieu: using regalloc3 in Cranelift?
  - RA3 is new register allocator started ~a year ago; inspired by RA2's
    algorithms and techniques, but improved
    - https://github.com/Amanieu/regalloc3
  - about 5% perf improvements on bzip2 and pulldown, 2% on SpiderMonkey.wasm,
    on "splitting" variant with some (hopefully possible to fix) slowdowns;
    "spilling" is ~at parity on both metrics
  - alexcrichton: single-pass too? Amanieu: not yet
  - alexcrichton: adaptation layer? Amanieu: yes, ~10% overhead
    - cfallin: would be great to see a datapoint with the adaptation done
  - fitzgen: would be great to stay with a single interface, so we can keep RA2
    as an option
  - cfallin: social aspects from Zulip thread: code review, making this a BA project
    - Amanieu: yes absolutely fine with this
    - abrown: means that we are comaintainers too
  - plan: get RA3 to be reviewed and published, and integrate as RA2 backend
    first; get comfortable with it; then eventually implement ra3::Function in
    parallel with ra2::Function directly in Cranelift; then migrate completely
    if we're seeing improvements

- abrown: CL assembler -- deep-dive in assembler types and making
  autoconversions work
