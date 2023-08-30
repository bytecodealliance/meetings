# August 30 project call

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

* Chris Fallin
* Jamey Sharp
* Nick Fitzgerald
* Alex Crichton
* Andrew Brown
* Ulrich Weigand

### Notes

* Status Updates
  * Chris
    * Michelle's interpreter `loop(switch(..))` transform
      * works for hand-written toy examples
      * will look into making more production-y and investigating further
  * Jamey
    * (not own update) but excited for CLIF iconst range validation
    * some codegen changes showing up in filetests
    * need to audit them
  * Alex
    * potential egraph optimization: bit select over vector with a constant
      condition, should be a shuffle
  * Andrew
    * no updates
  * Ulrich
    * starting to look into tail call ABIs for s390x
    * need to define a frame pointer in the ABI, needs to be unallocatable
    * Allocatable vs not registers are defined in the machine environment, which
      is global, and not per-ABI
  * Nick
    * no updates
