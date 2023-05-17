# May 17 project call

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
- elliottt
- alexcrichton
- fitzgen
- avanhatt
- abrown
- jameysharp

### Notes

- abrown: Johnnie is working on a PR to allow Sightglass to run native
  code too
  - useful baseline comparison
  - https://github.com/bytecodealliance/sightglass/pull/228

  - fitzgen: earlier feedback was: adding a separate build path; has
    been addressed?
    - abrown: still works that way
  - fitzgen: does this impact "add a new benchmark" workflow?
    - yes, it might
  - fitzgen: we should make sure this doesn't impact others if we merge
    it as-is
  - jameysharp: as long as doesn't impact rest of sightglass, no reason
    not to merge it
  - cfallin: does it measure precise phases like the Wasm version
    (bench::start, bench::end)?
    - abrown: yes; compile + instantiate are no-ops, but runtime catches
      the start/end hooks

- Status
  - afonso360: added two SIMD instructions to RISC-V backend
    (extractlane, splat); working on insertlane
  - abrown: reviewing Alex's SSE2 PRs
    - Q: `func_addr` and RIP-relative addressing; should we have it?
      Right now Abs8 used, and not supported in Wasmtime (but Wasmtime
      doesn't use `func_addr`)
    - cfallin: yes, maybe use `colocated` flag to choose this behavior
  - elliottt: merged RA2 changes!
    - also, RA2 bump soon because this also solves a slowdown in
      `merge_bundles`: merging vreg sets for spillsets. New changes from
      overlap work remove that set altogether.
    - next, wanting to remove the redundant move eliminator in RA2
  - alexcrichton:
    - after RA2 bump, will re-land LEA-based add
    - fixed out-of-range `ldr` for constant pool on aarch64; added more
      fuzzing with BB padding
  - avanhatt:
    - defended PhD thesis!
  - jameysharp:
    - merged docs to Wasmtime book about auto-assignment of reviewers
  - cfallin:
    - code-review, no direct CL work this week
  - fitzgen:
    - work on tail-calls continues! New epilogues in calling convention
      work; just tail calls themselves TBD
    - afonso360: how does this interact with CFI?
      - fitzgen: covered in RFC, should be compatible
      - cfallin: shadow-stack (x86-style) can work by lining up original
        call and eventual return from tail-called thing; the tail-call
        itself does not push onto shadow stack. Signed return address
        (aarch64-style) works as long as we preserve the full return
        address.
      - afonso360: cool, wanted to think about how RISC-V CFI would
        work, they're still discussing/designing; likely to be a shadow
        stack
