# January 7th 2025 SIG-Embedded Meeting

## US and Europe Timzone meeting
** 11am US East Coast, 10am US Central, 8am US Pacific, 5pm Central European Time, 11pm China **

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call

2. Announce that the meeting will be recorded and hit record as agreed upon in inaugural meeting. 

3. Announcements
   
   1. **Requests for clarification: Devices without an MCU / MMU** </br>
      It is proposed that we declare that for the embedded-sig we will not address hardware which does not have some form of memory control either in the form of an MMU or an MCU.  
      **Background**  
      Based on the hardware platforms we selected, I couldn’t find devices which had the required amount of memory, but didn’t come with at least some form of memory protection.  
      Request for Hardware Requirements Clarification:  [https://github.com/bytecodealliance/sig-embedded/issues/7](https://github.com/bytecodealliance/sig-embedded/issues/7)
   2. **Proposed Approaches to Shared Memory**  
      There are requests for feedback, requirements and scenarios for using shared memory on embedded systems. Not just from the WASI SDK folks, but also from the Core-WASM folks. An initial set of approaches was outlined here - Memory Sharing Discussion - [https://github.com/WebAssembly/memory-control/issues/19](https://github.com/WebAssembly/memory-control/issues/19). This should be expanded to include some use cases and we should attempt to recommend a possible approach.

4. _Submit a PR to add your announcement here_
   
   4. Other agenda items

5. _Submit a PR to add your item here_

## Attendees

* TODO

## Notes

* Introduction to memory control:

  The [memory control proposal by Google](https://github.com/WebAssembly/memory-control/)
  defines e.g. read only memory. 
  (Note by author virtual mode is likely the one eSIG is looking for because the other sub-proposals are too limited.)

  Luke proposed to use shared memory for zero-copy streams, [probably this link](https://github.com/WebAssembly/component-model/issues/369#issuecomment-2484459838) 

  The core group is currently discussing memory control, Google is preferring 
  zero hardware requirements.

* [Hardware requirement clarification](https://github.com/bytecodealliance/sig-embedded/issues/7):

  Chris found no hardware with 500kB of RAM which doesn't provide at least an MPU. 
  Requiring an MMU might be possible as a project requirement. The existence of an
  RTOS can be assumed.

  The proposed RISC-V chip comes with an MPU like device called PMP, see [this link](https://github.com/bytecodealliance/sig-embedded/issues/7#issuecomment-2574759255)

  On Cortex-M an MPU is optional, but M4/M7 typically provide it, M0 might need special
  workarounds (CPU cost).

  The [Cyber Resilience Act](https://digital-strategy.ec.europa.eu/en/library/cyber-resilience-act) will likely require sandboxing.

  [Option 5](https://github.com/WebAssembly/memory-control/issues/19) will likely be able
  to support chips without MMU by a copy and without an MPU by [masking](https://github.com/gwsystems/aWsm)

  Discontinous memory can avoid copying and masking with an MPU might result 
  in non-conformant sandboxing of the binary (e.g. accesses into the hole might not trap).

* Three phases for shared memory implementation:

  1. Mapping shared memory simultaneously into the address space of several modules
  2. Implementing subpartitioning of the memory (parts readable, parts writable)
  3. Handling mutual access to parts of the memory

* Basically there are two different approaches to shared memory design

  - bottom-up approach: Provide primitives for low level shared memory and work
    from there. Most likely compatible with existing software design. Equivalent
    to Option 1 to 4.

  - top-down approach: Provide semantic access to shared memory based on high-level
    API design. Best for sandboxing and portability. Equivalent to Option 5.
    
  Option 6 might align well with CHERI hardware capabilities.

  Ayako confirmed that they are currently [going for](https://github.com/bytecodealliance/wasm-micro-runtime/issues/3546) Option 4, because this doesn't 
  require an MPU. With an MPU native code would be the more performant choice providing
  similar sandboxing.

  It looks like there is currently no size-fits-all approach within the eSIG.

* Proposal to create a Matrix of options and hardware capabilities.

  - To be clarified: Which axises should span the matrix, Chris will make a proposal
    and wait for feed-back.

* Interaction with other parts of the community

  - Marcin will discuss multithreading solutions with Andrew Brown later this week.

  - The activation of reftypes by default by the WASI SDK created a portability
    roadblock, the ability to turn it off requires a nightly Rust compiler.

  - A bridge between WASIp2 for a WASIp1 environment added to the agenda
    of the next eSIG meeting.

  - [WASI 0.3 is progressing nicely](https://github.com/orgs/bytecodealliance/projects/16), wasm-tools contains the changes, 
    [wit-bindgen](https://github.com/bytecodealliance/wit-bindgen/pull/1082) is in review phase, [wasmtime](https://github.com/bytecodealliance/wasmtime/pull/9582) is worked on.

    There is already an [early prototype of Streams with native compilation](https://github.com/cpetig/wit-bindgen/tree/main/crates/cpp/tests/symmetric_stream).

## Action Items

* [ ] Create Matrix of shared memory requirements
