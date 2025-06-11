# June 11 project call

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

* Nick Fitzgerald
* Andrew Brown
* Alex Crichton
* bjorn3
* Erik Rose
* Rahul Chaphalkar

### Notes

* Updates:
  * bjorn3: no updates
  * erik: no updates
  * rahul: two PRs for the new x64 assembler
  * alex: little things here and there, migrating instructions to the new x64
    assembler
  * andrew: more new x64 assembler stuff, linking sse and avx variants of
    instructions to automatically use the best version available
  * nick: turned conditional branches to unconditional traps into conditional
    trap instructions in legalization, allows mid-end to GVN them
    * (some discussion around branch folding and const propagation through
      conditional branches)
* alex: traps and setjmp/longjmp
  * would like to remove setjmp/longjmp from wasmtime, one of the very few
    places where we have to use C instead of Rust, have previously talked about
    using exceptions infrastructure to do this stuff in JIT code instead of C
  * but setjmp/longjmp is O(1) unwinding, exceptions are O(n) unwinding. this
    might not actually matter at all though?
  * VMStoreContext has initial FP and all that, bounds JIT frames, data saved in
    our entry/exit trampolines
  * what if we had another trampoline in between those and wasm, that saved
    FP/PC of where to "longjmp" to and just go there directly?
  * (discussion about how to factor searching for unwind target such that traps
    just look in the VMStoreContext and exceptions do a stack walk)
  * (discussion about how to get a particular call's return address in CLIF by
    having a `ir::GlobalValue` symbol that is resolved at static link time to
    the call's address)
  * goal: all unwinding happens in JIT code, no need to worry about `extern "C"`
    versus `extern "C-unwind"`
* andrew: tracking partial writes to registers
  * have historically had bugs where we accidentally used undefined bits of
    registers
  * have WIP for defining which bits are written to (and ultimately defined) at
    the assembler level:
    https://github.com/bytecodealliance/wasmtime/compare/main...abrown:wasmtime:asm-partial-write
  * how to handle this stuff at the ISLE level?
  * (discussion around existing invariants about which bits are defined in ISLE
    lowering rules)
  * (discussion about `inst.isle` versus `lower.isle` abstraction levels; might
    make sense to model a lot of this at the `inst.isle` level, but wrap up in
    higher-level helpers for `lower.isle`)
  * (discussion about this stuff surfacing via false dependency perf bugs)
  * (discussion about a translation validation pass to check that only defined
    bits are ever used)
* alex: `throw` intrinsic: would ideally like to have this in JIT code, not host
  code
  * still search in host code, but unwind in JIT code
  * (discussion about the details of a CLIF `throw`/`longjmp` instruction
    tailored to our use case; seems doable)
