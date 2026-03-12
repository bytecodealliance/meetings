# March 12 | Wasmtime Project Bi-Weekly

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
   1. Find volunteer note taker. Thanks!
1. Announcements
   1. _Submit a PR to add your announcement here_
1. Other agenda items
   1. Should we turn off the merge queue?
   1. Can we adjust Tier 1 requirements to allow fuzzing or formal verification?
   1. Guest-controlled resource exhaustion debrief (@alexcrichton)
     * also delay any release happening in an embargo window
   1. Long-term fate of native DWARF (@alexcrichton, https://github.com/bytecodealliance/wasmtime/issues/12677)
   1. `bail_bug!` in Wasmtime, panics vs traps (@alexcrichton)
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2026-02-26)
   * [Old, Backlog Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aissue%20state%3Aopen%20sort%3Acreated-asc)

## Attendees

* Nick Fitzgerald
* Joel Dice
* Alexa VanHattum
* Paul Osborne
* Alex Crichton
* Tolumide Shopein
* Bailey Hayes
* Chris Fallin
* Erik Rose
* Yordis Prieto

## Notes

* Alex: Should we turn off the merge queue?
  * Github actions are unreliable, merge queue increases total number of jobs
    required to merge (have to pass initial checks and then also queue checks
    which are a superset of initial checks)
  * Right now, one failure means you get kicked from the queue and have to retry all queue
    jobs when you re-enqueue
  * Switching away means we can retry individual jobs
  * Will require some CI infrastructure work on our part
  * Will also have to worry about the case when there is no merge conflict
    syntactically, but there is semantically
  * Chris: CI seems to have been more reliable in the last month or so, maybe we
    can delay and see if it gets unstable again and revisit?
  * (Some general agreement)
  * Alex: won't be easy to just flip a switch and go back and forth between
    merge queue and not, if we do switch, it should be considered
    "semi-permanent"
* Nick: Can we adjust Tier 1 requirements to allow fuzzing or formal verification?
  * Right now, require continuous fuzzing
  * Should we allow formal verification to also allow us to reach Tier 1?
  * Chris (and Alexa): concrete scenario: aarch64 is very important (see macos, phones,
    etc). We rely on OSS-fuzz for continuous fuzzing. They don't support aarch64
    fuzzing. Simultaneously, we have formally verified our ISLE lowerings for
    aarch64, which gives strong assurances for that (significant) portion of
    code. Still, not everything that a fuzzer *could* exercise in theory; but
    also the parts that are formally verified give us stronger assurances than
    fuzzing coverage does.
  * [Some discussion of subtleties of theoretical bug that affects aarch64 but
    not e.g. x64 and wouldn't be caught by formal verification]
  * Alex: how do we write this in our docs? what level/amount of verification is enough?
  * Chris: sort of skin-in-the-game thing?
  * Pat: has anyone run fuzzers locally on aarch64?
  * Chris: Afonso did some back in the day; people with macbooks could also do
  it these days; maybe we could do like once a month?
  * Nick: I did it maybe a year ago, there were some silly config paper cuts and
    little bugs like that. maybe we could add a CI job to build fuzzers on
    aarch64? and even a step better would be we run the fuzzer for ~1 minute in
    CI just to smoke test and make sure it hasn't broken
  * Alex: if we do this, then maybe we adjust the requirement from "continuous
    fuzzing" to "regularly scheduled fuzzing" or something along those lines?
    could have a CI cron job
  * Chris: could also do for riscv and s390x under qemu perhaps
  * [Consensus on adjusting requirement to "some kind of regular (but not
    continuous) fuzzing and verification" as an alternative to "continuous
    fuzzing" for Tier 1 requirement]
  * [Alex will write up an issue and experiment with getting CI cron job for
    occasional fuzzing smoke checks going]
* Alex: Guest-controlled resource exhaustion debrief
  * Recent security advisory: https://github.com/bytecodealliance/wasmtime/security/advisories/GHSA-852m-cvvp-9p4w
  * Pat gave more details at recent BA plumbers summit
  * Should we also delay any regular release happening in a security embargo window?
  * [consensus on "yes"]
  * [discussion of whether CM calls can trap when passing too much data]
  * [discussion of fine-grained limits on resource tables and traits for getting
    the heap size of a particular resource]
* Long-term fate of native DWARF debug feature (debugging native wasmtime
  and wasm guests at same time)
  * in the context of e.g. https://github.com/bytecodealliance/wasmtime/issues/12677
  * Chris: we designed guest debugging to avoid problems we have seen with
    native debugging: brittle, never perfect fidelity, values optimized away,
    empirically we have had and continue to have a long tail of weird and very
    subtle bugs. Also it is misleading to users in that it confuses users which
    approach to use, burns maintainence resources, complexity, compile
    time. Would like to remove it once guest debugging is stable and shipping.
  * Chris: to be fair, though, it doesn't allow debugging native wasmtime and
    wasm guest at the same time. but there are not many people doing this (vs
    wanting to just debug guest); less than a dozen.
  * Nick: ship guest debugging first, have a transition period, don't
    immediately remove it. we should be conservative with the transition. should
    create an issue seeking user feedback before committing to
    decision. SingleAccretion has historically invested a bunch in native debug
    and its fidelity, should definitely ping them and see how they feel
  * Alex: before deciding on the transition plan, we should try and funnel users
    into guest debugging and see if it is actually better for their use cases,
    we may naturally end up in a situation where the choice is obvious, but we
    shouldn't assume ahead of time
  * Paul: we would still be able to get stack traces across host<-->guest calls?
  * Chris: yes this is only about value recovery, not about unwind info used by
    e.g. profilers like `perf`
  * [Chris will update docs about native and guest debugging when guest
    debugging lands and ships]
* Alex: bail_bug! in Wasmtime, panics vs traps
  * Certain assertion failures, which do not e.g. corrupt data structures but
    are just about checking that we match spec semantics, could be DoS CVEs if
    they fail
  * Things we can't prove statically
  * Hope fuzzers hit these cases, but can't always rely on that, often in CM
    async machinery
  * Would like to downgrade these things from potential DoS CVEs to terminating
    the wasm program with a trap
  * the host cannot easily recover from a panic, while confidently reusing a
    store/engine/etc...
  * but they can easily recover from a wasm trap, since the whole system expects
    to propagate them
  * [general consensus this is good for this particular shape of Wasm semantics
    related assertions, rather than e.g. data structure sanity checks or
    pointer-is-not-null checks]
  * FYI: `bug.rs` in Wasmtime has a `bail_bug!` macro and a `WasmtimeBug` error
    type that `wasmtime::Error` can be downcast to, you can start using them for
    these assertions

## Last Old, Backlog Issue

*This tells us where to begin the backlog triage next time.*

Resume old issue backlog triage at #1111 next time.

TODO
