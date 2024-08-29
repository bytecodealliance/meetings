# August 29 project call

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
* Trevor Elliott
* Chris Fallin
* Bailey Hayes
* Dan Gohman
* Victor Adossi
* Andrew Brown
* Roman Volosatovs

### Notes

* General discussion about the Wasm stack switching proposal
* General discussion about the Wasm exceptions proposal
* fitzgen: Updates on Wasm GC implementation status
  * Host allocation of structs and arrays is implemented
  * `i31ref` is implemented
  * Still need an `EqRef` host type
  * Ready to start implementing all other instructions
  * Writing a blog post about the stack maps overhaul
  * General discussion about how to get GC type information from the validator
    into the Wasm-to-CLIF translator
* fitzgen: Updates on the Pulley interpreter
  * Interpreter itself and the Cranelift backend have landed
  * Working on rebasing, cleaning up, and upstreaming the runtime integration
  * Will make a "quest" issue with many check boxes for various Wasm
    instructions after that
