# Meeting Minutes
## WAMR Project Maintainers and Bytecode Alliance TSC
**Date:** November 18, 2025  
**Time:** 4:00am PT  
**Place:** By online video conference  

### Introduction
The Bytecode Alliance Technical Steering Commmittee (TSC) organized a working meeting with WAMR Project Maintainers on November 17, 2025 to continue their ongoing collaboration related to WAMR Project security processes and operations.  This document summarizes discussions held at that meeting and captures action items related to furthering the shared efforts.

### Attendees
* Liang He, WAMR Maintainer
* Marcin Kolny, WAMR Maintainer
* Tianlong Liang, WAMR Maintainer
* Xin Wang, WAMR Maintainer
* Oscar Spencer, TSC Director
* Bailey Hayes, TSC Member
* Till Schneidereit, TSC Member
* David Bryant, BA Consulting Executive Director

David served as secretary of the meeting in capturing these notes and identifying action items.

### Agenda
Oscar and Bailey introduced the agenda topics for today's meeting, based on the ongoing email discussion and previous working sessions:
* Talk through key technical topics related to project security and recent project security work (e.g. responding to Security Advisories).
* Make sure project maintainers have the resources they need and access to necessary tools
* Affirm a common understanding of the importance and implications of  project security processes overall.
* Identify specific issues to work on going forward, e.g. standards compliance documentation 

### Discussion

#### General Perspectives on Project Security
Oscar asked the WAMR team how they were feeling about project security overall, based on their focused efforts over the last several months. Liang summarized lessons learned from handling two recent Security Advisories, highlighting insights gained in handling public communications around those advisories, benefits of the team's construction of a Security Run Book patterned after the existing Wasmtime one, value of advice and support from the TSC, and recognizing having broader contributions from project participants as new maintainers. He also called attention to the challenges of supporting WAMR on a growing number of target devices and environments.

Oscar, recalling that some aspects of recent Security Advisory work had been unfamiliar to WAMR maintainers, asked how team members felt about the importance of security overall as part of project development and maintenance. Marcin observed that the Bytecode Alliance perspective on and approach to security was different and new to some maintainers, suggesting that it could help to spend more time learning about that and understanding where differences or disagreements might lie.

#### CVE Walkthrough
Oscar ask the WAMR team to recap their perspective on handling CVEs by picking one and talking through the work involved both in responding to the submitted Security Advisory and generating a release to resolve it. Tianlong led the resonses, narrating a walkthrough of the GitHub repository pages involved. Afterward Liang raised for discussion a few questions that the team had encountered regarding whether it was appropriate to include test cases and similar work products in public aspects of the process (e.g., pull requests) and what "timely manner" meant in resolving an advisory.

#### Project Scope and Prioritization
Marcin raised the question of prioritizing work across the project, explaining that his employer only used a subset of WAMR and could not justify him or other company engineers spending time on WAMR feature or security work that did not align with the company's business needs. He asked how the Bytecode Alliance could assist with that, and whether that situation existed on other projects hosted by the Alliance.

Oscar mentioned that similar situations existed in other projects so this was not an unfamiliar circumstance. He offered the Alliance's assistance in  communicating across member organizations to help find contributors who could assist by concentrating on particular areas of need perhaps by aligning different business case motivations across those organizations. Additionally past experience has shown that some projects may benefit from subsetting to distinguish between areas that do have well established development and maintenance support among participants and those which do not, potentially to the point of refactoring the project. However, he drew a distinction between feature work, where companies understandably may wish to concentrate their resources, and security work which is expected to be equally important to all maintainers across the entire project. Marcin did not believe his employer was motivated to support security work in features that did not align with its business needs. Oscar offered to work with the team to identify which parts of the WAMR project aligned with participants' motivations and which parts might not, allowing a reduction in project scope to only those portions with committed maintenance resources. Xin warned that partitioning might not yield the best outcome if it fragmented interest in the project and undermined the ongoing success of the broader WAMR community, to which everyone agreed.

Till explained that while the Alliance could help engage and encourage member organization involvement it was not currently set up as an entity to directly hire and retain software engineering resources to carry out project maintenance. He outlined how undertaking such efforts would require significant changes to Alliance bylaws including to reflect that such investments would favor some members rather than being neutrally supportive of the Alliance's mission, clarifying in doing so that no other project has ever received this kind of support and that the Bytecode Alliance had only done contracting work benefiting the entire ecosystem such as building out the WASI test suite in support of a W3C standard. He observed that mechanisms to do that kind of thing exist separately from the Alliance's current structure, as reflected by other organizations, but were not practical within the Alliance today.

Bailey called attention to a need for additional documentation, e.g.,highlighting features available to guest programes that are non-standard. She offered to identify what she'd like to see there as part of action item follow-up from this meeting.


#### What Constitutes a Security Issue?
Bailey asked the team if it felt comfortable identifying security issues for WAMR. Liang referred to the recently published project document (["About security issues"](https://github.com/bytecodealliance/wasm-micro-runtime/blob/main/doc/security_need_to_know.md)) that gathers the team's thinking about that, including a section that helps explain that thinking for anyone handling an issue or having an issue to report to the team. Tianlong asked about the appropriateness of considering issues differently across the different tiers of platforms supported by WAMR, especially in cases where lower priority platforms require hardware environments not readily available to project maintainers. Till explained that this same situation exists for Wasmtime, citing how it is [documented](https://docs.wasmtime.dev/security-what-is-considered-a-security-vulnerability.html) so as to be clear.

### Action Items and Next Steps
Reaching the end of the scheduled hour for the meeting, several follow-on action items were identified:
* Till offered to work with project maintainers to make sure all the right people have GitHub access roles and rights to WAMR project assets so releases and project operations can happen smoothly.
* Oscar will work with project maintainers to identify and explore varying business case relevance for different portions of the project with the idea of partitioning it to enable sunsetting unmaintained parts.
* Bailey will provide her list of additional desired documentation for consideration by maintainers and the TSC.
* The TSC (via Oscar) will provide a list of remaining concerns, updated based on the discussion at this meeting, to be used as topics for follow up between the TSC and project maintainers. Additional working meetings will be proposed to explore and resolve those concerns.
* David will prepare notes from the meeting to share with maintainers who were not able to attend, and also to publish as a public work product of the TSC.