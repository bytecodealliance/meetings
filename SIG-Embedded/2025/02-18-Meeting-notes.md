# February 18 SIG-Embedded Meeting
## US and Europe Timzone meeting
** 11am US East Coast, 10am US Central, 8am US Pacific, 5pm Central European Time, 11pm China **

**See the [instructions](../README.md) for details on how to attend**

## Agenda
1. Opening, welcome and roll call
1. Announce that the meeting will be recorded and hit record as agreed upon in inaugural meeting. 
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. Discussion of the proposal titled "WASI / Wasm System Structure Exploration of improvements or alternative solutions"

## Attendees
* Chris Woods
* Christof Petig
* Stephen Berard
* Ralph Squillace
* Dan Mihai Dumitriu
* Dominik Tacke
* Emily Ruppel
* Luke Wagner
* Marcin Koiny
* Max Seidler
* Michael Sanches
* Thomas Trenner
* Ron Evans

## Notes
- Chris opened the call and briefly discussed the proposal document (link:  https://github.com/bytecodealliance/sig-embedded/blob/main/ExploringOptions/Alternative_Solutions_(WASI_Wasm).md)
- Open issues:  https://github.com/bytecodealliance/sig-embedded/issues
- Chris articulated Siemens' position on Wasm and their needs
    - People are building real products and services on wasi-p1
    - The Component Model is interesting, but a ways off from being delivered and useable as a replacement for P1
    - There is a need for business continuity; how do we provide continued support
    - How do we enable progress on things like 0.2, 0.3, etc.; how do we avoid confusion between the wasi-p1 continuation and the Component Model
- Stephen articulated similar points.  He pointed out that there is a need for a solution that works today for developers.
- Ralph 
    - Was a bit confused about the discussion in the document around the Component Model
    - Chris's point about the intent, he understands and it makes sense to him
    - In the end, what do we need to do now to address these concerns?
- Luke
    - He likes Chris's framing as well
    - Part 1:  How do we produce code that runs on existing P1
    - Part 2:  How do we manage the transition
    - Was confused about the layer; doesn't believe there is any layering here.
- Chris
    - There's a difference between how the Web world works and how the embedded space
    - In the document, we articulated how we could separate (layer) this to allow the two efforts to move forward in parallel.
- Luke:  I think the successful outcome is to not have a fork here.  The engineering effort needs to contribute to the same direction and not fork.
- Marcin
    - Question to Luke:  I would like to get your insights on the proposed layering idea.  If feels to him that this layering concept makes sense.  Why can't we build a low-level API, standardize this, and then build a higher-level API on that using the Component Model.
- Luke:  He likes the idea, but the crucial idea is that is someone makes arbitrary decisions at the low-level API.  
- Marcin: It was a struggle for me understanding all of the CM.  When I see something in Wit, it's hard to understand what that will look like under the hood.  
- Luke:  Yeah, something like a C Header file would make sense.  If someone is building this manually there is a lot of room for error.  By code gen’ing this we avoid those errors.
- Dan:  The one thing that I'd like add that we'd discussed before is having the lowering and lifting code be done at compile time vs. at runtime.
- Chris:  That makes sense and something that was discussed last week in SF at the Wasm CG. JCO and transpiling.  It was a quick conversation so definitely more discussions is required.
- Marcin:  He would like to learn more about this Jco and transpiling idea.  One thing he's like to discuss is if this is actually the right approach/direction.  If feels like this is more work than necessary because we start with the Core Model.  Shouldn't we do this the other way around?
- Luke: In some ways yes, this has been proposed.  That is, generate a Core Wasm module and then at the end, wrap that up as a Component.  This does have some limitations particularly around having full expressibility.  With the JCO approach, this is code gen automatically and what is outputting is Core Wasm (but without the limitations).  Some magic core built-ins could be employed to improve things.
- Stephen:  Lots of good discussion here.  I'd like to articulate a few things I think we all agree on:
    - Using a strucuted approach with some IDL (e.g., WIT) to avoid hand crafting and potential errors
    - Clear documentation on how this should work.  Today the situation is extremely difficult to understand and many are taking the "wait and see approach" vs productively moving forward.
    - Improving tooling
- Thomas:  I'd like to highlight the importance of systems-level programming.  There is a lot of legacy code that people just want to get running.  There are ways to link native C-interfaces and expose them via WAMR.  That works but is not standard.  A lot of these things are just hard today and we need to make systems level programming easy (and possible).  I think the layering concept is a way to potentially tackle this.
- Chris:  I spoke with a number of the Wasm technical team about this.  Part of the issue here is that we're not always having the conversations with the right people to be effective.  So we need to improve on this in the e-SIG.
- Luke:  The good thing about what you've outline around systems-level programming is that these is a limited number of these things. That means we can address these .  There are things such as built-in which are only able to be done in Core Wasm.  We can use these for addressing these hard problems at a lower layer.  Let's articulate these needs.  We can decide what should be in the CM and what should be done as a built in.  [link to CM built-ins](  https://github.com/WebAssembly/component-model/blob/main/design/mvp/Explainer.md#canonical-built-ins)
- Chris:  where is the right place to discuss this
- Luke:  the e-Sig is the right place for these 
- Dominik:  I'd like to point out that there are people trying to build on things today, specifically on P1.  
- Christof:  I just wanted to ask Luke about using the JSO approach around getting the glue code standardized.  This would likely help implementing 0.3 as well.
- Luke:  yes
- Christof:  Is there a standardize naming around Core Module naming. [Link](https://github.com/WebAssembly/component-model/blob/add-build-targets/design/mvp/BuildTargets.md#imports-and-exports)
- Marcin:  I wanted to also touch on the built-ins.  Now we have shared everything threads.  I think there is a need to have a built-in for creating a thread but I've also heard about an Core Wasm instruction for creating a thread.  Does this mean that Core Wasm is taking a dependency on the Component Model.
- Luke:  This is more of a timing thing.  Eventually there will be a Core Wasm instruction but that is a lot of work on the browser side.  So today, this is done via a built-in.  Eventually, this will be an instruction (potentially implemented by the built-in).  The browser has a way of doing this so they are not taking a dependency; it's just outside of the browser that is doing this.
- Ralph:  I think there are things that we can all pursue.  One thing that I think will build frustration, is that unless some of these ideas are tested out it will just not move forward.  I think we need to take some things and try to make progress on.
- Stephen:  I agree with Ralph.  I propose as a next step we clearly articulate the list of things needed and discuss that in our next call.  (general agreement with this suggestion)
- Stephen:  I also think we need to update the document to remove the confusion around the Component Model as this may be a source of confusion for some.
- Ralph:  The document should clearly articulate what it is what we're doing to get there.

Meeting Closed

## Action Items
* [ ] Build the list of key needs
* [ ] Update the proposal document to better articulate framing [PR is here](https://github.com/bytecodealliance/sig-embedded/pull/18)
