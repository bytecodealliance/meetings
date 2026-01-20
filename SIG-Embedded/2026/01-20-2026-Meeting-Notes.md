# Jan 20th 2026 SIG-Embedded Meeting
## US and Europe Timzone meeting
** 11am US East Coast, 10am US Central, 8am US Pacific, 5pm Central European Time, 11pm China **

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announce that the meeting will be recorded and hit record as agreed upon in inaugural meeting. 
1. Announcements
    1. _Submit a PR to add your announcement here_
    2. Administration: Improving E-SIG Communication
        1. Email over zulip (time zone improvements)
        2. Updating ICS files to be invites from google groups, so we can cancel / date / send notification when not needed.
    
        3. Should we send meeting notes via email as well?

    3. Embedded Tasks for 2026 - A wish lists

1. Other agenda items
    1. _Submit a PR to add your item here_

## Attendees

* Emily
* Stephen
* Dominik
* Luke


## Notes
### Meeting Synopsis
The meeting was rather short by Embedded-SIG standards. It focused on administrative updates for the E-SIG and a quick poll for the major items to address in 2026.

#### Administrative Updates
##### Email First Discussion
Chris explained that in the last meeting of 2025 we held a review of what we could improve, and communication was part of that. During the new year break Chris had looked at ways to improve this. He reported that for geographically distributed teams using asynchronous communication was better than synchronous. This allowed folks from different tome zones to collaborate better. Conversations on IM systems like Zulip continued in real time, leaving folks in different time-zones without the ability to participate.

Email based communication resolves this. It gives an expectation and understanding that contribution can be made regardless of the time zone induced delay that has occurs.

The E-SIG has a google group which has an email list. This hasn't really been used, but Chris explained that we could use this more.

This was discussed with others in the group. Emily agreed and explained that she faced similar issues, missing IM conversations, etc. Emily also explained that often there was an expectation of a reply with an email, but not so much with IM conversations. We should make clear the E-SIG emails don't all have to be read and replied to. Emily said it was worth a try.

Stephen shared that the WAMR team has recently setup and started using Google Groups for internally communication, that seems to be working really well. Stephen went on to explain how some of his own company are working, using Google Documents and other document editing and sharing solution to create the initial draft of documents, before moving them into Git repos for management.

Chris explained that he hoped that the email / google group would help conversations continue beyond the face to face time in these meetings.

##### Scheduling Updates
Chris explained that using the email list would be a great way to provide calendar invites for the E-SIG, ones which could be updated to reflect when meetings need to be cancelled - something that is really hard. Chris offered to get in touch with David (the MD of the Bytecode Alliance) to understand if there was an email address that could be used to send meeting invites from. This address should be shared with the co-chairs allowing the E-SIG to be run effectively by whoever is chair.

There were no objections to these approaches, and eventually the following action items were identified:

* That Chris would inform the Zulip community of the Google Group
* That we would use the Google group for regular communication
* That Chris would ask David about an email address which can own the calendar invites for the E-SIG

### Items for 2026
Following the administrative discussion the group turned to discuss the topic items to drive in 2026. The group discussed two things:

1. LTS and Lime 1

2. Shared Memory Proposal

#### LTS and Lime 1
Chris brought up the LTS work, this has paused for some time. In the mean time the Lime1 proposal has set some feature requirements that might address the need, identified via LTS, to set a common feature set between runtime and compiler, and to keep these in sync. Chris explained that he felt this should be investigated.

#### Shared Memory Proposal
Chris brought up the shared memory work of the WAMR team and asked if this should be considered for a W3C Core Proposal. Luke raised his hand and joined in the conversation, explaining that he had had some discussions with the Mozilla team on this and they were interested in a proposal. This might, Luke explained, be a way of addressing mapping Wasm memory to a byte array. The team in Mozilla were interested in getting some performance figures. Luke explained that he understood that this means that there is an extra conditional in a memory access, which could add a performance over head.

Stephen explained some additional detail on how the WAMR implementation works, yes there is an additional conditional check. There is also a concept of chained shared memory, were multiple memory regions are shared. This causes multiple conditional checks, and does indeed introduce additional overhead. This, as Stephen explained, increases as the number of shared regions increases. Luke explained that he had come across this chain work in his research on state of the art.

Stephen went further into what could be done, explaining that these regions could be mapped to memories, and that you'd just flip the memory when you identified the offset it relates to.

Luke explained some further detail on the use case for the Web, where perhaps one memory region would be enough, and that this could be mapped into memory just prior to accessing it, then remaped. or unmaped afterwards. This would work for frame by frame image or video handling in the browser.

There were some questions from Stephen on where the origins of the WAMR implementation came from. Chris explained that he understood that this was from Midokura (Sony), Xiomi and Intel China. Luke said that he had seen the Midokura demo using this to process frames of image data, by chaining together modules.

Chris suggested that there was a need to get in touch with the Mozilla team, and there was also a need to identify the core use cases for the embedded world. It would be great if we could use a standard for the embedded use case, and hope that what is suggested for the Web world would also work for non web use cases.

*Editor note: While this was identified, there were no owner for this*

As this part of the conversation drew to a close, Chris pointed out that this question should also be raised in the next set of E-SIG meetings which would include other time zones.

## Resources Discussed or Shared During the Call

* [The Google Group for the E-SIG is available here - https://groups.google.com/a/bytecodealliance.org/g/sig-embedded/members](https://groups.google.com/a/bytecodealliance.org/g/sig-embedded/members)

## Action Items


* That Chris would inform the Zulip community of the Google Group
* That we would use the Google group for regular communication
* That Chris would ask David about an email address which can own the calendar invites for the E-SIG


