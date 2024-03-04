# February 14 project call

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
- cfallin
- elliottt
- saulcabrera
- alexcrichton
- avanhatt
- abrown
- uweigand

### Notes

- fitzgen: none

- acrichton:
  - CL fuzzbug with egraphs
  - as a result of that, hole in x86-64 lowering rules for `fcvt_to_{u,s}int`
    exposed; added general case

- avanhatt:
  - spinning up undergrads looking at verification of ISLE predicates in Rust
  - working with PhD student Michael to use SAIL ISA specs

- saulcabrera:
  - finished core Wasm in Winch (!!!)
  - verifying performance now, and enabling all tests that can be; some bugs
    identified

- abrown: no updates

- elliottt:
  - working on followup fixes to egraph issues; avoiding using union-find
    during elab, and using domtree scoping idea
  - reworking stack-check prologue and stack probes in Winch on Windows

- uweigand: none

- cfallin:
  - back from parental leave! resuming from where I left off with
    proof-carrying code
  - working on a generalization of the dynamic/static-range facts to support
    both at once, necessary when different kinds of checks "meet" onto one
    GVN'd value. Then will plan to fuzz using elliottt's patch for this.
  - fitzgen: other thing that fuzzing found was some SIMD ops do not access the
    full vector-size in memory
