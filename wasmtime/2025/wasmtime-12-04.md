# December 04 | Wasmtime Project Bi-Weekly

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
   1. Dead URL links in docs and fixing via PRs (@alexcrichton)
   2. Trusted publishing for Wasmtime (@alexcrichton)
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)

## Attendees

* Alex
* Roman
* Till
* Chris
* Victor
* Nick
* Paul
* Erik
* Andrew
* Pat

## Notes

* Possibly enable AI notetaker for future meetings; not to be published directly.
* Alex: Dead URL links in docs
  - AI bots fixing typos are rejected, this is an extension of this.
  - Chris: Probably falls under "trivial" PR policy
  - Maybe roll them up if we want to keep them
  - A few problems, annoying, chew up CI, etc.
  - Till: some auto changes may have value and we could just land them (after
    reviewed). Maybe we don't need to always reject.
  - Chris: could also follow up on the spirit of the changes (e.g. fix
    deadlinks) and just use it as a reminder to go and do a pass (possibly tool
    assisted).
  - Chris: may do a pass to find and fix deadlinks.
  - Till: Canned Github responses could be useful here to quickly dismiss.
    https://github.com/settings/replies
  
* Alex: Trusted Publishing Crates.io
  - Only trusted workflow can publish if configured.
  - Propsal: remove all from crates.io except GH user (wasmtime-publish) for
    publishing which will then allow us to restrict to the published workflow.
  - There is a backup plan for this by adding back accounts, but it takes a fair
    bit of work to do that.
  - For a lot of things, we may just need to do a version bump and republish.
  - Generally, people agree that this sounds good.
  - Will need to be backported to all supported branches as it is global for the
    crate(s) on crates.io.

* Nick: OOM Handling custom error enum
  - Some enum variants gated on features until all things enabled.
  - ^ May not work very well in practice.
  - Enum is probably always going to need an out like User/Custom to
    reasonably cover cases that are hard to enumerate.
  - Chasing exhaustive coverage might not be achievable and would be a large
    target to try to jump to.
  - Desire to keep error small and keep Result<()> in a single word.
  - There could be workarounds with a Box but there's extra space neeeded for
    the discriminant.
  - Alex: there's a lot of exhisting stuff to deal with.
  - Nick: can't trivially encode OOM enabled vs. disabled.
  - Need to be able to construct an OOM error without allocation.
  - Lots of anyhow type things will always do `Box<dyn Error>`
  - Alex: move lots more to avoid stringy errors in favor of codes.  Restrict
    convenient anyhow style errors to only be allowed without OOM allocations.
  - Till: There's OOM platforms where memory usage is critical (embedded) and
    others that aren't as sensitive to memory size but want OOM failures to 
    be handled explicitly.
  - Nick: so far been doing everything to be anyhow compat to maintain API
    compatibility with what is in tree today.  Touching every fallible function
    would be a very large change.
  - Alex: with features disable, drop the enum variant to model full information
    of custom errors, just have a variant with a From impl that will drop info
    on the non-exhaustive enum.
  - But with normal set of features enabled, things just work without those
    restrictions.
  - Nick: still a big lift to touch everywhere in tree that uses Error
  - Nick: Lots of nasty stuff to get thin pointers following example of anyhow.
    This impl will be very disparate from the impl using codes.
    Unsafe/transmute and other dragons in this impl.
  - Alex: the simple error code version should be trivially correct and test
    burden of that variant should not be difficult to verify (if it builds, it
    should mostly be correct/functional).
  - Nick: alternate plan -- make an enum where all the conversions are OOM-safe
    but looks like an anyhow::Error from the outside still (including macros
    like `bail!`` et al.)
  - Errors that OOM during transform will end up as OOM errors.
  - We can move more errors over time to versions that can't OOM using a more
    unified ErrorCode or similar.
  - Lots of tricky cases to avoid OOM with string formatting, etc. with the
    macros.
  - Nick: developing under miri to aid in avoiding UB.

* Andrew: Announcement - Leaving Intel EOY, probably moving away from active
  involvement in wasm space.
  - wasi-nn will need a new maintainer.
  - Andrew preparing a transition document that will cover some stuff, but we
    will likely need someone to step up for larger pieces like wasi-nn.
  - Possibly move out of tree with what is available now with component model.
  - At some point, marking as deprecated could bring interested parties willing
    to dedicate resources out.
    
* #12076 - Non-deterministic JIT execution failures on ARM64
  - Ideally, we would like to get a this architecture/os
  - Eric: probably only an issue shipping via the official apple app store where
    there are additional restrictions.
  - Maybe we can figure out how to run CI in this hardened mode.
  - Maybe with the `codesign` utility?
  - https://developer.apple.com/documentation/security/hardened-runtime
  - Changes are only in cranelift-jit, not sure on submitters exact use case
    but may be targeting cranelift directly.
