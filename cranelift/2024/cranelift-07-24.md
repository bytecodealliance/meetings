# July 24 project call

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

* chris
* nick
* alex
* alexa
* andrew
* ulrich
* trevor
* jamey

### Notes

* nick: no agenda, updates!
* trevor: no updates
* andrew: no updates
* alexa: just upstreaming stuff
* alex: no updates
* chris: no updates
* nick: re-implemented user stack maps to support moving gcs and not be
  quadratic. Fuzzing with afonso's fuzzer, found one more bug, reducing that.
  Soon a PR for pulley interpreter, mostly the infrastructure. Cranelift backend
  is next and wasmtime integration is after that. Heads up it's going to start
  appearing soon.
* alexa: as we're extending the verifier to loads/stores, much easier to model
  maximum one load or store for one side of a rule. Hard to do 128-bit stores
  which generate 2 64-bit stores. ISLE rules with multiple distinct operations?
* nick: i128 loads also two loads? is there x64 instruction for 128-bit load store?
* chris: not in x64. With simd can be done though.
* alex: read-modify-write of adding a constant to memory?
* alexa: that's ok, only problematic with multiple loads/stores with the model.
* jamey: vector-op based thing changes alignment requirements?
* chris: there are unaligned ops and generally have same perf.
* nick: will simd loads + lane extract be as performant as 2 loads/stores?
* chris: probably not, need to evaluate. Not a clear win/loss since it's less
  loads/stores, so maybe a win in a tight loop.
* nick: can also leave out i128 loads/stores for verification. Maybe generalize
  later?
* alexa: probably what we'll do if the performance is much slower
* chris: just now remembering the torn store problem, do we worry about this
  causing partial view on trap? Answer is wasm doesn't use i128 but maybe we
  care for consistency of IR? Could argue for this from the elegance
  perspective.
* jamey: looking to add 128-bit ops and cranelift will hopefully start fusing
  and might end up with 128-bit loads.
* alex: can also store the high bits first then the low ones to preserve wasm
  semantics.
* nick: jamey any updates?
* jamey: 128-bit ops we can do after status
* nick: ulrich updates?
* ulrich: making progress on tail calls but not finished. Tail call ABI
  implemented and tests passing with it on-by-default. Working on actual tail
  calls, making progress. Been using ABI to distinguish vector lane order.
  Default ABI affects interacting with Rust code but also affects when using
  Cranelift as a backend for Rust. Wasmtime needs little-endian but
  general-purpose wants opposite. Swaps on boundaries right now where each ABIs
  have a specific lane order. Currently `WasmtimeSystemV` is little-endian and
  everything else is big. What to do with `Tail` for s390 here? Is "Fast" always
  little-endian?
* nick: I think `Tail` should always be little-endian as it's effectively the
  wasm ABI. Replacing `WasmtimeSystemV` with Tail. The `Fast` ABI shouldn't
  matter since we're not using it.
* ulrich: Default selection of ABIs which sometimes uses `Fast`. Is that a
  fallback where you don't want tail? Historic artifact?
* nick: Only for no tail calls enabled, we can get rid of that. Only there for
  legacy reasons at this point. Can make it always `Tail`
* ulrich: Great that makes that simple. With my version of the tail ABI the
  argument area sits below the stack area. Might need to reintroduce nominal SP
  in backend.
* jamey: Why nominal SP?
* ulrich: Have to decrement the stack pointer at the beginning and if during the
  sequence it needs a spill slot needs the proper offset.
* jamey: Can't leave the stack pointer in a fixed place during call sequence?
* ulrich: not with the tail call abi. Wanted to avoid because moving the stack
  pointer moves the backchain field. Can revisit though but seemed more
  complicated with alternatives. Reintroduces a backend-only thing for nominal
  sp.
* jamey: if re-adding doing it only in one backend seems fine. Not sure I
  understand what you're describing, interested to see patches.
* ulrich: Description of ABI in issue I posted awhile back. Focusing on getting
  it fully implemented and working and then iterate afterwards and possibly
  change.

### i128 ops

* alex: opened an issue with Jamey on the design repo for 128-bit ops. Summary
  is to add 128-bit ops as `[i64 i64 i64 i64] -> [i64 i64]` to wasm. Not
  proposing overflow flags or carry flags. Plan is to discuss here, present to
  CG on 8/13, hopefully reach stage 1, and then start upstreaming PRs for
  implementations and such.
* nick: why no div?
* jamey: should see if bigint lib has division
* alex: should look more closely into this and have a better answer
* ulrich: s390 has the same primitive as x64
* jamey: x64 gives both division and remainder
* jamey: one thing worth noting here - key reason for this 128-bit op approach
  is that without optimization work we saw perf regression using these
  instructions. Optimizations are possible but some are more involved than
  others. For example fusing iconcat/isplit would be useful. Good results
  without optimizing is promising though. Reason is likely that these
  instructions are used in loops when peformance-critical and you practically
  must clobber the flags register to branch in a loop (only exception is `loop`
  instruction which isn't useful). Even if we did all peephole things end up
  being forced to move carry flag into register and back into flags no matter
  what and the result was 128-bit ops produced better output.
* nick: doesn't aarch64 have a way to only set certain flags with conditionals?
* chris: it has a bit on alu ops to set flags or not.
* nick: does loop unrolling help? Would still have to check condition...
* jamey: llvm is actually already unrolling the loop to two bodies. In principle
  unrolling lets you have more ops in a row before flags put into a register.
* chris: would need tests in pre/post loops to clobber too
* alex: one thing we found is that the behavior of the wasm instructions if
  specifying flags required pessimal lowerings in the most general case and
  without optimizing these fully-general lowerings performance was worse than
  baseline today. Don't want to put LLVM in a position of not being sure which
  op to emit.
* chris: adds very cheap but flag manipulations quite expensive. Lots of ALUs to
  do lots of adds.
* nick: really valuable to do this deep dive since this isn't the naively
  expected solution.
* alex: another nice part about 128-bit opts is that it's easy to pattern match
  in cranelift when the upper bits are 0 since the input is always `iconcat`
  right now.
* jamey: want to convert to uextend/sextend in mid-end. Easy to match in
  lowerings too! Curious how many ways there are to sextend. Right-shift 63
  seems most obvious but unsure if there are other patterns.
* nick: we have some interesting lowerings for 128-bit shifts. x64 has some
  interesting instructions. Maybe worth looking at those.
* jamey: talking about 64-bit shifts here where wasm gives 64-bit values and
  upper 64-bits always produced with 64-bit operations, so question is how they
  produced it.
* nick: perhaps a fruitful area for souper? Might be worth to see what happens?
* chris: how implemented in llvm and bignum library?
* alex: 128-bit ops in rust and custom lowerings in LLVM
* jamey: common op seems to be 64-bit limbs representing a bigint and in a
  source language you do 128-bit opts on these limbs.
* alex: various knobs in LLVM to customize lowerings and shapes of this proposal
  right now.
* chris: is benchmark blind-sig using openssl?
* jamey: no
