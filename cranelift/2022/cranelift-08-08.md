# August 8 project call

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
    1. [Endianness and vector types/opcodes](https://github.com/bytecodealliance/wasmtime/issues/4566)
    1. ["Winch" baseline compiler](https://github.com/bytecodealliance/rfcs/pull/28): factoring/sharing of Cranelift's machine-code layer
    1. _Submit a PR to add your item here_

## Attendees

* jameysharp
* cfallin
* saulecabrera
* uweigand
* akirilov-arm
* bnjbvr
* fitzgen
* bjorn3
* afonso360
* abrown
* jlb6740
* elliottt

## Notes

### Endianness

* clarify `raw_bitcast` semantics
  * all current uses of `raw_bitcast` in cranelift assume little-endian lane
    order semantics, because they're driven by web assembly
  * a use of cranelift like `cg_clif` using a big-endian backend now would
    benefit from using big-endian lane order, as it wouldn't be necessary to
    fix-up indices or values
* fitzgen: one option would be to only have `v128` and remove `raw_bitcast` if
  possible
* uweigand: `swizzle` allows you to specify an output type, implying a
  `raw_bitcast` of the result
* jameysharp: can this be fixed up when going between memory and registers?
  * cfallin: you can have programs that don't have loads and stores that still
    expose this problem
* abrown: if the little-endianness of the value and the lane order are both
  flipped, does it just work?
  * uweigand: lane order no longer matches what the hardware does, but we can
    fix that up in the backend, and that is what's implemented today

* webassembly assumes little-endian memory for vectors, so we must byteswap for
  a big-endian target

* there's a choice in the backend: be lane order or le lane order
  * the choice is visible in the ABI: SystemV requires BE lane order for a BE
    target
  * ideally, we would make a choice about lane ordering per ABI

* we can implement either choice in the implementation: it's a no-op on the
  native architecture and a permute otherwise

* fitzgen: we should rename `raw_bitcast` when working with vector data

* fitzgen: what is still using `raw_bitcast` but not with vector types?
  * ideally we would remove it
  * abrown: it's nice to keep `raw_bitcast` as a no-op, as that means that we
    can remove it whenever we like -- it would be great if `raw_bitcast` was
    always a no-op

* abrown: if everything is a no-op, do we need the le/be variants of
  `raw_bitcast`?
  * cfallin: we want the semantics to be determined by the abi of the function
    being called, so allowing inlining between functions requires that the
    semantics of those operations survive inlining; the presence of those
    variants means that the lane order endianness is preserved even when the
    function is inlined

* abrown: it seems like we need to rename this op into something more like
  `vector_reinterpret_cast`
