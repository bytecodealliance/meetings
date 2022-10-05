# October 5 project call

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

* @cfallin
* @avanhatt
* @uweigand
* @jameysharp
* @afonso360
* @bjorn3
* @abrown
* @akirilov-arm
* @elliottt

### Status

* elliottt
  * merged the isle overlap checker, and resolved overlaps in existing backends
  * looking into removing boolean types next
* avanhatt
  * rebased the verification branch on main recently
  * extending annotations to more rules to aid verification
  * lots of progress!
* jameysharp
  * looking for compilation speed improvements, investigating the
    cranelift-frontend crate
  * reviewing cfallin's egraph mid-end work
  * out of town for the next two meetings
* uweigand
  * not much to report
  * regarding removing booleans, will we keep `b1`?
    * te: no, we'll switch to having comparisons return an `i8` whose value is
      in the range `[0,1]`
    * cf: this maps well onto our existing use of those values, as we're usually
      branching on whether or not they're zero; all non-zero values effectively
      become `true`
* afonso360
  * looking into flushing the icache on windows, which spawned a long saga of
    refactorings
  * patching fuzz-gen to also test compiler flags, to find bugs in optimized
    code
* abrown
  * excited about the results from aligning loop headers to 64-byte boundaries
  * worried about accurate measurement for the pr
* akirilov-arm
  * team's priorities have changed, and are winding down work on cranelift and
    wasmtime for now
  * we're still available to answer questions, and aren't disappearing
    completely
  * cf: thank you for all your team's work, it's been really valuable!
    * what's the status of dynamic vectors?
    * ak: depending on the status of the flexible vectors proposal, we could use
      it soon
* bjorn3
  * opened a bug https://github.com/bytecodealliance/wasmtime/issues/5021
    * cf: let's track the bug down before release instead of reverting 4892
* cfallin
  * working on getting the mid-end optimizer merged, jameysharp is reviewing the
    pr
  * the plan is to have the mid-end optimizer disabled to begin with, and to
    fuzz it in the background
  * we have a lot of duplication of logic in the backends for i128. it would be
    great if we could try taking a legalization approach to i128 support in
    terms of i64, and we could do this with rewrites in the mid-end
    * uw: s390x supports 128-bit adds, so it would be great to allow this
      legalization to be architecture-dependent
  * also looking into performance more generally
    * investigating some strange performance results in a regalloc2 pr
    * allowing values to exist in multiple places at once in the register
      allocator would mean that we could reduce moves substantially, but to do
      this we'll need to ensure that we lower to SSA

### Performance Measurement

* cf: we've been having trouble with this as well, it's very hard to get good
  measurements on a laptop; using a desktop is a more reliable way to measure
* js: there have been cases where valgrind with dhat show reduced instructions,
  but sightglass shows increased runtime
* uw: that happens on s390x as well
  * you can replace three simple instructions with one complicated one
  * you can restructure code so that the branch predictor performs worse
* cf: in the register allocator, examining higher-level stats can be helpful,
  like the number of spills or moves
  * relying on a stats framework that we can use in parallel with other
    benchmarking utils is very helpful
* ab: going back to dhat, would it be worthwhile to integrate that into
  sightglass?
* js: that sounds hard, how easy is it to embed valgrind?
  * running valgrind on `wasmtime compile` makes it easy to run outside of
    sightglass
