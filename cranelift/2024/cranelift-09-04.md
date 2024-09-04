# September 04 project call

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

* abrown: avx10 and apx
  * avx10 is most relevant for vector operations
  * apx gives more registers, and is applicable outside of vector operations
    * apx also relaxes some operand constraints for specific registers
    * some regalloc2 considerations, as we'll want to conditionally emit reuse
      constraints for instructions that have their constraints relaxed. we'll
      also need to pass in a different MachineEnv that exposes more registers
  * cfallin: for instructions that support both formats, we could rework the x64
    lowering rules to emit encoding agnostic instructions that decide their
    encoding at emission time instead of at lowering time. this would simplify
    supporting new formats in the future
* bjorn3: exception handling
  * <https://github.com/bytecodealliance/regalloc2/issues/186>
  * small amount of cleanup necessary before making a pr for exception handling
    support in cranelift
* alexcrichton: 128 bit ops
  * other runtimes are pushing for overflow ops
  * llvm wasm codegen for overflow ops was very bad, but modeling the overflow
    as a single flags register that was costly to move into/out of produced much
    better code
  * added a way to ask if a value was dead in isle
    * this allows us to ask if an overflow flag is dead, and skip codegen for it
      if that's the case
  * perhaps we could have pseudo-instructions that would move into/out of
    eflags, that would help us to avoid codegen for some cases?
  * cfallin: should we be pushing on the overflow flags approach, given that
    you've already proven the 128-bit implementation?
    * alexcrichton: yes and no. pushing on the overflow flags implementation
      will either prove the path, or give more evidence in support of the
      128-bit ops approach
  * cfallin: can binaryen support the overflow flags approach?
    * alexcrichton: they can't support multivalue, so there would be other work
      for them to do there
  * abrown: meta comment: trying to implement the overflow flags version in v8
    would probably get more traction, as you'd be able to explain in their terms
    why the implementation is problematic
  * cfallin: if it's decided that the overflow flags approach is the best, we
    can fall back on supporting multi-result instructions in the mid-end, and
    rewriting those instructions into the 128-bit form that we want

### Attendees

* abrown
* avanhatt
* alexcrichton
* bjorn3
* cfallin
* elliottt

### Notes

* abrown
  * avx10 would be a great improvement to the x64 backend, but we need to
    prototype that and maintain backwards compatibility. what's the best way to
    acomplish that?
* elliottt
  * fitzgen and i removed safepoint support from regalloc2 last week, and have
    merged those changes into cranelift
* alexa
  * pull request is up for the verifier
    * <https://github.com/bytecodealliance/wasmtime/pull/9178>
  * cargo deny and cargo vet errors remain
    * alexcrichton will investigate
* cfallin
  * interesting discussion around exceptions, and control flow to exceptional
    blocks
