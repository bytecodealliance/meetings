# September 03 SIG-Embedded Meeting
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
- Chris Woods
- Ron Evans
- Luke Wagner
- Philip Bergmann
- Marcin Kolny
- Boris Kreminski
- Stephen Berard


## Notes
* Chris recapped the previous meeting
  * Goal is to define a critical minimal set, the E-SIG should align around this and support it.
  * Something that is not in this minimal set is not unsupported, other platforms can be supported
  * Aligned on minimums for RAM and NV storage.  Settled on 512KB of RAM and 1MB of non-volatile storage
  * ISA
    * Intel shared more on ARC; likely beyond the scope
    * Xtensa, RISC-V (32- & 64-bit), ARM (32- & 64-bit), and x86
    * MIPS likely won’t make the minimal set (Marcin confirmed that is probably OK)
  * RTOS / Operating Systems
    * Concerns were expressed that the combinations of ISAs and OSes is getting large
    * It was discussed that others are already working with operations systems like Linux and Windows; suggestion was that E-SIG should focus on what’s different.
    * That gets us settled on Zephyr, FreeRTOS, and NuttX
* Marcin mentioned that the developer environment is important as well.  In general, this is largely Linux.
* Chris: MPU will probably be even more common in the future as Moore’s Law catches up for embedded
* Marcin asked about game consoles with things like the Nintendo Switch
  * Christof mentioned that WebAssembly components as a plugin interface for [Veloren](https://veloren.net/blog/devblog-228/)
  * WASI GPU was also mentioned.
* Christof has been using working with the Component Model.  See GitHub issue https://github.com/WebAssembly/component-model/issues/386 for more discussion.  Plan is to setup 20-30 minutes for a presentation.  Tentatively scheduled for Oct 15th.
* Reminder WASMCon November 11-12th
* No other topics, meeting ended


## Action Items
- [ ] @Chris Woods: edit Target platforms page
- [ ] Add clarification to MPU discussion on platforms page
- [ ] Add clarification to MMU discussion on platforms page
- [ ] Start platforms discussion on GitHub (share on Zulip) so we can close this chapter
- [X] Schedule a time for Christof to present (20-30 minutes)
- [X] Ask Marcin about MIPS-32 and Windows

