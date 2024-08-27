# August 20 SIG-Embedded Meeting
## Asia Pacific and US Timzone meeting
**8am China, 8pm US East Coast *$*, 7pm US Central *$*, 5pm US Pacific *$*, 2am Central European Time**
**NB:** *$* - Due to the international date line, 8am on Tuesday, is late Monday evening in the USA

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announce that the meeting will be recorded and hit record as agreed upon in inaugural meeting. 
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. _Submit a PR to add your item here_

## Attendees

* Emily Ruppel, Bosch Research
* Chris Woods, Siemens
* Luke Wagner, Fastly
* Xin Wang, Intel
* Kailas, Intel
* Wenyongh
* Dongsheng Yan, Sony
* Qi Huang
* Liang He


## Notes
* Recap of last meeting: 
  * Still discussing target platforms
  * Dominik submitted a PR to the e-sig platform definition sheet
  * Substantial discussion around adding Zephyr + ESP-IDF to list of OS
  * More discussion around adding x86
  * Discussion of 1 vs 2 tiers of support

* Goal of list of platforms:
  * Should help WASI community understand needs in embedded systems
  * Issue: we need to balance our highly specific embedded needs with the goal of providing an easily accessible target

* Moving forward for first cut
  * Allow all ISAs
  * Pick initial RTOS/OS
  * Look at platform specs and report most restrictive hardware specs
  * Idea: define a “worst case” platform that all new features work on
  * Some features supporting peripherals will necessarily be optional.
  * We need to think about the implication of specifying the minimums
* Problems from heterogeneous devices need to be separated into what’s solved by
the WASI APIs and what needs to be solved by a higher level orchestrator. (Two
different layers)

* Questions about level of portability:
  * I.e. Should we be looking at APIs that let Wasm modules run on both Siemens &
  Bosch PLCs
* Editing Target platforms Page: 
  * Need to clarify purpose of document (to scope the base level of support for future WASI APIs, not to limit what we’re allowed to work on)
  * Moving ARC to required? ARC allows for extensible/pick and choose instruction set (made by Synopsys)
    * Settled on removing Arc from the required list for now and revisiting in the future: right now there’s no easily available board running ARC
  * Xtensa? Dongsheng from Midokura– Midokura is working on getting WAMR support on Xtensa, they’re targeting a product with it in the future.
    * Espressif is also trying to support WAMR
    * ESP-32 is a very popular chipset
    * Xtensa is shifted to a must
  * Need to follow up with Marcin on MIPS-32. How important is it really?
  * OS discussion
    * Could we drop OS’s that are already covered by WASI (e.g. Linux?, Windows)
    * Major question: is there anything that would be specific to embedded Windows/Linux RT? Safety considerations? Those are covered by other RTOS
    * Question: which operating systems is the e-SIG uniquely positioned to work with? 
    * Considerations: Marcin generally is supporting a very wide range of platforms, consult with him to determine priorities
  * RAM/Storage
    * Minimums: 512kB RAM
    * Minimum 1MB Storage
  * MPU Question: How important would an MPU-enabled optimization be? 
    * MPU isn’t available in the absolute cheapest or oldest hardware, but otherwise it’s very common
    * MPU will probably be even more common in the future as Moore’s Law catches up for embedded
    * TODO: clarify that “MPU optional” means that most ICs will have MPUs and that if non-MPU ICs take a performance hit that’s on them
  * MMU Support– argument is that for large apps (e.g. 1MB) it’s very useful 
    * Clarify that we definitely aren’t prohibiting it
    * Justification: MMU adds uncertainty in timing, can’t have it in hard real time setting

## Action Items
* [ ] @Chris Woods: edit Target platforms page
* [ ] Add clarification to MPU discussion on platforms page
* [ ] Add clarification to MMU discussion on platforms page
* [ ] Start platforms discussion on Zulip so we can close this chapter
* [ ] Ask Marcin about MIPS-32 and Windows
