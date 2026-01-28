# January 28 project call

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

- avanhatt
- fitzgen
- bjorn3
- cfallin
- uweigand
- jlbirch

### Notes

- Updates
  - avanhatt: looking at trying to verify the recent CVE (fcopysign on x86-64);
    auto-conversions and/or sinkable loads
  - uweigand: reviewing z17 instruction patch, no issues
  - cfallin: CVE with fcopysign; thoughts about how to alleviate this footgun
    in the future
  - bjorn3: `cg_clif` updated to latest Cranelift; noticed some Wasmtime
    dependencies added by accident, then landed PR to fix
  - jlbirch: no updates
  - fitzgen: no updates

- CVE and load widening
  - cfallin: we need to "break the chain" somewhere: auto-conversions that do
    sinking, or "type punning" that widens f64 in XMM to full XMM, or sinking
    logic (needs to know where sunk load is used), or sinking in general
  - avanhatt: feels uncomfortable that rule looks like it doesn't have anything
    to do with memory
  - fitzgen: sinkable loads feel uncomfortable in general -- side effect
  - jlbirch: perf impact?
    - cfallin: could evaluate by turning off autoconverts for sinking
  - cfallin: full types? "narrow f64 in XMM reg", no autoconverts for those
  - fitzgen: big lift; maybe the dynamic type checking idea is best
    (debug-assert)
  - discussion about how this interacts with verification
  - cfallin: will write up an issue describing the options here
