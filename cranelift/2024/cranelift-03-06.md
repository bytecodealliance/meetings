# March 06 project call

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

- elliottt
- fitzgen
- alexcrichton
- saulcabrera
- jameysharp
- avanhatt
- cfallin

### Notes

- Status
  - fitzgen: no CL updates; but starting to think about GC-related issues
    - use `r32` on 64-bit architectures; limited by stackmaps with
      one-bit-per-machine-word format
    - what optimizations can we do on ref values when we have moving
      GC? Interaction of GVN and code motion. Will need to thread data
      through safepoints as explicit operators
  - avanhatt: no major updates, continuing work on veri-isle
  - jameysharp:
    - working on egraph pass and auto-subsume ("available block" idea)
      - later, question whether we still want subsume for perf
        reasons; but we'll get this change in first
    - interesting paper: "Indexed Types for a Statically Safe WebAssembly"
      - https://williamjbowman.com/resources/geller2023-wasm-prechk-current-preprint.pdf
  - saulcabrera: work on Winch
    - resolving fuzzbugs; two pending right now
    - starting to look at perf
    - discussing ABI changes with Trevor for trampoline reuse wrt Cranelift
    - project board to coordinate?
  - alexcrichton: no CL updates; looked at Indexed Types paper as well
    - Wasmtime changes were trivial (removed code/checks); wasm-tools
      Wasm validator used Z3 (!) but maybe not fundamental
  - elliottt: pushing Winch forward; started on component model
    trampolines; decided to reuse Cranelift trampolines instead, which
    needs harmonized ABI between Winch and Cranelift
  - cfallin: code review on Jamey's egraphs work

- jameysharp: discussion with John Regehr about their work Hydra,
  generalizing Souper-discovered rules; they will look at applying
  this to Cranelift
  - fitzgen: back in the day, "closed the loop" with Peepmatic
    (feeding Souper results back into automatically generated rules);
    would be good to build automated tooling in general
  - fitzgen: would be good to have profile-guided extraction of the
    most interesting rule left-hand sides
  - cfallin: what if we don't profile-guide and just generate all the
    rules? we've always claimed rules are cheap-ish because of merging
    - jameysharp: not sure of algorithmic complexity but maybe!
    - alexcrichton: main limiting factor will be Rust compiler
    - split `constructor_simplify` into multiple functions?
    - cfallin: pragma on some extractor to split on its result
      (e.g. opcode)? or maximally split and rely on inline
  
