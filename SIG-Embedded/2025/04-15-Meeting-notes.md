# April 15 SIG-Embedded Meeting
## Europe & Asia Pacific Timzone meeting
** 10am Central European Time, 4pm in China, 3am US East Coast **

**See the [instructions](../README.md) for details on how to attend**

## Agenda
1. Opening, welcome and roll call
1. Announce that the meeting will be recorded and hit record as agreed upon in inaugural meeting. 
1. Announcements
    1. Recap of WASM I/O
    2. Shiv Maddanimath: There is interest in interfaces for CAN, SPIO, GPIO.

## Attendees
* Xin Wang
* Liang He
* Shiv Maddanimath
* Ashish Shrikhande
* Christof Petig
* Thianlong Liang
* Ron Evans
* Stephen Berard
* Thomas Trenner

## Notes

* Stephen's Recap: Wasm I/O recap
  * LTS idea:
    * What can be done to support P1 in the ecosystem for business continuity, as P2+ might not be production ready for a while.
    * LTS means support for a number of years - to get it up an running as long it is needed until P2+ (future WASI 1.0) is production ready.
    * LTS is not meant to become a W3C standard
    * Idea got a broad support from informal discussions at WASM I/O
    * Currently, Stephen is working to figure out, what is needed and what is missing.  He will share some materials shortly.
    * Contributions are necessary, hence commitment of resources is needed - MoUs might be a way to move forward with that
  * Statements about WAMR and LTS:
    * Xin talked with persons from Xiaomi about LTS for P1
    * P1-LTS is very valuable, WAMR's team supports that, also Xiaomi
    * WAMR at Intel strives to support for Zephyr
    * A signing/MoU on company level would be good - it is open, for which time frame this is to be approved, and the alignment is ongoing
    * WASI-LIBC needs more contributors to get it production ready
    * Committing resources is usually bound to internal projects of different organizations. Once the projects end, the contributor moves on. It would help, if a commitment of resources (possibly 1/4 or 1/2 person) for WAMR/WASI-SDK/WASI-LIBC/Clang... could be done. This is to be aligned in different organizations/companies.
* Question and action items about P1-LTS:
  * What is the path from P1-LTS and converge it to WASI RC/1.0 in future?
  * In general documenting structure, mission and doing alignment
  * Call to action for everyone: What is the top 10 needed things to be successful on P1? (e.g., Atym: Bring WASI-Sockets on WAMR?)
* Christof Petig gave a P3 announcement summary:
  * A commercial deadline on finishing P3 in Q1/2025 has passed - but the funding has been extended.
  * Guests using P3 are near-to-stable (thread-handling, streams/future mapping, s. https://github.com/bytecodealliance/wasip3-prototyping)
  * Async host APIs and subtasks (intrinsics) now more and more in state of code clean up
  * P3 not finalized, yet. But gets closer to cross the finish line, possibly in the next couple of weeks.
  * Side note 1: Autosar interest in Wasm & CM is because of standardizing specified data types and cross language possibilities and portability throughout different ECU.
  * Side note 2: Christof mentioned JCO, that "transforms" components to core-wasm to make them execute in browsers. Thomas mentioned, that this approach might be a good way to let P1-LTS converge to P2+. But the differences must be figured out, because JCO's assumption that a JS-runtime and polyfill mechanics in browsers are available just do not hold for embedded devices. Hence, the questions of how to come to contributors for this is open.

## Action Items
* [ ] Stephen to put together a draft list for folks to list their requirements/needs for LTS
