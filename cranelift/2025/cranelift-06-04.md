# June 04 project call

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

- abrown
- alexcrichton
- bjorn3
- fitzgen
- Arjun Ramesh
- cfallin

### Notes

- Updates
  - alexcrichton: no updates
  - bjorn3: no updates
  - Arjun: no updates
  - abrown: progress on x64 assembler
    - custom hooks for regalloc, printing, etc on some instructions -- nice to
      have escape hatch
  - cfallin: next step of exceptions in CL/Wasmtime: wrote unwinder crate,
    integrated into clif-utils

- abrown: compiler builtins RFC -- how does it intersect with Cranelift?
  - fitzgen: depends on approach we take -- we might reuse Wasm translation
    entirely, we might need to build an inliner (driven by embedder's explicit
    instructions), ...

- cfallin/abrown: xmm1/xmm2/xmm3 operands in assembler -- can we have just
  "xmm" and tie it to an operand slot in DSL?
  - abrown: simpler in the end to do this, and reflects what the manual says
  - eventually: we'll refactor to separate out (names and types) -- Alex will
    file an issue

- unwinder
  - naming: wasmtime-unwinder? Concern is that `cranelift-unwinder` suggests
    that this is the only way (or a necessary part of) unwinding with
    Cranelift; the format is really Wasmtime-specific and we just factored it
    to allow using it in `clif-util` as well.
    - sure; `cranelift-tools` can depend on `wasmtime-unwinder`
  - separate crates for Cranelift metadata translation (compilation) and
    runtime?
    - probably not; instead, ensure that it builds without the runtime bits,
      i.e., just missing (refactor the `arch` module's facade to use
      conditional `pub use` of `imp` modules rather than an outer function)
  - bjorn3: extern "C" vs extern "C-unwind" and UB when unwinding past?
    - alexcrichton: we call longjmp today and we don't abort -- how does it
      work?
    - will file followup issue for Wasmtime
