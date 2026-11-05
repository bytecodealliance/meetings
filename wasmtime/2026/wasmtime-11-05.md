# November 05 | Wasmtime Project Bi-Weekly

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
   1. Find volunteer note taker. Thanks!
1. Announcements
   1. _Submit a PR to add your announcement here_
1. Other agenda items
   1. [till] Release process for patch releases
   1. [till] Should we tighten our requirements for tier 1 features?
   1. [jrandolf] Stabilizing the component model `map` and `implements` features
      1. [Enable `map` by default](https://github.com/bytecodealliance/wasmtime/pull/14801)
      1. [Enable `implements` by default](https://github.com/bytecodealliance/wasmtime/pull/14970) without a dedicated fuzzer
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2026-10-22)
   * [Old, Backlog Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aissue%20state%3Aopen%20sort%3Acreated-asc)

## Attendees

* Nick Fitzgerald
* Pat Hickey
* Paul Osborne
* Daniel Hillerstrom
* Alex Crichton
* Till Schneidereit
* Adam Bratchi-Kaye
* Chris Fallin
* John VanEnk
* Joel Dice

## Notes

* Release process for patch releases
  * Till: context: waves of vulnerability reports as new models are released; when we get a report, anyone else with access to those models can also find them ~immediately. some people have argued we should follow the linux model, I want to argue for something else
  * Alex: can confirm that others find the vulns within a day or so
  * Till: proposing two specific changes: (1) invite "trusted embedders" to private reports as we get them so we aren't blocking anyone from applying their own mitigations immediately and which doesn't depend on the Wasmtime project. (2) replace 3-day embargo with per-business-day bundles of vuln/patch releases and ensuring that each vuln fix has been exposed to "trusted embedders" privately for at least 24 hours (don't want to turn a likely zero-day for embedders into a definitely zero-day for embedders).
  * Chris: need to invest in automation to make this as painless as possible. private repo for developing fixes with a branch per release, automation to do the round up PRs from this private repo to main wasmtime repo
  * Alex: some ideas here, unfortunately we don't have our CI capacity for private repos
  * Till: have been building some automation, including building release binaries ahead of time. also looking into reducing CI time in various ways
  * Alex: want to keep it so that individuals can own particular advisories, decoupled from bundle of today's vuln fixes
  * Nick: can we use github teams to notify "trusted embedders"? might have to add them to BA org. maybe easier to just `@` each person
  * Nick: how do we choose who is a "trusted embedder"? how do we gatekeep that?
  * Pat: TSC?
  * Till: probably project maintainers instead of TSC
  * Alex: we should hash out specifics in an RFC
* Should we tighten our requirements for tier 1 features?
  * Till: should we add "needs ~continuous frontier-model LLM audits"? and also some kind of performance requirements (or at least shouldn't be able to impact other tenants in a co-tenant environment by e.g. taking contended locks)?
  * Nick: feel alright modulo specifics on the "don't affect co-tenants" requirement. who is paying for LLM audits?
  * Till: can try using free token credits I have access to
  * Chris: I also have some access via openai's open source program
  * Alex: can also tweak rules if we run out of resources to do this in practice
  * Alex: back to the co-tenancy: only whether it affects if you are not using the feature or also include new uses of new features?
  * Chris: makes more sense to think of it in terms of DoS vector rather than performance
  * (some discussion around "torture" benchmarking/testing for new features; rules for resource consumption in tier 1; etc)
  * Joel: do we need a soft feature freeze to focus on this?
  * Alex: no known issues at the moment
  * Nick: if we make this part of tier 1 requirements, that kind of becomes forcing function for new features
  * (conclusion: investigate/discuss async, then talk again at next wasmtime meeting)

## Last Old, Backlog Issue

*This tells us where to begin the backlog triage next time.*

TODO
