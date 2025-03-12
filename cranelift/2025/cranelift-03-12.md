# March 12 project call

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

- alexcrichton
- abrown
- uweigand
- fitzgen
- cfallin
- bjorn3

### Notes

- Updates
  - uweigand: no updates
  - cfallin: no updates
  - abrown:
    - working on the assembler
  - alexcrichton:
    - aarch64 panic, filed issue (from Robbepop, wasmi author)
    - updating fuzzing to have a single target to better timeslice our time on
      OSS-Fuzz
      - fitzgen: could we remove the API fuzzer? Maybe covered by others at this point?
      - alexcrichton: always wary of deleting fuzzers, maybe good to still keep
        it
      - alexcrichton: maybe going to look at building something from an actual
        grammar for embedder API
  - bjorn3: no updates
  - fitzgen: heads-up about `can_move` memflag -- discussion with alexcrichton
    and cfallin about new semantics on loads to be more precise about hoisting;
    we were unsafely hoisting loads before
    - https://github.com/bytecodealliance/wasmtime/pull/10340
    - bjorn3: most optimizing compilers don't preserve data dependencies; e.g.
      LLVM can speculate
      - cfallin: we wouldn't speculate on these ones

- abrown: pretty-printer issue in new assembler

- exceptions:
  - bjorn3: did implement this in bachelor's thesis; but then found issue with
    interactions with regalloc, could insert moves between call and jump to
    landingpad
  - cfallin: no worries, we have a design we think will work in the RFC, I'm
    starting work soon; if you want to show yours as another reference point
    that might be useful
    - https://github.com/bjorn3/wasmtime/tree/exception-handling
    - https://github.com/rust-lang/rustc_codegen_cranelift/tree/exception-handling
    - https://github.com/bjorn3/eh_frame_experiments
  - bjorn3: design in RFC will work for rustc unwinding as well

- alexcrichton:
  - interesting discussion on wide-arithmetic Wasm proposal. V8 folks want
    `add2` / `add3` instead with explicit carry flags instead
    - https://github.com/WebAssembly/wide-arithmetic/issues/6
