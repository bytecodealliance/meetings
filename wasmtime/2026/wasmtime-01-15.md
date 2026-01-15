# January 15 | Wasmtime Project Bi-Weekly

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
   1. Find volunteer note taker. Thanks!
1. Announcements
   1. _Submit a PR to add your announcement here_
1. Other agenda items
   1. cfallin: Wasmtime publishing flow, crates permissions, and need for a runbook to manage process complexity
   1. cfallin: compiling very large functions, and implementation limits
   1. tschneidereit: a gh group for incident managers
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2026-01-01)
   * [Old, Backlog Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aissue%20state%3Aopen%20sort%3Acreated-asc)

## Attendees

* Nick Fitzgerald
* Chris Fallin
* Dan Gohman
* Alex Crichton
* Paul Osborne
* Till Schneidereit
* Johnnie Birch

## Notes

* Chris: Publishing flow
  * We now have additional docs for the publishing process, as well as adding
    new crates to the workspace, thanks Alex
  * Alex: new crates can be tricky because we are using trusted publishing, will
    need someone with BA 1password access, read the docs for more info
* Till: GitHub group for incident managers?
  * "Who can run the vulnerability runbook process?"
    * This is a slightly different set of people from reviewers and maintainers
  * Should we have a github team for this group of people?
    * (consensus: yes)
* Till: FYI EU's cyber-something resilience act
  * Doesn't apply to individuals, but does apply to foundations like the BA
  * May have some implications on / requirements for our development workflow
  * Not 100% clear yet
  * Here's an overview of the CRA implications:
    https://best.openssf.org/CRA-Brief-Guide-for-OSS-Developers
* Chris: Compiling very large functions
  * https://github.com/bytecodealliance/wasmtime/issues/12229
  * Some unoptimized configuration of Kotlin compiler
  * We inline large parts of GC-related instructions
  * This caused us to blow Cranelift's limits (e.g. for number of virtual
    registers defined in a function)
  * But the original Wasm was within the JS implementation limits that we've
    adopted (to have maximal compatibility with Web engines)
  * Web engines can always fall back to baseline compiler. We can't.
  * Should we (a) try to handle everything up to the limit? and (b) if so, how
    do we do that?
  * Till: do web engines blow up near, but within, the impl limits? SM has
    historically been tested with tiering disabled, so if they also cannot
    handle the limit when tiering is disabled, then maybe we should take this to
    the CG and advocate for lowering the limits?
  * Alex: want to strive to compile all wasm within limits for all
    backends. that said, for things like when debugging is enabled, we are
    already changing Wasm-to-CLIF translation and can do special things here,
    e.g. manually spill values to the stack
  * Chris: would prefer adjusting bitpacking to manual spilling
  * Nick: also prefer to support established limits. if they are too onerous,
    then we should take that conversation to the CG rather than just giving up
    on supporting the established limits
  * (discussion about what it would take to make Cranelift resilient to DoS during
    compilation)
* Nick: FYI `wasmtime::Error` is not a re-export of `anyhow::Error` anymore
  * `wasmtime::Error`, and the whole `wasmtime::error` module, is API compatible
    with `anyhow`, however
  * There is a new `"anyhow"` cargo feature for Wasmtime to enable
    `anyhow::Error`-to-`wasmtime::Error` conversion helpers
  * This allows us to start handling memory-exhaustion, out-of-memory, and
    allocation-failure errors in Wasmtime

## Last Old, Backlog Issue

Resume backlog triage at issue 1096
