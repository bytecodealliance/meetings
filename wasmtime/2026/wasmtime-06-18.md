# June 18 | Wasmtime Project Bi-Weekly

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
   1. Find volunteer note taker. Thanks!
1. Announcements
   1. _Submit a PR to add your announcement here_
1. Other agenda items
   1. [Adjusting release process slightly](https://github.com/bytecodealliance/wasmtime/issues/13622) (@alexcrichton)
   1. Fibers support and testing for long-tail platforms (@tschneidereit)
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2026-06-04)
   * [Old, Backlog Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aissue%20state%3Aopen%20sort%3Acreated-asc)

## Attendees

* Alex Crichten
* Bailey Hayes
* Chris Fallin
* Erik Rose
* Nick Fitzgerald
* Paul Osborne
* Roman Volosatovs
* Till Schneidereit
* Victor

## Notes

### Release process adjustment: Alex

#### Motivation
We would like an easier way than git deps to test wasmtime releases, before they're released, in spin.

#### Proposal

1. `main` always has a version like "1234-dev".
2. When we release, change version to something like "46.0.0rc1".
3. There will be a button to set off automation to apply that tag and publish to crates.io.

#### Discussion
* Do we want to yank RC releases when we publish the next one? Leave them up?
* General agreement is to yank, because it won't break anybody with a Cargo.lock dep on them but will discourage new dependers. This way, we don't have to worry about RCs that have security bugs.
* No released package should have a dep on an RC.
* For low friction, we wouldn't bother making nice release notes for each RC.
* Till: thinks we could do spin patch-releases monthly which include a new version of wasmtime, due to how solid wasmtime's QA is. This way, spin can stay on a security-supported version of wasmtime by releasing every 8 weeks, without having to fall all the way back to an LTS.
    
  Spin will "splat" out their config to avoid this implicitly enabling any new features (whose default enablement had changed) in a patch release.
* Bailey: We should mention this new wrinkle of our release process in the wasmtime book so we can point people to it if they errantly start depending on RCs for production use.
* There was some concern in the room about routine yanking's effect on Sonatype and similar tools' reputation imputation. Consensus in the room moved to figuring out the yanking decision later.

### Fiber support and testing for long-tail platforms: Till

* Till: Wasmtime doesn't support all platforms due to fibers. There's a PR to support some MIPSy arch I can't even remember the name of. Do we want to land these, in general, and assume the maintenance burden?
* Alex, Chris: It's a wad of asm and a one-time review cost. It shouldn't change very often.
* Consensus is to land them.

## Last Old, Backlog Issue

*This tells us where to begin the backlog triage next time.*

TODO
