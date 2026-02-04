# February 04 project call

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

- fitzgen
- bjorn3
- alexcrichton
- avanhatt
- jlbirch
- uweigand
- cfallin

### Notes

- Updates
  - alexcrichton: no updates
  - bjorn3: miscompilation in icmp/select mid-end rules, fixed
    - would be good to backport to v41
  - avanhatt: student looking at libLISA for x86-64 specs for verification of
    x86-64 backend
  - cfallin: fix to miscompile; discussions on newtypes to ensure type-width
    correctness in x86-64 backend (CVE followup)
  - jlbirch: no updates
  - uweigand: reviewed s390x z17 patch; now merged
  - fitzgen: no updates

- alexcrichton: testing `cg_clif` on release branches?
  - question for bjorn3: run tests on release branch when it's cut, to catch
    issues before release (yes, sounds good)
  - filed
    [issue](https://github.com/rust-lang/rustc_codegen_cranelift/issues/1626)
    to track automating this

- fitzgen: x86 spec/libLISA question: how closely does libLISA's machine model
  match what we're doing already?
  - avanhatt:
    - fairly close already
    - making immediates/constants symbolic needs to be done
    - memory model open question
  - example of website: https://explore.liblisa.nl/instruction/4801D3

- fitzgen: why didn't we catch icmp bug in CI with validator?
  - just missing test coverage; IR validator does run in tests (verified with
    this fix)
  - better fuzzing would be nice too; top-down rather than bottom-up expression
    generation
  - cfallin: would be great to harvest rule LHSes to drive fuzzing as well
    - RGFuzz paper
    - fitzgen: indeed, hard part is mapping from CLIF back to Wasm
  - do we run verifier in fuzzing? if not we should turn it on
  - discussion of fuzzer effectiveness, maybe "red-team" test with bug
    re-introduced under a knob to see if fuzzers can find it, etc

- CI (GitHub)
  - many outages lately, including two in the last 1.5 weeks and one during our
    security release
  - discussion of alternatives: probably nothing that would match what we're
    getting for free from GitHub, in terms of available compute parallelism and
    platform support; best of bad options
  - action item: explicitly document that security release timeline is
    best-effort and may be delayed by CI availability/outages; anyone who
    really truly needs zero exposure should be part of embargo process
