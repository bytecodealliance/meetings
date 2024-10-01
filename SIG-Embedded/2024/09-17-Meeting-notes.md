# September 17 SIG-Embedded Meeting
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
Dominik Tacke
Stephen Berard
Max Seidler
Christof Petig
Ron Evans
Thomas Trenner
Dan Mihai Dumitriu

## Notes
- Stephen read through the notes from last meeting
- Christof made a suggestion on Zulip around zero-copy IPC to WIT (link:  https://bytecodealliance.zulipchat.com/#narrow/stream/438936-SIG-Embedded/topic/Sept.2017th.202024.20Meeting)
	- Presentation was shared on Zulip [here](https://bytecodealliance.zulipchat.com/user_uploads/15107/bIXAPYU7mRh5pJHSy7pqQE6A/Allocation-pools-Shared-memory-in-WIT.pptx)
	- Christof showed an example of iceoryx2 (zero-copy IPC library used in automotive) with two problems:
		1.  Including a string in the record requires a future WIT extension for embedding data
		2.  The WIT is not legal because the borrow call on the receive is not allowed
	- Christof proposed "flat types"
		- Example showed a list of strings and how they could be encoded inline
		- Supports shared memory, allocation pooling for both arguments and results, and a common serialization mechanism
		- Disadvantage:  new ABI definition
	- Dan said that this was desirable, they already do some encoding like this and this suggestion makes it part of the ABI
	- Synchronization of the shared memory would be handled by the host or component side, with either future<> (WASI 0.3) or pollable (0.2), a separate component would manage (re-)allocation of shared memory and the signaling
	- This will work well with WASI 0.3 and asynch support
	- Next things on this topic:
		- Decide on a encoding mechanism and how addressing should be done
		- Binding examples for Rust and C++
		- If there is enough interest, write-up pull-request (Christof)
	- Ron had a comment about doing this in the guest vs. the host
		- Christof said his bias is to move everything out of the host
		- Thomas agreed that having things in the guest makes it easier to update vs. the host
- Dominik brought up this PR for awareness:  https://github.com/bytecodealliance/sig-embedded/pull/6
	- Please review and we can discuss in the next call (or comment directly in the PR)
			

## Action Items
None for the group
