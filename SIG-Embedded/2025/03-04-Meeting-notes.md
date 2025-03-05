# March 04 SIG-Embedded Meeting
## Europe & Asia Pacific Timzone meeting
** 10am Central European Time, 4pm in China, 3am US East Coast **

**See the [instructions](../README.md) for details on how to attend**

## Agenda
1. PR for "Proposal for alternative solution (WASI_wasm)" https://github.com/bytecodealliance/sig-embedded/pull/18
2. GPIO interface


## Attendees
* Thomas Trenner (note taker)
* Stephen Berard
* Merlijn Sebrechts
* Michiel van Kenhove
* Ling He
* Max Seidler
* Jarno Vanruymbeke
* Tianlong Liang

## Notes
* Stephen summarizes the open PR suggesting alternative solutions for WASI.
  - Highlights key aspects of existing gaps like threading, signals plus concerns regarding business continuity & maturity.
  - Summarizes of some aspects of the ongoing discussion (like problems when handcrafting adapters p2-p1).
  - Pls hop on to the PR to follow up the discussions.
  - Agreement exists about the existing gaps.
  - Call to action as there is a need to step up and participate in making it working, e.g. via code contributions
* Jarno shows proposal for the GPIO interface, based on WIT:
  - PR for WASI GPIO: https://github.com/idlab-discover/masters-jarno-vanruymbeke
  - Looked at embedded hal traits and ESP ports and it based on those insights
  - Review is done by Max Seidler and Stephen Berard
  - Stephen: Things/Concepts like callbacks (async as a pre-requisite), discovering the configuration or mapping to some semantic information (LED-0 <-> GPIO-Pin 4) seem to miss, yet. Such gaps also exist for SPI, I2C approaches etc. and might thus need to be solved in general.
  - Merlijn brings up security aspects, e.g. for supporting specific USB devices and that the is kind of hard with the current component model; it seems to be too coarse as it just defines that it is acting with USB, but not describing which kind of device is behind it.
  - Christof Petig outlined that the CM is made to abstract the HW-layer below the underlying Interface, e.g. to simulate the connection to the HW upfront Software deployment
  - Merlijn: The CM worked pretty well with wasmtime and performance seemed to not have a big impact, development time was low - in contrast to WAMR. However, footprint is still a concern. Christof proposed using shared-lib and shared-everything approaches.
  Michiel mentioned performance measurements, Thomas and Stephen emphasized that performance numbers would be great - however, taking comparability and hardware setup/influence (e.g. x86 vs cortex-m) into concern.
* Christof on WASI 0.3:
  - WASI 0.3 async needs Rust or C++ 20+.
  - It seems to use "stack pausing and continuation".
  - It needs the core wasm stack switching proposal.

## Action Items
* [ ] TODO
