# May 07 project call

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

- erikrose
- cfallin
- saulcabrera
- fitzgen
- alexcrichton
- avanhatt
- jlbirch
- abrown

### Notes

Updates
- avanhatt: able to attend these meetings again! A few students working on
  verifying the mid-end, and working on upstreaming latest version of
  verification framework
- alexcrichton: bug report this morning with tons of results at Wasm level --
  heads-up, pinged cfallin
- saulcabrera: no updates; Winch development continuing
- cfallin: no updates
- erikrose: no updates
- jlbirch: working on a patch to add instruction support to the new x64
  assembler
- fitzgen: no updates
- abrown: continuing work on new x64 assembler

- fitzgen: question about verification efforts: thoughts on what it would take
  to turn on in CI?
  - avanhatt: mmcloughlin (lead PhD student) does have CI on in private fork;
    want to work on upstreaming this summer

- alexcrichton: question on Winch aarch64 support: ungated new tests and found
  plenty of issues -- is it too early to run aarch64 winch tests in CI?
  - saulcabrera: probably; running tests locally for now
  - alexcrichton: should we have an allowlist of tests that are known to pass
    and enable at least those on CI?
  - saulcabrera: sure, we could do that
  - alexcrichton: unfortunate aspect of Wasm spec tests is that unless one
    supports all of Wasm, it's hard to run them; Winch will need e.g. GC
    support. Maybe we'll maintain a fork; but for e.g. SIMD, clean errors
    rather than panics help
  - cfallin: what's the roadmap for Winch?
    - saulcabrera: tail calls, relaxed SIMD, and reference types to get to tier
      1 on x64 for sure; rest uncertain timeline, small team, etc
    - alexcrichton: are we assuming tier 1 backends need to support all tier 1
      features? we don't necessarily need that property -- maybe we should
      update wording or requirements. "Winch is tier 1 for these proposals"
      etc.
    - cfallin: debug support will also enable us to justify more work / more
      people working on it eventually -- wouldn't worry too much about "running
      to keep up" and trying to get all features implemented
