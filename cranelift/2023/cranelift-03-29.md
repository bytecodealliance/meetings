# March 29 project call

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

- afonso360
- cfallin
- fitzgen
- jameysharp
- alexcrichton
- avanhatt
- uweigand

### Notes

- jameysharp: auto-reviewer set up
  - set up yesterday; a few PRs have come in, seems to work?
  - if multiple paths match, all teams will be assigned; unclear how
    round-robin works in that case
  - alexcrichton: how to handle PTO?
    - jameysharp: set in GitHub profile that you're "busy"
  - cfallin: trying to establish a norm: assigning self to PR when I start to
    review?
    - fitzgen: maybe leave a comment saying "taking a look" instead, easier to
      notice?

- alexcrichton: regalloc glue code question: getting order right in "collect
  operands" vs. pretty-print vs. emit: error-prone?
  - cfallin: yes, still an issue! we've previously talked about generating the
    code from one central description and giving a type-safe pattern for
    backends to fill in
  - alex: for now, add an assert, since we pass in vreg?
  - cfallin: yes, we could definitely do that

- status
  - afonso360: cleanups for riscv64, improving codegen, starting to look into SIMD
  - cfallin: lots of code review + discussion
  - alexecrichton
    - mostly finished avx-ification of x64 backend
    - lots of discussion about load-sinking and multiplicity of fcmps
      - Jamey came up with a good approach and filed an issue: #6097
  - jameysharp
    - load-sinking thinking/issue
    - issues describing how to handle more cases in egraphs pass
      - side-effects, no results, ...
    - PR from a few weeks ago from a contributor for branch folding in egraphs:
      need it to be split up
      - main issue is that we can't do a fixpoint loop
    - writing up remove-constant-phis approach
    - team auto-assignment for reviews
  - avanhatt
    - verifying icmp in aarch64 -- flag bits, etc; fixing flaky SMT issues; etc
  - uweigand
    - no work on CL this week; in broader Rust-on-s390x, libffi support
  - fitzgen
    - winding down bounds-check optimizations: filed issue summarizing work and
      what to do next
    - focus on tail-calls (more in Wasmtime at the moment; calling convention
      in CL next)

- fixpoint loops in egraph for branch folding?
  - cfallin: not that we can't, but we've chosen not to, for compile time
  - cfallin: also issue of currently we remove insts during first pass; we
    couldn't do that if we pass multiple times
    - jamey: indeed, we have a new approach proposed though (side-list of
      effecting insts)
  - jamey: other issues?
    - cfallin: traversal order and `value_to_opt_value` issue?
      - jamey: solved if we take the proposed new approach for
        idempotent/side-effecting ops
