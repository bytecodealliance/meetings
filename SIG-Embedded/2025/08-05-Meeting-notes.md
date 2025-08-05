# August 05 SIG-Embedded Meeting
## US and Europe Timzone meeting
** 11am US East Coast, 10am US Central, 8am US Pacific, 5pm Central European Time, 11pm China **

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announce that the meeting will be recorded and hit record as agreed upon in inaugural meeting. 
1. Walking through [LTS document](https://docs.google.com/document/d/1FYaR_pBfO4QlHLVqxw597JCqyp6AhZm7vGd9OsD6Ocs/edit?tab=t.0#heading=h.4smfv3avawne)

## Attendees

* Thomas Trenner
* Stephen Berard
* Merlijn Sebrechts
* Michiel Van Kenhove
* Emily Ruppel

## Notes
  1. Answering the question, what exactly is LTS?
  - Spec is still WASI spec
  - LTS is an effort to maintain WASIp1 encompassing
    - Runtimes
    - Toolchains (libc + compilers + debugging)
    - Spec
    - Documentation
    - Conform to wasip1 spec
  - As features evolve, they are updated slowly for different platforms, with
    clearly documented compatibilities
    - Focus on pragmatic extensions to wasip1 to support porting existing code
      to Wasm
    - Examples: signals, sockets, threads
    - Need to define how this all maps to WASI 1.0
  - LTS is a WASI-p1 LTS pressure release valve
    - Allows for more breaking changes to WASI
    - Limited to necessary platforms
  2. Synchronized Toolchains and Runtimes
  - How do we manage changes from upstream (e.g., clang)?
  - Need synchronization between compiler, libs, runtime: currently there is
    no guarantee on what combinations work
  - Artifacts focus on C/C++ and Rust (not Go/TinyGo at the moment)
  3. WALI Summary
  - WALI/WAZI show that you can easily interface with the underlying OS
  - Reduces update effort substantially compared to WASI
  - In general, using Linux/existing HALs as models for different extensions
    is valuable: e.g., Linux already has done a ton of work making a usable
    signals interface, libusb has already defined how you interface between
    kernel and userspace usb driver
2. Future "Effort Bucketing"
  - Define first version of WASI-SDK and WAMR to target
    - Do we stick with latest WASI-SDK and WAMR or wait for LIME1?
    - Note, Lime1 = first WASI "label"
  - Next focus point is wasi-libc, important immediate extensions:
    - Threads
    - Sockets
    - Test suite + build infrastructure
  - How do we develop LTS version of SDK?
    - Concern is that WASI is a w3c standard, that we are operating outside of.

## Action Items
* [ ] Document sign off (to allow for championing within each of our
organizations/companies)
* [ ] Future call idea: are there grants to apply to for funding a few grad
students to work on WASIp1 LTS
