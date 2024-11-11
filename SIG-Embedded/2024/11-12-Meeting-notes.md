# November 12 SIG-Embedded Meeting
## Asia Pacific and US Timzone meeting
**8am China, 8pm US East Coast *$*, 7pm US Central *$*, 5pm US Pacific *$*, 2am Central European Time**
**NB:** *$* - Due to the international date line, 8am on Tuesday, is late Monday evening in the USA

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call

1. Announce that the meeting will be recorded and hit record as agreed upon in inaugural meeting. 

1. Announcements

    The discussion from last meeting broke down roughly as follows:

    1. Issues concerning core-wasm

       1. Shared Memory

       2. Stack Switching

       3. Threading
          Note that for threading there have been requests from both Conrad and Andy for input to WASI-threads and Wasm-threads on embedded use cases for threading. The current WASI threading model appears to suggest that threading is self contained inside the runtime? - Although this should be discussed with Luke.

          One of the most useful features of WAMR's threading is the ability to integrate with the hosts threading model, allowing native (host) code and 3rd party Wasm code to be tightly coupled, the Wasm code able to wait on semaphores from the host system, etc.

    2. Issues concerning WASI and binding

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

* TODO

## Notes

* TODO

## Action Items

* [ ] TODO
