# August 31 project call

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
    1. `wasmtime run` CLI and argument parsing (bytecodealliance/wasmtime#6737, bytecodealliance/wasmtime#6925)
    2. Procedure responding to security@bytecodealliance.org (acrichto)
    3. More about wmemcheck (acrichto)
    4. Minimum supported Rust version (cfallin)
    1. _Submit a PR to add your item here_


## Notes

### Attendees

* Nick Fitzgerald
* Bailey Hayes
* Chris Fallin
* Jamey Sharp
* Dan Gohman
* Dominique Saulet
* Alex Crichton
* Roman Volosatovs
* Saul Cabrera
* Till Schneidereit
* Zeeshan Lakhani
* Andrew Brown
* Johnnie Birch
* rwong
* Yordis Prieto

### Notes

* Alex: CLI Arguments
  * slated for 13.0
  * problem with old flags: ambiguous whether flags are for wasmtime CLI or the
    Wasm binary
  * changed to be more consistent where once you provide the Wasm binary
    positional argument, every subsequent CLI argument that follows goes to the
    Wasm binary
  * Chris: both of my interns ran into this and had unexpected behavior in their
    scripts, stuff broke silently and/or with confusing error messages
    * Can we detect and warn about when there is differing behavior? Alex had
      previously suggested we could leave both argument parsers and complain
      loudly if they disagree
    * That seems very heavyweight to me, can we instead just scan the `--` flags
      and see if any match a Wasmtime flag and emit a warning?
  * Alex: don't like when I'm using something correctly and it is giving warnings
  * Chris: Would also have a way to disable the warning for when you know it is
    a false positive
  * Andrew: I also ran into issues with the CLI argument parsing change. Also
    there are people using the wasmtime CLI in CI and things like that, how can
    we help them migrate?
  * Chris: maybe we should leave `wasmtime run` forever and define a new
    `wasmtime exec` to replace it?
    * Alex: should design for a year in the future and how we want things to
      ideally look. we won't want two duplicated/confusing commands a year in
      the future.
  * Dan: could pair this change with the other CLI flag changes, and then it is
    less surprising because everything in the CLI has changed, not just one
    subtlety
  * Chris: we could add `exec` and remove `run` to make it fail loudly
  * Dan: I like `run`
  * Chris: I guess it is okay if this is a cost we pay once.
  * Johnnie: what mechanisms do we have to broadcast this change?
  * someone: could have a mini blog post on BA blog?
  * Till: could pair this blog post with announcement of running components via
    CLI and typed main? might require waiting to land the CLI changes a bit
    longer
  * Dan: Let's not tie this together. Worried about message getting mixed up
    where there are two different things that we each want to talk about
    independently muddying each other's water
  * Yordis: do you want people to adopt wasmtime CLI more? what are the goals
    for that?
  * Alex: goal isn't necessarily to increase CLI usage, just polish what we
    have. risk is breaking existing users and losing them because they don't
    want to update
  * Andrew: should we go back and edit blog posts that contain wasmtime CLI
    invocations?
    * general consensus: yes
  * Chris: new CLI arguments also remove old CLI arguments?
    * yes
    * in that case, it will at least fail loudly, not silently, which is good
      and easy to migrate
  * Alex: okay we can aim for wasmtime 14.0 for both CLI changes
* Alex: Responding to security@bytecodealliance.org
  * got an email a couple weeks ago and for various confluence of reasons
    (vacations, etc) we didn't respond quickly
  * Especially good to talk about now as wasmtime core maintainers are starting
    to spread out across multiple companies
  * Till: github now has the ability to do private vulnerability reporting which
    can be enabled per-organization and per-repository. planning on enabling for
    BA org and wasmtime repo, TSC is encouraging enabling for all hosted
    projects. will need to update public docs about reporting to use the project
    reporting if it exists, or else org reporting, or else
    security@bytecodealliance.org as final fallback. This way we keep group
    notified small and most precise/localized as we can, for good security
    hygiene. Also helps avoid bystander phenomena when smaller group is notified
    versus larger group.
  * Alex: sounds good, that addresses my process concerns
* Chris: minimum supported rust version
  * what should our policy be?
  * two common approaches: some old version forever until decide to bump or most
    recent stable (or most N recent stables)
  * some people stuck on old versions from OS package manager or can't use
    rustup for whatever reason (say big co build systems with precise fixing of
    compiler versions)
  * Alex: *if* we have an MSRV, then CI must use that. downsides to both
    approaches: if we upgrade rustc quickly, we can leave behind debian
    users. if we fix to old versions, then we don't get rustc/std library bug
    fixes.
  * Chris: new versions can also introduce bugs, such as when our existing UB
    started leading to miscompiles after rustc upgrade
  * Alex: also holds us back on using new std features that let us drop
    dependencies and stuff like that
  * Chris: less concerned about features, more concerned about bug fixes
  * fitzgen: could just define MSRV and it can change per major release, but not
    necessarily
  * Alex: puts too much pressure on staying on old version, as people will come
    out of the woodwork and say "you broke me" even tho we reserved that
    right. also no policy for what version to upgrade to
  * Andrew: what do other projects in the rust community do?
  * Alex: no real consensus. serde is very conservative because it is used by
    everything, most everything very loose
  * Dan: debian just upgraded to a year old rustc, will be a long time before
    they upgrade again
  * Alex: cost of supporting a 1-2 year old MSRV is greater than having a few
    extra developers using wasmtime
  * Andrew: whenever MSRV has needed to go up in practice it has been an easy
    upgrade for me
  * Chris: What is the philosophical goal here? run everywhere, including old
    systems? or be modern and fast?
  * Dan: wasm's future is bigger than its past or present, build for the
    future. debian stable too far out. prefer stable - N for some reasonable N.
  * Chris: I think a window of supported versions is important (N) rather than
    only stable
  * Till: Debian is big outlier here, and that is something they chose to do,
    not something where we need to bear all the costs for them. also our
    precompiled binaries should just work on debian anyways.
  * Alex: one other philosophical thing: is anyone else doing this? do we want
    to trade feature development resources for this? do we want to differentiate
    in this way?
  * Alex: any objections to moving forward with stable - N?
  * fitzgen: how will we do CI?
  * Chris: can summarize and open an issue for final discussion and determining
    final details
