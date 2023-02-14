# February 14 project call

**See the [instructions](../README.md) for details on how to attend**

## Agenda
1. Opening, welcome and roll call
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. PoC between Wasmtime and Winch for simple functions.

	1. Scope of the initial skeleton Winch <> Wasmtime 
	  - Adding a `winch` feature flag
	  - Support for the untyped `Func#call` API
	  - Expose `MachTextSectionBuilder`
	  - Host-to-Wasm trampolines (generic arguments, single return)
	  - CI (`aarch64`, `x64`)

	1. After landing the initial skeleton
	  - Support for the `VMContext` 
	    - Use a `callee-saved` register excluded from `regalloc`
	  - Stack checks
	  - Dealing with callee-saved registers
	  - Support for the `call` instruction
	    - Relocations
	  - Access to tables and memories
	  - Windows ABIs

## Notes
