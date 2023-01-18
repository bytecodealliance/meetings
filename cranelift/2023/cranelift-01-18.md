# January 18 project call

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

- afonso360
- avanhatt
- bjorn3
- fitzgen
- uweigand
- cfallin
- jameysharp
- elliottt

### Notes

- afonso360
  - implementing some opts in the egraph framework
  - getting Wasmtime to work on FreeBSD/aarch64

- avanhatt
  - verification: last few Wasm-MVP things; signed-divide, traps, immediates

- bjorn3
  - working on preparation for `cg_clif`'s test suite in Rust's CI

- jameysharp
  - no updates

- uweigand
  - looking into fuzzing for s390x
    - found a bug in ABI code with extensions on i128
    - unimplemented operations: 128-bit divides, integer/float conversions
      - libcall?
          - cfallin: should be able to do this
          - cfallin: do other backends support i128-to-float?
          - uweigand: seems not
          - cfallin: disable this combination, or make it a libcall,
            probably...
    - fitzgen: running compile-only fuzzer or...?
      - uweigand: native fuzzing on s390x
      - fitzgen: surprised this works!
      - uweigand: all good but needs `-s none` (ASan not working)

- elliottt
  - merged BlockCall PR, rebased `brif` PR; some refactors

- fitzgen
  - RFC for tail calls in Wasmtime (including support in Cranelift)
    - most work so far in thinking about Wasmtime's trampolines
    - thinking about new calling convention

- cfallin
  - egraphs on by default -- PR up
  - blogpost on ISLE coming out

- i128 and legalization (via egraph rules)
  - fitzgen: legalizations can help fill out "all types that make sense" for
    e.g. i128 cases; share logic
    - need some way of customizing which rules are used
  - uweigand: on some platforms, GCC for example doesn't support i128; it's an extension
  - jameysharp: a few tricky bits
    - customizing which rules; different ISAs have a patchwork of native
      instructions for some cases but not others
    - what about no-opt case?
  - jameysharp: remove ValueRegs?
    - cfallin: ABI code uses it too; don't want to have to rebuild that
    - fitzgen: legalization-based ABI ugly; we used to do that, very hard to follow
  - cfallin: what about no-opt case? egraph alawys on if we do this?
    - fitzgen: maybe, depends on compile time

  - bjorn3: do it in backend instead?
    - fitzgen, cfallin: in principle yes, but tricky
    - no ctors for CLIF terms; recursively invoke lower?
      - but then no chance to optimize legalized form; better to do in mid-end

  - jameysharp: do we have enough in CLIF ops to lower to? add-with-carry, etc
    - if we don't have enough, we can add them
    - afonso360: open issue for shifts with carries (bytecodealliance/wasmtime#1044)

  - jameysharp/cfallin/uweigand/fitzgen: matching on add-with-carry is tricky:
    in general, quadratic for long chains, if user of carry matches
    add-with-carry that produces it and generates that add, etc. Need ability
    to match multiple outputs. Also inherently a code-scheduling problem:
    flags, only one carry flag live at a time. Want add/adc/adc/... chain if
    ops in sequence, or reify carry otherwise. Want for bignum support as well
    as i128 legalization. Needs more thought.
