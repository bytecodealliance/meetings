# December 05 project call

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
    1. Making Wasmtime a Bytecode Alliance [Core Project](https://github.com/bytecodealliance/governance/blob/main/TSC/core-and-hosted-projects.md#applying-for-promotion-from-hosted-project-to-core-project)
        1. [Draft proposal](https://hackmd.io/@tschneidereit/S1e46RdXQye/edit)
    2. Should we have an issue triage process to further improve response/resolution times?
    3. Should we have a `cargo vet` triage process to ensure our dependencies don't go too stale?
    4. (if time allows) Discuss state of [async support work](https://github.com/bytecodealliance/rfcs/pull/38) and next steps.
    2. _Submit a PR to add your item here_

## Notes

### Attendees

* Joel Dice
* Alex Crichton
* Nick Fitzgerald
* Till Schneidereit
* Andrew Brown
* Victor Adossi
* Mark R
* Chris Fallin
* Saul Cabrera
* Dan Gohman
* Danny
* Roman Volosatovs
* Kate Goldenring
* Yordis Prieto

### Notes

* Till: Promote Joel Dice to a reviewer?
  * (general consensus to promote Joel)
* Wasmtime draft application for becoming a BA core project
  * Background: TSC has defined the standards for what it means to be a BA core
    project. Currently there are only hosted projects, but now hosted projects
    can apply for promotion to a core project. Core projects are the "flagship"
    projects of the BA. Core projects gain a representative on the TSC.
  * Till: I've written a draft application w/ help and feedback from Alex and
    Nick. Would like to go over it here, and unless there are serious
    reservations, submit it to the TSC.
  * (Walking through draft together as a group)
    * Discussion of issue triage process
      * Nick: We started doing some issue triage for the purposes of identifying
        missing docs, how do people like doing general issue triage and response
        at the same time? And doing this at the end of each Wasmtime meeting,
        after scheduled agenda items?
      * (general consensus to try this out)
      * (Nick to formalize this process by adding a recurring item to the agenda)
* Joel: updates on async and component model streams prototype for Wasmtime and
  the associated RFC
  * most of the prototype is complete
  * would like to give a synchronous zoom overview of the RFC and
    implementation with an eye towards people who will be reviewing this stuff
  * ping Joel on zulip for an invite
