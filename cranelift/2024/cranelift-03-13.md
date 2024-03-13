# March 13 project call

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
    2. fitzgen: operations on narrow types that get lowered to operations on wider types and that effect on memory accesses
       * https://github.com/bytecodealliance/wasmtime/issues/8116
       * https://github.com/bytecodealliance/wasmtime/issues/8112
       * https://github.com/bytecodealliance/wasmtime/pull/8113
       * can we better fuzz these things?

### Attendees

* abrown
* alexcrichton
* avanhatt
* cfallin
* elliottt
* fitzgen
* jameysharp
* lpereira
* uweigand

## Notes

* fitzgen: how should we prevent this sort of bug in the future?
  * abrown: why didn't wasm-smith find this?
  * fitzgen: it has to be right on the edge of memory, and the sequence of
    instructions has to be just right. perhaps we can look again into biasing
    constants when generating inputs (right near the end / just past the end of
    memory etc)
  * cfallin: we could try to generate better values, though we could also look
    into better oracles. pcc would have caught this, as the access size would be
    too large
  * fitzgen: we don't have to choose one approach, and we should do both; adding
    interesting value generation is easy
  * alexcrichton: wasm-smith has the notraps mode that we should enable
  * fitzgen: can we introduce newtypes to avoid this in the future?
  * cfallin: it would be a large refactor, but yes
  * fitzgen: how much of this is a reflection of the underlying isa? is this a
    reflection of the operations that exist for xmm registers vs gpr registers?
  * cfallin: we could remove the implicit converter that allows for load fusion
    with floating point values?
    * we could name the existing function something like `maybe_fuse_load`
  * avanhatt: would it make sense to have the implicit conversion happen only if
    the type was valid?
  * jameysharp: can we have the implicit conversion only produce the memory
    variant of `XmmMem` when the loaded value would be 128-bits wide?
  * alexcrichton: we could also have `XmmMem{4,8,16}` to indicate how many
    initial bytes of the register are used
  * cfallin: let's audit every pseudo-instruction with internal control flow,
    and then ensure that all of those instructions are annotated with their
    width
  * fitzgen: the wasmtime misc test suite should be run in different
    configurations as well (like the spec test suite)
    * alexcrichton will take adding this wast test case
* jameysharp: almost every use of `tableaddr` is followed by a load or a store.
  the only exception here is for an `externref` with gc turned on, where some
  reference counts are incremented before the load.
  * fitzgen: we can move loads before the increment at the potential cost of
    additional register pressure, but we can't move incref before decref.
    removing the `tableaddr` instruction entirely and relying on wasm semantics
    is where we want to get to
* jamesharp: we should really determine what optimizations are legal for
  `select_spectre_guard`, as it would be really nice to be allowed to constant
  fold them (deferred to 2024-03-20)

### Updates

* jameysharp
  * working on removing support for tables in cranelift, and simplifying the
    code generated for tables in cranelift-wasm
* lpereira
  * hacked together a proof-of-concept stat tracking framework to investigate
    `SmallVec` sizes
    <https://github.com/bytecodealliance/wasmtime/pull/8107>
* elliottt
  * switched wasmtime-winch to depending on wasmtime-cranelift for component
    model trampolines, and am in the process of making this change for all
    trampolines as well
