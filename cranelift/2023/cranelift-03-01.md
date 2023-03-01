
# March 01 project call

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
  1. Note: meeting notes linked in the invite.
  2. Please help add your name to the meeting notes.
  3. Please help take notes.
  4. Thanks!
2. Announcements
  1. *Submit a PR to add your announcement here*
3. Other agenda items
  1. Representing relaxed simd in Cranelift (alexcrichton)
  2. *Submit a PR to add your item here*

## Notes

### Relaxed SIMD

* New SIMD instructions added to WASM
  * The semantics of the new instructions is that they have one of two behaviors
  * This would make it hard to write a differential fuzzer, as we can't
    determine what the behavior will be
* Proposal: add new CLIF instructions that are implemented by single backends,
  and WASM will use those arch specific instructions.
  * All instructions in CLIF will keep precise semantics, and we won't inherit
    the nondeterminism from the WASM proposal
* What do we think is the best way to represent these different instructions in
  the CLIF IR?
  * cf: An alternate suggestion was to parameterize the behavior on a cranelift
    setting, but alex's suggestion to introduce new ops prefixed by the arch
    name is cleaner.
* avh: What is an example of instructions that differ in semantics between
  backends?
  * ac: Lane selection instructions are a good example: they're either bit
    selects, or they only use the top bit to determine the result.
  * avn: From the verification perspective, having different instrutions makes a
    lot of sense
* ab: Let's try to avoid baking in arch specific changes to cranelift/wasmtime
  if possible, so that we can adapt to a changing landscape easily in the
  future.
* ab: We can always fall back on the lowerings that don't perform as well for a
  specific target
  * ac: That's what we will do now, wasmtime will have an option to select which
    lowerings we want to use (do we require determinism or not)
  * cf: There's also nothing to stop us from lowering the x86 prefixed
    instructions on aarch64, which could be one strategy for dealing with live
    migrations.
* ab: My only concern with the x86 prefixed instructions is that it makes x86
  the pariah backend. It would be great if no backends needed anything special.
  * uw: Most compilers allow generating target specific SIMD operations
    directly, we don't have that in cranelift now but we might have the need to
    use platform specific intrinsics for other reeasons. `cg_clif` will already
    need to support producing platform specific ops.
  * cf: It was nice to avoid supporting this while we could, but maybe it's time
    to start supporting platform specific instrutions.
* uw: We'll need to ensure that fuzzing doesn't generate instructions that are
  known to not be supported by a specific target
  * a360: It would be really nice to not introduce too many new instructions
    that we need to avoid generating fuzzing

### Constants in cranelift

* ac: `clang` will try to keep constants in registers, but we try to sink them
  everywhere
  * cf: We try to sink/remat constants to decrease register pressure, but it
    would be interesting to see if relaxing this makes a performance difference
* ac: Do we de-duplicate constants?
  * ab: At the function level, yes. We have discussed a module-level constant
    pool as well.
  * cf: Module-level linking would be complicated by some architectures needing
    different instructions depending on how far the data is
* ab: More generally, are we looking into loop hoisting with e-graphs?
  * cf: We do, but remat overrides LICM. We reasoned through that it's cheaper
    on x64 to reload registers with constants, which is why we prioritize remat.
  * ab: Much of the relaxed SIMD work is driven by the meshoptimizer and xnnpack
    benchmarks
  * jb: We have run xnnpack on wasm in the past

### Attendees

* abrown
* afonso360
* alexcrichton
* avanhattum
* cfallin
* elliottt
* jameysharp
* jlbirch
* uweigand

### Status

* afonso360
  * working on the riscv64 backend, making lowerings more efficient
  * working on the fuzzer
    * calls work
    * SIMD is working
* alexcrichton
  * SIMD work on the x64 backend
* avanhatt
  * thinking about integer constants with narrow widths
* uweigand
  * two bugfix prs
  * gave a talk about gdb last week
