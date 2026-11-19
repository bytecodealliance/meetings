# November 19 | Wasmtime Project Bi-Weekly

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
   1. Find volunteer note taker. Thanks!
1. Announcements
   1. _Submit a PR to add your announcement here_
1. Other agenda items
   1. cfallin: clarify/update policy on new contributors and extractive contribution patterns
   1. cfallin: policy on cwasm stability and rebuild-the-world across security patches
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2026-11-05)
   * [Old, Backlog Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aissue%20state%3Aopen%20sort%3Acreated-asc)

## Attendees

- Alex Crichton
- Nick Fitzgerald
- Chris Fallin
- Adam Bratschi-Kaye
- Roman Volosatovs
- Joel Dice
- Erik Rose
- Oscar Spencer
- Pat Hickey
- John VanEnk
- Johnnie

## Notes

### New Contributor / Extractive Contribution / LLM Policy (Chris/Pat)

* Increase in drive-by "slop" contributions driven by agents that human submitters often do not understand
  - Not universally bad but often bad
  - Sometimes they are good
* Current policy: purely extractive AI submissions disallowed
  - Even disallowed, this is a lot of work for maintainers to swift.
  - Automating slop sifting could be nice
  - Requiring interaction with project prior to dropping patches first could help.
* Idea: require discussion prior to dropping code changes -- via zulip or otherwise.  Talk through idea, use case, change ideas, etc.  This is a degree of gatekeeping and has tradeoffs and against historical open source (but that may be dead now).
* We have some automation ready to try out.
* Alex: don't want to do this, but we might need to.  Possibly trial this (but not set in stone).
* Nick: +1 for trying it out.
* Pat: recent stories of interacting with agents -- if we directly prompt to interact with Zulip, that gate may not work indefinitely.
* Nick: dropping untested PRs uses a lot of CI resources.
* Pat: approved-contributors demo.  Text needs to be workshopped further.  PRs not on allowed would be closed immediately (but could be reopened).  Currently on PRs only; there's an argument to be made on issues but abuse there isn't as prevelant (but exists).
* Oscar: only wasmtime, all ba projects? At what level does the list exist? Pat: not designed to be ba-wide right now but not tied to one specific repo -- currently group wasmtime, wit-bindgen, wasm-tools.
* Nick: maybe some prior art from cargo-vet (vouching).
* Chris: ra2 doesn't get as much, but would be nice to have a low friction way to opt into the tool.
* Alex: the bar to get onto the allow-list should be low; mostly looking to filter out purely extractive or bot contributions without thinking human in the loop.
* Pat: preview of current list; add in recognized contributors.
* Eric: could attempt to detect AI generated text?  Pat: maybe, but likely not necessary at this time.  Nick: even if a user on the list starts submitting low-quality PRs, not the end of the world and can be warned and it will correct most likely.
* Pat: Enable this week?  Yes, nice to reduce noise leading into holiday week.  Just for wasmtime to start to limit blast radius.

### Policy on cwasm stability and rebuild-the-world across security patches

* Cwasm has no ABI stability guarantee for releases, except for patch releases today.
* Idea: don't want to require recompiling cwasm when applying security patches to avoid, for example, recompiling the world in a PaaS environment.
* Limits patching in way that is hard to reason about and makes it impossible to apply certain patches.
* Nick: interpretred as "really try to avoid doing this" but we'd still do it if requiered in order to patch something.  
* For PaaS companies, are we relying on this compat right now?  We don't think so.
* Some related context [in zulip discussion on snapshot for WasmGC](https://bytecodealliance.zulipchat.com/#narrow/channel/206238-general/topic/Snapshot.20for.20WasmGC.20in.20wasmtime/near/639518784).
* Consensus: we are OK with the change; double-check docs, etc.

## Last Old, Backlog Issue

*This tells us where to begin the backlog triage next time.*

TODO
