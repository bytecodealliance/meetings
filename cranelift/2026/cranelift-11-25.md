# November 25 project call

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
    2. fitzgen: Split out `cranelift-{object,module,jit}`?

## Notes

### Attendees

- fitzgen
- adamrk
- erikrose
- bjorn3
- cfallin
- jlb6740

### Notes

- fitzgen: split out `cranelift-{object,module,jit}` crates into some other
  location or locations?
  - only user is `cg_clif`?
  - bjorn3: anyone who isn't a Wasmtime user uses `cranelift-object` at least
  - fitzgen: proposal: move module and object into cg-clif, moving jit into its
    own BA repo
  - bjorn3: looks like there are non-toy users...
  - also: cranelift-jit is used by filetests infra
  - cfallin: maybe an alternative is to keep in-tree and just nominate/bless a
    reviewer? bjorn3, maintainer/reviewer for these crates?
    - bjorn3: sounds good

- Updates
  - adamrk: no updates
  - bjorn3: merged Cranelift update PR for cg-clif; now don't need to patch
    rust stdlib anymore for atomic u128s!
  - erikrose: fixed inliner bug for interrupt-poll
  - jlb6740: working on splitting wasi-nn into its own repo. going to focus on
    getting APX work going again
  - fitzgen: looking into compile-time builtin perf (involves Wasmtime,
    Cranelift, RA2). Made RA2 split cold blocks into their own LR bundles.
  - cfallin: reviewing a contributor's PR to support separate constants
    (rodata)

- mention of ppc64 on Zulip -- new CL backend or just Wasmtime+Pulley? unclear
- arm32 backend, should we try to push it forward? maybe ping existing folks
  working on it at some point
