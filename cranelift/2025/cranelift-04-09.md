# April 09 project call

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
- uweigand
- fitzgen
- alexcrichton
- bjorn3
- abrown
- Rahul Chaphalkar
- cfallin

### Notes

- Updates
  - Rahul: no updates
  - alexcrichton: no updates
  - abrown: no updates
  - cfallin
    - callsites no longer have retval insts (10502)
    - `try_call` support (10510)
    - testing normal return path, not testing unwinding yet (no support in
      clif-tests); that's next
    - had to disable fastalloc
      - doesn't support arbitrary number of `any` defs -- panics when it runs
        out of registers
      - we should fix this eventually, but for now, no cycles to dive into its
        internals, and it's blocking exception support otherwise, so disable
      - alexcrichton: worth leaving as an option and not fuzzing, or enabling
        only when not multi-value returns, or...?
        - possible but tricky to expose a "sometimes works" interface and
          describe it clearly -- better to disable and fix properly later
  - uweigand
    - s390x version of 10502 and 10510 (exception support)
    - issue with additional defs with `any` constraints always going into
      spillslots
      - all vector registers are clobbered, so a load from a stack-carried
        return that allocates its def into an `any` always gets a spillslot
      - semantically, this def happens *after* clobbers, and it'd be nice to
        model that
        - cfallin: yep, eventually, big change to regalloc internals though
          (more program points within an inst)
      - cfallin: set some particular fixed-reg def and remove it from clobbers
        (i.e., "artificially" make it a reg allocation if we know the reg is
        otherwise clobbered/free)?
    - bjorn3: some more discussion about simplifying ABI code, removing srets
      - uweigand: in general we should consider a refactor here -- plenty of
        tech debt; will look
  - bjorn3
    - looking into using exception handling support; looks like it will work
      for `cg_clif`; left some comments on details
  - fitzgen
    - introduced a new kind of optimization rule to mid-end:
      `simplify_skeleton` for skeleton (side-effecting) insts
      - allows for `uadd_overflow_trap` cprop, removing never-triggered
        conditional traps
      - right now, can't change the CFG, by e.g. inserting unconditional trap
        (creates unreachable/dead code) -- possible, thoughts in issue, future
        work
