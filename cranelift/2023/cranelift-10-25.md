# October 25 project call

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
    1. [Legalizing in ISLE](https://github.com/bytecodealliance/wasmtime/pull/7321) (acrichto)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- fitzgen
- afonso360
- abrown
- cfallin
- elliottt
- alexcrichton
- jameysharp
- saulcabrera
- nagisa
- uweigand
- avanhatt
 - Rahul

### Notes

- alexcrichton: legalization in ISLE (issue 7321). Heads-up, discussion in
  issue. WIP PR that moves all existing legalizations and NaN canonicalization
  moved to ISLE. Also includes two ISA-specific rules.
  - cfallin: (summary of comments in issue)
  - fitzgen: could we use this to remove `iadd_imm`, etc.?
    - alexcrichton: slightly separate; this is more about tuning subset of CLIF
      to fit targeted ISA
  - alexcrichton: another goal is to make 32-bit platforms easier ("narrowing"
    legalizations)
  - uweigand: from backend perspective, whole point is to avoid difficulties.
    guarantee we would want is that we have a particular subset
  - alexcrichton: indeed, opts creating things again is a specific issue we
    need to ensure; actually happened here
    - we could do legalization before and after
    - cfallin: cost function could be used to push away from illegal choices
    - uweigand: separate "legal" test in opts?
    - cfallin: multi-value representation
    - will we always have some legal choice?
    - yes, egraph never deletes, so legalization happens first -> always have some choice
    - fitzgen: thinking about cases where opt rules create new illegal nodes
      - (lots of discussion, run legalization again, ...)
    - cfallin: maybe ok: if legalization runs first, we have some legal
      representation for every "root" (value referenced from side-effecting
      skeleton); if rules create new choices in intermediate nodes, we don't
      select them.
      - jameysharp: requires propagating infinite cost upward; but we can do that

- status
  - alexcrichton: no updates
  - avanhatt: no updates
  - cfallin:
    - PCC: x86 works now! working on dynamic next
    - LICM vs. remat fix (PR 7306); pushes remat late so it doesn't interfere
      with LICM's choices. Mostly works, doesn't support recursive remat but
      that is probably OK
  - afonso360: RISC-V improvements
  - abrown:
    - MPK issue fix
    - getting VTune support working
      - can demo VTune working at another meeting (nice!)
    - working on wasm-threads
  - elliottt: no updates
  - saulcabrera: progress on Winch
  - jameysharp: egraph stuff last week
  - uweigand: no updates
  - Rahul: no updates
  - fitzgen: no updates
