# October 12 project call

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

* @abrown
* @afonso360
* @avanhatt
* @bjorn3
* @cfallin
* @elliottt
* @jlbirch
* @uweigand

### Status

* elliottt
  * working to remove booleans from cranelift
* cfallin
  * merged the mid-end
  * making the mid-end more robust
  * saw good improvements from the original fork in May, but they seem to have
    shrunk a bit in the meantime
  * run the spec test suite with egraphs enabled
* bjorn3
  * working on a new progress report for `cg_clif`. also rebasing my `cg_clif`
    rustup component pr
* uweigand
  * will investigate the booleans pr
* abrown
  * merging a pr in sightglass that allows benchmarks to be organized into
    suites
* afonso360
  * testing the egraphs
  * implemented some more operations in the fuzzer
  * https://github.com/bytecodealliance/wasmtime/pull/4997
  * should we restrict `iconst` to only allow the number of bits present in the
    type it represents?
    * https://github.com/bytecodealliance/wasmtime/pull/5046
    * https://github.com/bytecodealliance/wasmtime/issues/5047
    * we should mask the valid bits off during parsing
* avanhatt
  * verifying rotate rules
  * is there a way to find clif instructions that are expected to be lowered?
    * specifically, `iadd_cout` vs `iadd_ifcout`
    * cf: we should remove the unused instructions
    * cf: for `iadd_cout`, let's ignore it for now as it's a weird variant of
      `add`
  * when do frontends use `cls`?
    * there was an interesting bug with `cls` that could make a good use-case
      for verification
* jlbirch
  * no updates
