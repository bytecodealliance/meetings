# September 09 project call

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
- erikrose
- Rahul Chapulkar
- cpetig
- uweigand
- avanhatt
- Jacob Denbeaux
- jlb6740
- adamrk

### Notes

- Updates
  - erikrose: MMU interruption work
  - cpetig: many things on plate, need more time to get arm32 backend into
    shape; and other contributor vanished
  - uweigand: no updates
  - Rahul: planning to work on APX work that Johnnie had started; REX2 prefix,
    EGPRs
  - Jacob: inspiration in ZJIT from the way we run meetings, cool
  - avanhatt: no updates
  - fitzgen: poking around with a new register allocator; next step is to port
    over RA2 checker and shake out bugs. Also some alias analysis fixes; now a
    proper lattice. Also worklist seeding order changed -- ends up optimizing
    some worst cases much much faster.
  - cfallin: mul-overflow fix with `second_result` vs `is_second_result`;
    MachBuffer optimizations
  - jlb6740: finally merged APX version of `add` instruction.
  - adamrk: no updates

- cfallin: ISLE autoconversions, `second_result`
  - fitzgen: early experience with backend before autoconvert existed seemed
    OK; do we really need them for ergonomics
  - cfallin: yes, `def_inst` could fold into instruction extractors, but things
    have shifted -- e.g. all the Gpr/Reg/Xmm newtypes, Gpr -> GprMem, lots of
    other little type models. Would be very very verbose and awkward without.
  - avanhatt: verification? could/should it have caught this?
    - cfallin: seems like it needs updates to model: current model of
      `second_result` looks like identity function; maybe conflating
      instruction versus (the one) result a bit too much?
  - consensus: keep autoconverts, audit the cases we have, come up with rules
    for when it makes sense / is unambiguous and footguns to watch out for
