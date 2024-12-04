# November 26 SIG-Embedded Meeting
## US and Europe Timzone meeting
** 11am US East Coast, 10am US Central, 8am US Pacific, 5pm Central European Time, 11pm China **

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announce that the meeting will be recorded and hit record as agreed upon in inaugural meeting. 
1. Announcements
    1. BCA Elections are upon is, and there is an opportunity to elect folks to the TSC. This is your reminder to ensure you are aware and, to found out who the RC are who are able to participate in the vote.
    
    2. Issues concerning core-wasm
    
       1. Shared Memory. - Update on some on-going discussions, including an interesting proposal on "`memoryref`"
    
       2. Stack Switching
    
       3. Threading
          Note that for threading there have been requests from both Conrad and Andy for input to WASI-threads and Wasm-threads on embedded use cases for threading. The current WASI threading model appears to suggest that threading is self contained inside the runtime? - Although this should be discussed with Luke.
    
          One of the most useful features of WAMR's threading is the ability to integrate with the hosts threading model, allowing native (host) code and 3rd party Wasm code to be tightly coupled, the Wasm code able to wait on semaphores from the host system, etc.
    
    3. Issues concerning WASI and binding
    
       This discussion detailed some important qualities for embedded and specifically realtime systems. It identified the following:
    
       * These are likely to operate without exception handling; the exception handling proposals are useful for the setjmp/longjmp renderings that they can provide. Not for exception handling itself.
       * These use statically typed languages, while not excluding dynamically typed languages, these are the exception not the norm
       * Most of the code base is in "C"
    
       It went on to discuss ways in which components could be consumed by a realtime embedded system the final discussion suggested the following:
    
       1. Unwrap the component and convert it to a module
          This then leaves the issue of lifting and lowering the data.
       2. This lifting/lowering would be achieved by host provided functions
    
       **Questions for discussion this week**
    
       * Are we going about this the right way? - There is discussion on how to adapt the component model. Shouldn't we first define what an ideal execution environment would be first, then work out how to achieve this with components
       * There was discussion on the last call about SDK generated code and runtime code. Shouldn't we detail the mechanism of deployment. Perhaps a lot of this work can be done as part of a component repository prior to deploying to an embedded system?
    
       E.g.
    
       * Is the ideal execution environment one which
         * Uses 'c' language and wasm renderings of it? - with c style bindings between modules
         * Shares memory rather than memcpy
         * Represents everything as 'c' structs (no lifting / lowering)
         * Uses wasm standard function invocation between modules
       * To achieve this, we can off load work to the orchestration / deployment mechanisms
         * We would rarely if ever deploy .wasm directly, and instead would use AOT compiled code first, This implies a prep step. Can we extend this prep step to add additional support for components?
         * Is there ever a use case were one embedded realtime device sends code to another embedded realtime device for execution? - Seems pretty rare (though plausible)?
1. Other agenda items
    1. _Submit a PR to add your item here_
## Attendees

* Chris Woods (Siemens)
* Stephen Berard (Atym)
* Christoph Petig (Aptiv)
* Luke Wagner (Fastly)
* Thomas Trenner (Siemens)
* Dan Mihai Dumitriu (Midokura)
* Ron Evans (The Hybrid Group)
* Marcin Kolny (Amazon)
* Emily Ruppel (Bosch)
* Mendy

## Notes

1. WasmCon Downloads
    1. Lots of new attendees, ARM, margo consortium,...
    2. Component model looks great, but need to figure out how it applies to
      embedded devices.
    3. Memory sharing : conversation with Renderlet in progress-- more a
    core Wasm concern than WASI. Goal: give feedback before implementation is
    rendered on Shared memory, async functions, stack switching, threading.
    3. Presentation on wasm2c on Zephyr for binary size reduction and shrinking
    runtime overhead. Footprint = 10s to 100s of bytes.
    4. Caller provided buffers need to be unified with WASI 0.3 buffers
    5. Questions about portability-- how much do we care about portability
    across runtimes? Conclusion is we care somewhat. WAMR continues to be a
    focus, but compatibility to other runtimes for tooling is importability.

2. Quick Threading discussion
    1. Marcin is using WASI-threads in production
    2. Luke pointed out that WASI-threads has terrible performance, so the
    proposed Wasm threads fixes that, trouble is this requires lots of toolchain
    work.
    3. Developers of core wasm threading are looking for use cases/requirements
    from E-SIG

3. Interfaces discussion
    1. Idea: is there a post-processing step to unwrap a component that would
    allow WAMR to call that code.
      - Problem: how do you deal with new types introduced by the component
        model?
      - Could you implement that part in the Wasm code as an adapter? This
        becomes the component developers responsibility as opposed to the
        runtime's responsibility
      - Need tooling to spare the developer this work
    2. Christoph's take: shared everything can be highly optimized, but limited
    sharing with shared buffers, etc, will require WASI 0.3
    3. Fastly already doesn't use a JIT in the fleet, they do an AOT compilation
    for all of the ISAs on the machines they support, i.e., a high degree of
    ahead-of-time optimization does make sense-- don't rely on the JIT for
    everything.
    4. Using WIT without the full component model for computer vision. Toolchain
    could improve, but it's a reasonable approach. Pattern is workable for
    embedded linux. More details [here](https://wasmcv.org/)
    5. Question: is there a way to hint which languages are on either side of a
    component so you can optimize?
      - Shared everything linking groups components together.
      - See details
        [here](https://github.com/WebAssembly/component-model/blob/main/design/mvp/Linking.md)
      - Trouble is linking modules that aren't explicitly intended to share
        memory is likely to go wrong because of slight toolchain incompatibilities
      - Question: how does interop from C and Rust work?
        - Answer: it's done intentionally at compile time _and_ Rust is designed
          to work well with C/C++.
        - Points for WIT: wit-bindgen hides the unsafe code you typically need
          to work across Rust and C and was equally performant
        - Can also use flat data types of some kind that (again) are pre-agreed
          upon

## Action Items

* [ ] (Chris Woods) Translate shared memory email chain to github issue
* [ ] (Chris Woods) Create github issue for "WIT-able" C/C++ interfaces
