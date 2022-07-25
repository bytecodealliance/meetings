# July 25 project call

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
    1. [E-graph-based mid-end optimization](https://github.com/bytecodealliance/rfcs/pull/27)
    1. [Usage of `cargo vet` in CI for dependencies.](https://github.com/bytecodealliance/wasmtime/pull/4444)
    1. [Definitions of tiers of support](https://github.com/bytecodealliance/wasmtime/pull/4479)
    1. _Submit a PR to add your item here_

### Attendees

- abordado
- abrown
- acrichton
- bjorn3
- cfallin
- fitzgen
- jlbirch
- jsharp
- telliott
- uweigand

### Notes

cfallin: this cranelift meeting needs to be shortened to 45 minutes due to conflict with another Fastly meeting

-----

cfallin: e-graph optimization has been a running effort for the last 1.5 months with accompanying RFC. I implemented full E2E rewrites last week; a lot more to do to optimize and benchmark. Will poll for more opinions in the RFC--any concerns, questions?
uweigand: initially concerned about the combinatorial explosion of all optimization opportunities--too optimistic? But "proof will be in the pudding."

cfallin: there may be some tricks we can do because of ISLE (e.g., extractors) to avoid extra work

uweigand, cfallin: ...platform-specific rewrites vs generic rewrites...

akirilov: another example is flexible vector support; we could legalize targets that have fixed vector widths (e.g., x64)

cfallin: some may ask, "isn't this like the old legalization framework?" Yes, but more targeted.

-----

acrichton: `cargo-vet` is a dependency auditing tool proposed in a new PR. We will initially "allow all" dependencies with an exception list but new dependencies will need to be audited. Will import audit list from Mozilla to kickstart this.

jlbirch: are audits required even for version bumps?

acrichton: yes, the audit is required for the diff between the old and new versions.

fitzgen, cfallin: what about our own dependencies?

jsharp: we should record in our own audit file that our dependencies are trusted.

acrichton: our (BA) list of vetted crates will be published to be usable outside (e.g., Mozilla).

cfallin: what about wasi-nn crates, ittapi, etc.?

acrichton: no hard and fast rule, but BA members should be able to vet their own crates.

cfallin: self-vetting, self-audit... does someone else need to do the audit?

acrichton: everyone can submit audits, a smaller set of people can approve the audits... e.g., "Andrew wrote the crate, we trust him" is fine. Also, this is a trial run but cargo-vet is still a very early-phase tool and may not be completely smooth; we should provide feedback to Mozilla.

telliott: what is a reasonable audit? especially if we import Mozilla's audits; are we satisfying their same criteria when we audit our own code?

acrichton: Mozilla has a policy: small group of people who can approve audits, high bar of entry, multiple levels of audits (safe to run, safe to deploy); the audits are not complete correctness checks, but more like is this a trusted author, are the changes reasonable.

bbouvier: most Mozilla comments to justify the audit are like "I am the author of this crate."

cfallin: it's not a "this code is perfect" stamp, the audits must work together with fuzzing, CVE processes.

uweigand: does Rust ecosystem use this?

acrichton: not yet, currently a third-party Cargo project by Mozilla.

-----

acrichton: definitions of tiers of support--I made an initial attempt to write this up. Motivation: threshold for inclusion in to the repository (what to do to make it to tier 3), also what does it mean to be the top level of support (tier 1). E.g., aarch64 target support lacking fuzzing in oss-fuzz. See https://github.com/bytecodealliance/wasmtime/pull/4479. Added what it would take to move some tier 2 items to tier 1. Added some Cranelift items to tier 3 (i8, i16, DWARF debugging). Would like to discuss here, only do RFC if necessary.

telliott: can definitions of tiers change over time?

acrichton: sure.

cfallin: it would be good to have a timeline for moving lower tiered items to higher; e.g., new ISA backends like arm32 items were unmaintained and became a burden to further progress--eventually deleted. Need to have a minimum bar to staying in the repository.

acrichton: e.g., RISCV backend...

afonso: RISCV does not feel ready.

cfallin: best case it merges with Wasm MVP support and tests pass under QEMU.

uweigand: could we have code in the repository that is not even yet tier 3?

fitzgen: for non-tier 3 backends, there is a burden on "someone" to keep that code compiling.

uweigand: in GCC, backends can be in different states, but the one key requirement is that there is a maintainer

akirilov: what is the GCC process for removing backends?

uweigand: the GCC steering committee decides.

cfallin: acrichton's definition does have a maintainer requirement

acrichton: there are timelines in the definition but the intent is not to "lawyer out" code, but instead to set minimum expectations.

cfallin: we could think up an "experimental backend" idea... or just implement featuers in your own fork?

(bjorn3: You can require `RUSTFLAGS="--cfg no_ready_for_production"` for building.)

fitzgen: burden on maintainers for reviews, on CI for build times.

akirilov: or just document in the repo "look at this fork" for this backend.

trevorelliott: this could be bad for code audits.

-----

cfallin: overflow policy... if we have extra topics, spill them over to the next week.

-----

uweigand: s390x has SIMD support merged; equivalent support to aarch64 and x64 except for i128. There are issues with the i128 ABI--passes everything by implicit reference. How to do this in current Cranelift ABI? Possible, but what I really want is a vector register. So, how equal should CLIF's i128 be equivalent to system ABI?

cfallin: disassembled some Rust i128 types as two i64's; this was different than what I expected for Windows. Recommendation: do what rustc does, since this is the only external user of Cranelift.

uweigand: ...

cfallin: matching rustc is a good first step, but passing by ref could be useful as well.

uweigand: will open an issue

