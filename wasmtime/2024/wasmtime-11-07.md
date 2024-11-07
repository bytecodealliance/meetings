# November 07 project call

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

* Nick Fitzgerald
* Alex Crichton
* Chris Fallin
* Joel Dice
* Andrew Brown
* Dan Gohman
* Johnnie Birch
* Yordis Prieto
* Vamshi Reddy

### Notes

* Alex: memory config refactorings
  * trying to delete difference between "static" and "dynamic" memories
  * just knobs for individual numbers, infer everything else from those configs
  * `Config::memory_may_move` controls whether a memory's base pointer can
    change
* Joel: check out the component model async RFC, making progress in the
  prototype
* Discussion about recent windows COM ports and unsanitized file paths CVE
* Andrew: digging into component translation and the Wasm type system, as part
  of adding shared-everything threads support to the component model
  implementation
