# May 9 project call

**See the [instructions](../README.md) for details on how to attend**

## Agenda
1. Opening, welcome and roll call
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. https://github.com/bytecodealliance/wasmtime/pull/6358
    1. Fuzzing: handling traps
    1. Calling into Wasm from epoch deadline callback

## Attendees

- Alex Crichton
- Jimmy Miller
- Saúl Cabrera

## Notes

* https://github.com/bytecodealliance/wasmtime/pull/6358

  - Consider dropping `winch_environ::FuncEnv`:

     - Have winch (`winch_codegen`) depend directly on `wasmtime_environ`.
       Having the `FuncEnv` trait might make testing easier right now but it's
       probably going to make maintenance harder as the trait implementation
       grows in size as we'd need to update multiple trait implementations
       (test vs proper) everytime we make a change.

     - The integration of Winch into Wasmtime, opens other options to
       test Winch which might be more insightful than testing `winch_codegen`
       in isolation: (i) fuzzing (ii) spec tests, etc.

  - Libcalls + pinned registers
    
    - Pinned registers (currently used by Winch) might not work with the
      libcalls.
    - Once Winch supports instructions that require libcalls (e.g.
      `memory.grow`) we'd need to see how much of an issue this is.

* Local fuzzing in Winch failing due to division-by-zero

  - Hint: we're probably not capture a trap when emitting a division; this
    could be fixed by emitting the right trap before the division.

  - Using `RR` as a way to diagnose the issue is another potential avenue that
    we could follow.

