# September 03 project call

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
    1. Support for section flags like Mach-O mod_init_funcs (https://github.com/rust-lang/rustc_codegen_cranelift/issues/1588) or string merging in cranelift-object.

## Notes

### Attendees

- alexcrichton
- abrown
- bjorn3
- Rahul
- jlbirch
- cfallin

### Notes

- Section flags on macOS (bjorn3)
  - global constructors need a special flag set on section
  - LLVM handles this by parsing a section name with options after commas
  - yes, seems reasonable to add an API for this

- Updates
  - alexcrichton:
    - "how hard could it be to get rid of setjmp/longjmp in Wasmtime"
    - using exceptions to propagate traps
  - abrown: no updates
  - bjorn3: no other updates
  - Rahul: no updates
  - cfallin:
    - putting together an RFC to update our debugger plans; planning to start
      work soon

- Exceptions
  - alexcrichton: Wasmtime trap unwinding using exception handlers: what about
    fastcall? Trampolines are fastcall ABI on Windows
    - (lots of discussion about differing payload definitions)
    - bjorn3: the `try_call` itself is still tail ABI, that controls the
      definition of clobbers etc
    - cfallin: actually yeah -- we should verify that the callee ABI on
      `try_call`s supports exceptions, and handle payloads according to that,
      not according to the containing function's ABI
