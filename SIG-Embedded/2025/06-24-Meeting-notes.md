# June 24 SIG-Embedded Meeting
## US and Europe Timzone meeting
** 11am US East Coast, 10am US Central, 8am US Pacific, 5pm Central European Time, 11pm China **

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announce that the meeting will be recorded and hit record as agreed upon in inaugural meeting. 
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. _Submit a PR to add your item here_

## Attendees

* Merrill (Schneider)
* Chris Woods (Siemens)
* Dominik Tacke (Siemens)
* Michael Sanchez
* Andrew Brown
* Luke Wagner
* Ashish (Collins Aerospace)
* Thomas Trenner (Siemens)

## Notes
* TODO: I'm taking notes! Review earlier part

* Question: how do we use the component model
  * Is component model going to target verticals from other
  * LTS is looking at hitting posix model
  * Hold off on alignment between WASI-LTS and WASI p2..

* Cross language interoperability
  * google approach: in memory representation
  * academic approach: formal verification
  * could we use these to obviate lifting/lowering issues?
    * Note: these were presented in totally different contexts (e.g., embedded JS runtime, with a GC)

* Goals:
  * Stability
  * Turning on features in WASI clang target has been a problem
  * goal is to control compiler toolchain

* Lifting/lowering
  * realloc is in progress
  * < > go back and check

* Conversation for plumber's summit
  * < > go back and check
  * Could intersect when LTS runs out
  * Need concrete "needs"

* Funding LTS
  * Donate developer time
  * Funding an organization that is good at LTS-type work
    * Some initial discussions with RedHat and WindRiver

## Action Items

* [ ] Shape up LTS (Chris Woods)
