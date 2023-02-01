# January 31 project call

**See the [instructions](../README.md) for details on how to attend**

## Agenda
1. Opening, welcome and roll call
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. Looping back to calling convention handling and callee-saved registers.
	1. Usage of `VMOpaqueContext`
    1. _Submit a PR to add your item here_
	
	
### Attendees

* Chris Fallin
* Kevin Rizzo
* Saúl Cabrera

### Updates

- Saúl:
  - Finished initial code generation for Aarch64: https://github.com/bytecodealliance/wasmtime/pull/5652
  - Helping with a proof-of-concept for host-to-wasm trampolines
  - Working on the remaining binary / unary instructions in x64
  
- Kevin:
  - Going over all the pieces for the Wasmtime integration
  
### Notes
  
#### Brainstorm calling convention and handling callee-saved registers

##### Alternatives

1. Exclude any callee-saved registers from register allocation.
   This will increase register pressure.

1. Save and restore callee-save registers at the trampoline prologue and epilogue.
   This requires saving the entire set of callee saved registers even if they
   are not used, but allows their usage in the target function (the one called
   by the trampoline), reducing register pressure.
   
We settled on alternative 2 to start. 

#### SP vs Shadow SP in Aarch64

The PR linked above, uses a shadow sp, which is always in sync with the real sp
to workaround the 16-byte alignment in Aarch64.

Things to verify for this approach:
- Signal handling: what happens if the real SP is unaligned and a frame for the
  signal handler gets pushed?
  
##### Alternatives to consider

- Use x28 as primary instead, and have SP aligned to x28 or above (always
  16-byte aligned, e.g. `and	sp, x28, #0xfffffffffffffff0`)
- Have a shadow stack pointer with a separate area of memory allocated for
  generated code to use as a stack.
- Allocate a fixed amount of memory upfront(16-byte aligned) and only allocate
  more if needed, using x28 as primary.
