# April 01 SIG-Embedded Meeting
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

* Marcin Kolny
* Thomas Trenner
* Shiv Maddanimath
* Ashish Shrikhande
* Luke Wagner
* Dominik Tacke
* Rob Woolley
* David Bryant
* Reid Rankin
* Stephen Berard
* Andrew Brown
* Christof Petig
* Ron Evans

## Notes

### PR on Working Doc
- [Link](https://github.com/bytecodealliance/sig-embedded/pull/18)
- Provoked lots of conversation, but it's not really in a state that you could
  present to a standards committee
- Next steps:
  - Present embedded requirements that came to light in the doc to WASI Working group
  - Define requirements for LTS WAMR and WASI p1

### Wasm I/O Recap
- How to support WASI p1 as it stands right now
  - Idea: present Wasip1 support as LTS, not necessarily creating a new standard
  - Benefit: lets companies actually build on WASI
  - Questions: what does that actually mean? Who's going to participate?
  - Need to look at WASI p1 implementations vs standard to sure up gaps
    - Need additional resources to make this happen
  - Stephen has been mapping "what works" in WASI p1 to estimate needed resources

- LTS idea needs commitment from mid-size/large companies to ensure that it
  actually happens, with a reasonable definition of "long-term"
- Migration paths for LTS --> p2/p3+ should be defined
- Don't want W3C standard for p1 LTS, issue is that WASI p1 LTS may become a de
  facto standard
- Need to bring maturity to full stack: documentation, wasi-libc, compiler
  toolchain, runtime implementations, etc.
- Future e-sig discussion could hash out a memorandum of understanding to define
  companies that are willing to commit to the LTS

### More LTS Discussion
- Amazon views WASI p1 LTS as very important
- Question: Can ByteCode Alliance provide any support for LTS? Short answer:
  majority probably cannot be done through BCA, but still need to sketch the
  scope of the work
- Migration: if LTS exists, why would anyone migrate to p2+?
  - At some point, different companies will have a need to move forward to get
    additional features
  - Will need to define the exact cadence this LTS operates on

### Wasm on Autosar
- Christof presented Wasm to Autosar Adaptive consortium
- Looking beyond WASI p3 as a means of using Wasm on top of Rust/C++/C
  interfaces
- Idea this lead to: can we include Rust in the definition of LTS?

### Languages/Toolchains in LTS
- Do we need language support beyond Rust/C/C++?
- Ron: TinyGo could be a good addition
  - TinyGo does depend on wasi-libc, so it could potentially benefit from the
  LTS effort

### Invitation
- If you have a particular use case you'd like to present, just submit a PR!

## Action Items

* [ ] (Chris) Wrapping up the document
* [ ] (Chris) Posting LTS discussion on zulip
