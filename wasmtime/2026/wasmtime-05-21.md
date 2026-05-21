# May 21 | Wasmtime Project Bi-Weekly

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
   1. Find volunteer note taker. Thanks!
1. Announcements
   1. _Submit a PR to add your announcement here_
1. Other agenda items
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2026-05-07)
   * [Old, Backlog Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aissue%20state%3Aopen%20sort%3Acreated-asc)

## Attendees

* Chris Fallin
* Joel Dice
* Nick Fitzgerald
* Elizabeth Gilbert
* Sebastien Guillemot
* Pat Hickey
* Ankush
* Yordis Prieto

## Notes

- Pat: security release going out today: 24, 36, 44, 45
  - will do it after this meeting; will need approvers
    - thumbs up from Chris and Joel
  - not expecting it affects a lot of people
- Chris: introducing F5 intern Elizabeth Gilbert
  - Elizabeth: Working on component instrumentation, e.g. observability layer around composition
  - Nick: yay!
  - Victor: maintains wirm (aka orca aka new walrus)
- Nick: plan to switch to copying WasmGC collector in this release cycle and then enable WasmGC support in Wasmtime by default
  - Running a subset of WAST tests under Miri
  - New fuzzer with more GC coverage and makes sure objects remain intact
  - tests for previous issues
  - wasm-smith has support for GC
  - no known issues; fixed recent trace info bug
  - then can turn on exceptions as well
- Sebastien: have hired a Wasmtime contributor
  - Interested in helping with component model, stack switching, pulley, and GC
  - Doesn't need to be related to what Sebastien's company needs
  - Nick: yay!  Name?  Sebastien: Thomas.  Organization: Midnight Foundation, generating proofs of execution of components.
  - Nick: invite to Zulip.  Sebastien: yes, will be more active there.

## Last Old, Backlog Issue

1138
