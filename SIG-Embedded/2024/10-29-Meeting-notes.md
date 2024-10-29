# October 29 SIG-Embedded Meeting
## Europe & Asia Pacific Timzone meeting
** 10am Central European Time, 4pm in China, 3am US East Coast **

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announce that the meeting will be recorded and hit record as agreed upon in inaugural meeting. 
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. _Submit a PR to add your item here_

## Attendees

* Dominik Tacke (Siemens)
* Stephen Berard (Atym)
* Xin Wang (Intel)
* Lian He (Intel)
* Xu Jun (Intel)
* Max Seidler (Siemens)
* Dan Mihai Dumitriu (Sony)
* Thomas Trenner
* Dongsheng Yan (Sony)

## Notes

* Dominik read the notes from the last meeting.
* PR request is still open, Dominik suggested we merge this now.  No objections voiced.
* Thomas asked about the wasi-p3 and desire to "kill" malloc/realloc
  * Stephen mentioned two prior discussions:  
    1.  ability to share a buffer of memory between modules or between module & runtime
    2.  Christof's mention of using an encoding for sharing
  * Dan echoed need/support for such an approach
  * Discussion here is about doing this in the runtime (Component Model) vs. in the SDKs (this discussion).  
  * Note:  this would largely be transparent to someone writing application code; it's the runtime and SDK developers that would be impacted.
  * Thomas agreed that the Component Model and it's design is nice; but at a practical level in the context of embedded/low-footprint devices it is a big challenge.  Without something like this, their team would likely need to remain on wasi-p1
  * Xin expressed the need for having a simpler ABI here between modules.  Personally, he likes something on top of the C ABI.  
  * Stephen reiterated Chris Wood's comments about the challenges with regard to byte alignment if we were to use something like the C ABI.
  * Dominik mentioned that today, this is largely supported today for most languages.  Rust, for example, can be configured to use the C ABI when needed.  This is how interfacing with OS and legacy code works today.
  * Stephen mentioned the issue regarding compatibility between the Component Model and Modules
    * Components could be "unwrapped" and run as modules.  The key challenge would be the lifting/lowering
    * Would this be possible:  WASI Component --> Unwrapped to WASI Module + Component Model Lift/Lowering adapter --> load on runtime (e.g., WAMR)
    * Thomas thinks this approach could work.
    * Max Seidler mentioned that this could be further optimized where a non-Component Model runtime could, selectively, provide native lifting/lowering for common conversions.  Given the WIT, this adapter code could be generated automatically.
 * Liang asked about the conversion types we're talking about  Guest to WASM type or WASM type to Component type
 * We had to close the meeting due to time, will continue in the next call.

## Action Items
* [ ] Dominik to merge PR
* [ ] Stephen to add agenda item to WASI CG meeting regarding wasip2-module work

