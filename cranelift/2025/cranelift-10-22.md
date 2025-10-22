# October 22 project call

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
- jlbirch
- cfallin

### Notes

- Updates
  - jlbirch: no updates
  - alexcrichton: no updates
  - cfallin: debugging stuff, mostly in Wasmtime; will have a CL PR at some
    point for patchable breakpoints
  - bjorn3: no updates
  - fitzgen: no updates

- discussion about patchable breakpoints
  - cfallin: plan is to have a load-from-page that we can unmap to make every
    breakpoint trap (for single stepping); and we can replace individual loads
    with break instrucitons (for breakpoints). This seems to be the best we can
    do (down to a single instruction, e.g. 4 bytes on aarch64)
  - alexcrichton: possible alternative: entry hook, set all breaks in func
  - cfallin: worry about reentrancy/recursion, etc; a little more fiddly in
    general.
  - fitzgen: no-signals -- make sure we can support this eventually?
  - we can add semantics to Pulley directly ("modify the hardware" of the
    virtual ISA) to get this -- a breakpoint Pulley op, etc -- for the
    no-std/embedded cases where no-signals is most useful
