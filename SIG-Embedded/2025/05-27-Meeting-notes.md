# May 27 SIG-Embedded Meeting

## Europe & Asia Pacific Timzone meeting

**10am Central European Time, 4pm in China, 3am US East Coast**

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announce that the meeting will be recorded and hit record as agreed upon in inaugural meeting.
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. _Submit a PR to add your item here_

## Attendees

* TODO

## Notes

### Recap last meeting

Ron Evans voiced concern about the proposal discussed in last meeting supporting [lime1 besides wasi-libc](./05-13-Meeting-notes.md#agenda): There is a lack of wasi-libc maintainers already, would spread thinner with more libs to support

### Discussion LTS

General discussion to move the WASI LTS forward.

* What should be the targeted duration of a LTS
  * Ron 7y
  * TT 10y
  * Dom 8y plus reasonable migratin strategy
  * Ashihs - 8 to 10y w/o migration path, otherwise less
  * Max TT and Doms proposal

* What platforms should be considered?
  * Will the decision be bound to chips, architectures or capabilities of target platforms?
  * What should be the scope: Flexibility / Testability? Having a wide scope of targets will make testing and verifying pretty tedious.
  * In general a tiered model might apply. Having a narrow scope tier one of platforms which will be supported first by the LTS and extended tier 2 and 3 which will get (partial) support over time

* Things we need to put in place:
  * Which runtimes to support? WAMR most probably, maybe wasm2c
    * Reuse the idea of tiers maybe?
  * We need consistent quality, supported by CI/CD
    * How to determine Non functional requirements (Performance, Resource demand, ...)? Proposal: Evaluate and document actual benchmarks first and compare to that and derive NFRs from that about:
      * Compilation time
      * binary size / footprint (from which)
      * execution time performance
      * define a set of parameters

## Action Items

* [ ] Conclude on the above mentioned items and move to [document](https://docs.google.com/document/d/1FYaR_pBfO4QlHLVqxw597JCqyp6AhZm7vGd9OsD6Ocs/edit?tab=t.0)
