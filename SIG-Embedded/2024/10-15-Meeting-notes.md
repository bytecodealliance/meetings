# October 15 SIG-Embedded Meeting
## US and Europe Timzone meeting
** 11am US East Coast, 10am US Central, 8am US Pacific, 5pm Central European Time, 11pm China **

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announce that the meeting will be recorded and hit record as agreed upon in inaugural meeting.
1. Goal of this meeting is to finalize the target platforms discussions.
1. Touch on list of broader topics to be discussed in future meetings.

## Attendees

* Chris Woods (Siemens)
* Dominik Tacke (Siemens)
* Christof Petig (Aptiv)
* Stephen Berard (Atym)
* Ralph Squillace (Microsoft)
* Max
* Marcin Kolny (Amazon)

## Notes

* Reminder: There was a [PR with the list of target
platforms](https://github.com/bytecodealliance/sig-embedded/pull/6). Please review and
  comment.
  * Remember: this is the *first* stab at defining what "embedded is"
  * Has anybody used WasmEdge? Do we want to have it in the target list? Answer: not interested in adding it for now
  * Confirmed decision to leave some BSD/Windows platforms off embedded list.
* Future Points of discussion:
  * Component model designs for microcontrollers
  * Implications of canonical ABIs and zero copy
  * Push for wasi-p3 and desire to "kill" malloc/realloc
  * [Zulip link](https://bytecodealliance.zulipchat.com/#narrow/channel/438936-SIG-Embedded/topic/Sept.2017th.202024.20Meeting/near/476908905) to conversation about Christoph's shared memory work:
    * iceoryx-like interface that's at a higher level (pub/sub api) (not low
    level like mmap)
  * Build target discussion: defines a new way to map WIT to symbols
    * Defines a new binary target that is a *normal* module with a standardized Wasi-p2 interface.
  * Threading
    * Andrew Brown presented at the Plumber's Summit
    * Andrew is trying to get wasi-threads running again.
    * Question that will come up: embedded systems have lots of different
    threading mechanisms. E.g. WAMR does threading as a pass through to host's
    threading mechanisms --> lets you use the same synchronization primitives as
    the host in Wasm too.
    * Per component model, the threading models on guest are disconnected from the
    host.
    * This could be a really neat challenge if you're running a cyber-physical
    system with non-functional (e.g. timing) requirements.
  * Stack Switching-- how do you handle signals in user code? Current
   implementation doesn't trap. There's no way to interrupt Wasm and force signal
   handler to run.
  * How do we support components on runtimes that embedded systems use, e.g. WAMR,
  that don't have full support for components.
  * What are embedded specific interfaces that we should work towards?
    * E.g. mmap : who gets access to which string paths? How do you provide access
    * control?
    * Can you develop "native components": native code with component interfaces.
    Could you provide a security model for native code using standardized
    patterns?
    * How do you access a C struct in an architecture agnostic way? (i.e. padding
    doesn't get in the way of field accesses).
    * Thought: start with WIT and go from there.
    * Issue: brownfield development of Wasm. Porting certain chunks of code to
       Wasm that is then triggered by the host. Mapping existing interfaces is
       a problem: padding of C structs can differ between different architectures,
       curbing portability

## Action Items

* [ ] Merge PR with embedded target platforms document updates (Chris)
