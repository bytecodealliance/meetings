# November 16 project call

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
    1. Can we remove `get_pinned_reg`/`set_pinned_reg`?

## Notes

### Attendees

* @abrown
* @afonso360
* @bjorn3
* @cfallin
* @elliottt
* @fitzgen
* @jameysharp

### Pinned Registers

* elliottt: can we remove get_pinned_reg/set_pinned_reg?
  * cfallin: there are two interesting cases for this: the spidermonkey heap
    base, and the fuel counter
  * cfallin: spidermonkey isn't planning to use cranelift anymore
  * fitzgen: the fuel PR has other problems that prevent it landing
  * cfallin: we could look at supporting something similar in the future by
    improving the register allocater's handling of long-lived values

# `heap_addr` discussion

* cfallin: we can gvn but not licm `heap_addr` instructions
* fitzgen: `heap_addr` is a bit of a pain, why don't we get rid of it and lower
  it directly in cranelift-wasm instead?
* cfallin: we could remove the concept of heaps entirely from cranelift, or we
  could bring the notion of wasm heap access directly to the lowerings in
  cranelift.
  * fitzgen: the latter complicates optimization substantially, and lowering
    heap accesses much earlier would simplify things
* cfallin: suggestion: let's use `uadd_overflow_trap` and make it gvn-able
* cfallin: the `iadd_cout` approach sounds like it's going to add complexity
  over the `uadd_overflow_trap` approach
* cfallin: not clear that our codegen will be very good with the `iadd_cout`
  approach
  * fitzgen: the current codegen could really be improved, which is why I'm
    investigating this
  * cfallin: we do also need the behavior of `iadd_cout`
* fitzgen: I'll prototype multiple outputs in ISLE, and add an issue for
  removing heaps from cranelift

# `iadd_cout` discussion

* elliottt: should we explicitly not handle all types for `iadd_cout`?
* bjorn3: handling i128 would help with detecting overflow for additions with
  i128
* fitzgen: we don't currently support it for any bitwidth, so any support in a
  backend is an improvement
* fitzgen: i'll split the run tests up so that we can better test partial
  support in a backend
