# June 25 SIG-Embedded Meeting
## Europe & Asia Pacific Timzone meeting
**10am Central European Time, 4pm in China, 3am US East Coast**

**See the [instructions](../README.md) for details on how to attend**

Meeting notes: [Google Docs - scratch document, re use this.](https://docs.google.com/document/d/1BCN1424GvPEWxK8GlsoxkJkmi4tsxdiZ7vgZk2c5Pt0/edit#heading=h.s481vtxdo641)

## Agenda

1. Opening, welcome and roll call
1. Announce that the meeting will be recorded and hit record as agreed upon in inaugural meeting. 
1. Recap by chair from the last meeting [review meeting notes from last meeting](06-19-Inaugural-meeting.md).
1. Announcements
    1. Discussing common hardware profile for the embedded sig. As agreed in our charter this is one of the first items that the SIG needs to address.
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. _Submit a PR to add your item here_

## Attendees

* Marcin Kolny (Amazon)
* Stephen Berard (Atym)
* Thomas Trenner (Siemens)
* Dominik Tacke (Siemens)
* Christof Petig (Aptiv)
* Ralph Squillace (Microsoft)
* Qi Huang (Xiaomi)
* Maximilian Seidler (Siemens)
* Dan Mihai Dumitriu (Midokura/Sony)
* Xin Wang (Intel)
* Liang He (Intel)
* Dan Mihai Dumitriu (Midokura/Sony)
* Dongsheng Yan (Sony)
* Wenyong Huang (Intel)
* Liang He (Intel)
* uyu198x (??)

* Meeting started: session recording
## Notes
* Review of last meeting (kickoff)
* Thomas asked a question about Zulip chat.  #WIP Embedded will be wound down and Sig Embedded should be used forward.  There are a few meaningful discussions in the old channel that we * should not lose them.  Marcin believes it’s possible to move discussions between channels.  Moving these to the new channel seems to be preferred choice of everyone.
* Xin suggested to have a topic timeline for these meetings.  Some topics will span long periods so having visibility of topics will allow people to see what topics are coming up and when.  
* Dominik noted that the initial SIG Embedded proposal had many of these topics and shared the document with the team.  These due need to be broken down into work packages.  
* Thomas said that he believes we should use the proposal as a guide and not stray from that for now.  
* In chat, Ralph suggested to take some suggestions in chat and it would quickly give us a starting list.  “definitely how to pass from wasipt1 --> wasip2 is a great one. what steps? what * resources? and what timeline?”
* Marcin:  there’s a need to setup workstreams and who’s leading/working on them as a lot of the technical work would happen outside of this call.
    * Dominik suggested that we first work on the technical scope whitepaper (embedded definition).  He suggested that we have a simple, open discussion this meeting as a first start.
    * Xin, should we be discussing the hardware people are already working on or new targets.
    * Ralph said that embedded is a spectrum and suggested that we should challenge the engineers.  Some of these devices will push the envelope and he is in favor of a wide spectrum of device.  * Dominik largely agreed but said that having too wide of a definition could cause distraction.  Ralph mentioned a prior conversation where concrete examples were a way to more quickly get to * a definition.  Being more precise will get us to a solution more quickly.  Stephen suggested that we define priorities or tiers of devices and use that to help us focus.
* Dominik started a whiteboard session:
    * Dominik started with an example definition:  Cortex-M4 @ 150MHz, 250K RAM, no MPU
    * Thomas suggested we include embedded OS / RTOS.  He also suggested we include additional CPUs such as ESP32 and RISC-V
    * Dominik asked everyone to put down their thoughts (5 minute exercise)
    * Stephen mentioned that, while not a hardware concern, many embedded devices run as dedicated devices, often without a UI, continuous operation for years, etc.  So while not HW * considerations they often drive HW selection and usage.  Ralph agreed and said that many of these devices are constrained by the environment in which they operate.  
    * RISC-V.  Christof ask 32-bit or 64-bit?  General consensus was 32-bit.
    * Ralph suggested that the location where this runs is going to be what drives a lot of these challenges (vs. work).  This ground should work on those challenges.  He said that we should not * get so hung up on “what is embedded” and instead focus on where the challenges exists that we need to address.
    * Dominik expressed that memory is the big challenge in his mind.  
    * Other topics discussed:
        * Low-power
        * Predictable execution, deterministic execution
        * Memory fragmentation, dynamic memory, virtual memory (or lack thereof) 
        * Real-time
        * Sandbox
* Stephen reviewed the action items with the group.
* Meeting concluded at 10:58 (central European time)


## Action Items

* [ ] Marcin/Dominik agreed to check with Zulip channel admin for #WIP Embedded to ensure we don’t lose current discussions.
* [ ] Dominik will take the whiteboard and start a document for further development.
* [ ] Chairs to have an internal sync to refine how these meetings run.
* [X] Stephen will post notes.

