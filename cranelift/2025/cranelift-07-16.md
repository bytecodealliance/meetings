# July 16 project call

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

- alexcrichton
- fitzgen
- uweigand
- Amanieu
- jlbirch
- cfallin

### Notes

- Updates
  - alexcrichton: no updates
  - uweigand: PR merged to enable feature detection on z17; next work is to
    make use of new instructions
    - generic boolean instruction (all 256 possibilities)
      - cfallin: could encode any tree of and/or/not/xor, right?
      - uweigand: indeed; associating with N inputs is tricky (NP-complete?) --
        some work on FPGA synthesis to tackle this
    - vector integer divide
  - jlbirch: no updates
  - Amanieu: posted RA3 review PRs
  - fitzgen: landed function inliner (!); now working on using it in Wasmtime
  - cfallin: reviewing RA3, starting with design doc; discussion point on blockparams

- RA3 and blockparams on multi-successor terminators (RA3 disallows them)
  - cfallin: concerns are perf (requiring a pass even in no-opt builds) and
    complexity -- shifting complexity into Cranelift rather than reducing it
    overall
  - Amanieu: adapter today handles the required simplification in 0.1% runtime;
    maybe not that bad
  - main remaining concern would be keeping the complexity out of the core of
    Cranelift, doing this just when we lower to RA3 operands
