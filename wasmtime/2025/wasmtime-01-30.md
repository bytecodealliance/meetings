# January 30 | Wasmtime Project Bi-Weekly

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
   1. Note: meeting notes linked in the invite.
   1. Please help add your name to the meeting notes.
   1. Please help take notes.
   1. Thanks!
1. Announcements
   1. Oscar Spencer: Bytecode Alliance core project proposal update
   1. _Submit a PR to add your announcement here_
1. Other agenda items
   1. Mendy Berger: Discuss adding wasi-gfx support in Wasmtime.
   1. Alex Crichton: Extending MSRV/Security bugfix windows - https://github.com/bytecodealliance/wasmtime/issues/10074
   1. Roman Volosatovs: WASI crate organization for p3 work
   1. Alex Crichton: support tiers of memory64 & Pulley
   1. Alex Crichton: archiving/updating wasmtime-libfuzzer-corpus - https://github.com/bytecodealliance/wasmtime-libfuzzer-corpus/issues/7#issuecomment-2620044796
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)

## Attendees

* Nick Fitzgerald
* Alex Crichton
* Oscar Spencer
* Till Schneidereit
* Andrew Brown
* Danny
* Mendy
* Dan Gohman
* Pat Hickey
* Chris Fallin
* Erik Rose
* Saul Cabrera
* Joel Dice
* Victor Adossi

## Notes

* Oscar: on behalf of the board, happy to announce that Wasmtime is officially a
  BA core project!
  * (general excitement)
* Mendy: I'd like to talk about what it would take to land a prototype for
  wasi-gfx in Wasmtime
  * fitzgen: big question is how to handle new dependencies in `cargo vet`
  * till: we should be able to inherit most of the graphics stack from Firefox's
    audits
  * chris: order of magnitude amount of code?
  * mendy: don't know off hand, guess is 10-20k, mostly just forwarding calls to
    `wgpu`
  * alex: can you tell us more about the current CI setup?
  * mendy: no CI right now
  * alex: is it even possible to test all this graphics stuff in github actions?
    do they have what we need?
  * till: we should look at what `wgpu` does
  * erik: for testing `skia`, we had a room with 400 machines with various
    different kinds of hardware
  * chris: is there a software rendering backend we can rely on for smoke tests
    in CI?
  * joel: should this be an RFC?
  * mendy: would also like to keep the code separate from wasmtime so it could
    be reused across other engines
  * pat: makes sense in the abstract, but is there a concrete use case?
  * till: standardization process will want multiple implementations, if
    everyone is using the same implementation, then that is actually harmful to
    the standardization process
  * (agreement to spin out a call with core wasmtime maintainers and mendy to
    figure out exactly what to do here)
* Alex: do we want to reconsider MSRV and security policy?
  * chris: msrv and backporting are semi-orthogonal. msrv is easy, unless deps
    start surpassing our msrv
  * pat: a lot of people want things, not many people are stepping up to do the
    labor
  * till: the specific ways that debian is packaging wasmtime results in
    something that is unusable, it doesn't have WASI, etc. supporting their
    packaging is not our job, that is the distro package maintainers job. we
    shouldn't make their lives harder when possible, but that is where it
    ends. Wasmtime embedders should be our focus, and whether we have the right
    stability guarantees for them.
  * roman: can we stop bumping major version for every release?
  * alex: can be very difficult to determine whether a major version bump is
    necessary or not, also forces necessary breaking changes to be delayed
  * fitzgen: could have every 10th release be an LTS release where we support
    security backports, keeps the msrv it was released with. that might mean we
    could loosen our two-most-recent rule now, since the LTS would support those
    use cases better
  * chris: can we identify a set of core APIs that we have stronger guarantees
    for?
  * pat: not super useful if the crate doesn't auto-update from `cargo` because
    of semver
  * alex: I think we do have some responsibility for trying to cater to open
    source users, can't just wait for some company to come out of nowhere and
    offer to maintain LTS releases, that will never happen
* roman: wasi crate organization for p3
  * roman: proposal: crate per wasi package, eg wasi-io, wasi-random, wasi-fs,
    wasi-sockets, etc...
  * roman: want to be able to virtualize wasi on the host, in no-std contexts
  * joel: you can already cherry-pick APIs by not using the root `add_to_linker`
    but instead finer-grained versions
  * pat: yeah, crate organization vs linker organization is orthogonal, wasi-io
    was split out not because of wasi proposal reasons, but because of what is
    reasonable or not to support on and share with no-std implementations of
    wasi proposals
  * (agreement to spin out a call between wasmtime's wasi implementors to figure
    out exactly what to do here)
* Alex: tiers of memory64 and pulley support
  * moving pulley up to tier 2
  * moving memory64 to tier 1
* Alex: archiving/updating wasmtime-libfuzzer-corpus?
  * (broad support for changing oss-fuzz builds to stop using it and then
    deleting the repo)
