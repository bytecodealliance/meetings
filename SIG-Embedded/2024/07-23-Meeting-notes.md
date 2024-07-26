# July 23 SIG-Embedded Meeting
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

* Stephen Berard (Atym)
* Marshall Meier (Emerson)
* Chris Woods (Siemens)
* Emily Ruppel (Bosch
* Marcin Kolny (Amazon)
* Maximilian Seidler (Siemens)
* Dominik Tacke (Siemens)
* Thomas Trenner (Siemens)
* Dan Mihai Dumitriu (Midokura)  
* Ayako Akasaka (Midokura)
* Luke Wagner (Fastly)
* Tobias Ahlgrim (Siemens)
* Christof Petig (Aptiv)
* Ralph Squillace  (Microsoft)


## Notes

* Summary of last meeting: Primarily a discussion of which platforms to target


- Walk through of [doc outlining target
  requirements](https://github.com/bytecodealliance/sig-embedded/blob/main/Target_Platforms/Embedded-SIG-Target-Platforms.md)
  prepared by Chris Woods, it outlines hardware considerations
  - Comment from Ralph: change section 6 into matrix of support for different
    runtimes so that the doc can evolve with new runtimes
  - ISA Requirements:
    - Comment from Stephen: avoid going down into the ISA extension minutiae.
      Focus on (e.g.) main arm64/32 ISA
    - Chris agrees, points out Wasm core has an ongoing discussion of exposing
      hardware acceleration in a portable way.
    - Question from Christof Petig– does Arm32 mean old arm standard or does it
      really mean Thumb2 (i.e. for M-class cores). Concluded: that arm32 == M class
      cores, amr64 == a class cores
  - MPU/MMU Requirements:
    - Question from Emily– could we make MPU required and MMU optional? MPUs are
      very (very) common Xtensa core does not have MPU(?)
    - Further point from Max: it doesn’t matter if there’s an MPU because
      conceptually the same features can be implemented either in software or
      hardware. Further, MPUs are all different anyway, with differing
      hierarchies, region sizes, etc.
    - Stephen’s conclusion: just advise that you use a board with hardware
      memory protection capabilities. Without such a board, software protection
      can be put in place in exchange for a performance penalty.
  - Lower limit of memory sizes
    - Siemens is working with ~300kB
    - WAMR says they’re run in 26kB (just the interpreter)
    - Note: we’re targeting something with a runtime (i.e. not wasm2c)
    - Max: in building up i2c proposal, smallest board was 512kB
    - Conclusion: 512kB
  - Minimum Storage size
    - Stephen: points out that there’s both on and off chip flash, so are we talking about total flash or on/off chip separately?
    - Dominik, Stephen: 1MB on board flash seems typical for available boards
    - Conclusion: 1MB
  - Operating Systems: are there any on the list that we should drop?
    - Ralph’s question: are we articulating how constraints change as we go from
      smaller to larger hardware?  OS customization does come in as you move
      from little mcu running RTOS to bigger microprocessor running (say) linux
    - Truly portable interfaces get exponentially more difficult as we support a
      wider range of OS’s
    - Ralph’s point: could this all be a matrix that describes the level of
      support for each of the features conditioned by hardware limits
    - Thought: let’s prioritize the OS list?
    - Stephen’s point: make sure that there’s an example for each feature that’s
      working somewhere
    - Should we be focusing on Linux with RT preempt or just Linux in general?
    - Question: Will this SIG dig down into task scheduling? If so, it would
      make sense to target Linux with rt preempt
    - Marcin: should we be supporting Unix based platforms (e.g. IOS/MacOS)?
    - Should we be supporting phones?
    - Argument: If RPis are in scope, phones should be in scope
    - Phones do have different app deployment mechanisms than other platforms
    - OS prioritization Vote: (everyone gets 3 votes)
      - FreeRTOS: 7
      - NuttX: 1
      - Zephyr: 7
      - Linux: 7
      - Android: 2
      - Windows: 1
  - Target Hardware suggestions:
    - Nucleo U575ZI-Q: STM32U575ZI@160MHz (Cortex-M33), 786KB RAM, 2MB Flash
      (https://www.st.com/en/evaluation-tools/nucleo-u575zi-q.html and
      https://www.st.com/en/microcontrollers-microprocessors/stm32u575zi.html)
    - Nucleo F412ZG: STM32F412ZG@100MHz (Cortex-M4), 256KB RAM, 1MB Flash
      (https://www.st.com/en/evaluation-tools/nucleo-f412zg.html and
      https://www.st.com/en/microcontrollers-microprocessors/stm32f412zg.html).
    - NUCLEO-F429ZI or STM32F429I-DISC1: STM32F429ZI@180MHz (Cortex-M4), 256KB
      RAM, <=2MB Flash
      (https://www.st.com/en/evaluation-tools/nucleo-f429zi.html,
      https://www.st.com/en/evaluation-tools/32f429idiscovery.html and
      https://www.st.com/en/microcontrollers-microprocessors/stm32f429zi.html)
    - Nordic nrf5340, STM32 Nucleo-H563ZI, Raspberry Pi 4/5
    - STM Nucleo-F429ZI
    - Xbox Series X
    - ESP32 Devkit  8MB RAM 8MB flash
    - ESP32S3 Devkit 8MB RAM 16MB flash
    - ESP32 family has the kind of MMU to map SPIRAM to virtual memory, but no
      MPU
    - Atom E3940
    - Question: which peripherals should we support on which classes of boards?
      E.g. Cellular connection on low end mcu?
    - Need to ensure that RTOS’s each work on proposed boards. Don’t want to put
      effort into making given rtos work on given board


## Action Items

* [ ] Determine which target hardware meshes easily with which RTOS's (Tobias)
