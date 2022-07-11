# July 11 project call

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

### Attendees

* Afonso Bordado (afonso360)
* Anton Kirilov (akirilov-arm)
* Jamey Sharp (jameysharp)
* Johnnie Birch (jlb6740)
* Nick Fitzgerald (fitzgen)
* Trevor Elliott (elliottt)

### Notes

* afonzo:
  * working on the interpreter, improving support for memory operations
  * adding support for tables
* jamey:
  * started at fastly a little over a month ago on the wasm team
  * working on understanding the state of fuzzing in wasmtime/cranelift
  * evaluating where fuzzing gaps exist
  * are you interested in fixing issue #3347, afonzo?
    * afonzo: i'd prefer if you could take that; the interpreter is being
      developed to aid fuzzing
* anton:
  * alex crichton reported an issue with constant generation in the aarch64
    backend
  * refactoring/improving the aarch64 backend
* trevor:
  * started at fastly a little over a month ago with the wasm team
  * working on finishing the x64 backend isle migration
* nick:
  * improving performance for stacktrace generation: the slowdown seems linear
    in the number of modules loaded
  * changing codegen to omit frame pointers for leaf functions that don't trap
    * walking over the whole clif function semes expensive, is there a better
      approach here?
* johnnie:
  * pushed the patch for a self-hosted runner to help with automated
    benchmarking
  * current state is that it only runs benchmarks for the x64 backend
  * we can expand to aarch64 later

#### Determining if clif functions trap

* nick: can we use the trap table for a function to avoid walking the whole
  function?
* anton: can we keep some metadata while we're generating the function that we
  set when generating instructions that can trap?
* nick: this semes far away from where the information would be used; what if
  the code that could trap becomes dead after optimization passes?
* anton: can we introduce traps through optimizations?
  * nick: yes, through legalization
* nick: i'll investigate the trap table approach, and fall back on modifying the
  instruction builder to record trap metadata if that doesn't work

#### Performance impact of preserving frame pointers

* anton: leaf functions that trap will save the frame pointer?
  * nick: yes exactly
* jamey: is there a flag for enabling debugging symbols that we could turn off
  for optimized builds?
* nick: there are flags available, but nothing specific that we might use to
  control preserving the frame pointer

#### How would fp preservation interact with authenticated pointers?

* nick: it shouldn't affect authenticated pointers
* anton: the upper ~16 bits (configurable) would be used for the signature
* nick: there shouldn't be any interaction, but we might need to mask those
  bits; we're not doing any unwinding right now, we're just preserving the fp
* anton: the exact number of bits for the signature is configurable and depends
  on page sizes, there are dedicated instructions for stripping the signature
* [ ] nick: i will cc you (afonzo) on the draft pr
* [ ] also cc uweigand
