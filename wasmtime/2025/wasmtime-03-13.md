# March 13 | Wasmtime Project Bi-Weekly

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
   1. Note: meeting notes linked in the invite.
   1. Please help add your name to the meeting notes.
   1. Please help take notes.
   1. Thanks!
1. Announcements
   1. _Submit a PR to add your announcement here_
   2. fitzgen: The `@wasmtime-core` team has chosen the Wasmtime project's TSC representative
1. Other agenda items
   1. [Manually submitting security bugs to rustsec](https://github.com/bytecodealliance/wasmtime/issues/10344) (@alexcrichton)
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)

## Attendees

* Pat Hickey
* Nick Fitzgerald
* Erik Rose
* Till Schneideit
* Alex Crichton
* Chris Fallin
* Oscar Spencer
* Andrew Brown
* Paul Osborne
* Joel Dice
* Roman Volostavos


## Notes

Nick: Announcement, the Wasmtime project has been selected as a Bytecode Alliance Core Project, so we can select someone to serve on the BA TSC. Leaders of the project met and came to a consensus that Till Schneidereit will serve as Wsamtime’s TSC representative. Thanks Till. We will make an announcement soon.

Till: Thanks. I will work with David on the announcement of Wasmtime as a core project and revamp the website. Its hard to see that these things changed right now. Going forward, in this bi-weekly Wasmtime meeting, I’ll have a standing topic to bring up any TSC items that are relevant to or want input from this team, serve as a bridge there.

Alex: Manually submitting security bugs to RUSTSEC.

Alex: Dfninty contacted me, their auditing system primarily pulls from RUSTSEC (cargo-audit). They’d like for us to submit our Github Security Advisories to RUSTSEC. It seems like theres some tool that may automate this but its not mature enough. So the ask is that we add on to our security advisory runbook to copy a bunch of the details from our advisory into rustsec. Nick you’ve done this?

Nick: Yes RUSTSEC is just a PR you open with some details. We can just link to our own advisory. I wish that RUSTSEC would implement pulling from github, but in the meantime we will do it manually.

Alex: There’s a lot of stuff we would need to backfill from our security advisories into RUSTSEC. So that’s a one time cost. I’m not excited about doing this but given how infrequently we have security advisories its not the end of the world.

Till: Should we be yanking crates when we issue an advisory?

Alex: No, it creates a lot of churn and has various downsides. Cargo-audit allows people to opt in to being notified, without the downsides of not being able to use/reproduce builds if the security issue doesn’t effect most users.

Nick: We can treat it case by case, for example the 2021 sandbox escape probably warrants a yank, but most of the issues around reference types don’t warrant it.

Till: So we can add considering a yank to the runbook as well

Alex: I will open a PR on the runbook, respond to Dfinity that we will do this, and eventually get around to backfilling RUSTSEC.

Nick: Any more nonscheduled topics?

Nick: In GC stuff, my quick update, I have fixed all the bugs that the Kotlin folks have reported, other than (ed: something about transitive decrefs? Learning new words here) Next steps are turning the table-ops fuzzer into a more general GC fuzzer.

Till: Excited to hear Chris is starting on Exception Handling. Any plans for timeline?

Chris: I’m as always 150% booked. We will see. Zeroth is we trap if we throw. Then cranelift comes first, a few weeks of work. Then wasmtime. I’d be surprised if its not done by the end of summer.

Alex: one great question is when will Rust be able to use exception handling by default with catch panics? I think it will be quite a long time.

Nick: I’d definitely like to see LLVM be able to emit the new exception handling binary format before we try that, at the moment it gets translated by binaryen.

Joel: C++ exceptions and setjjmp/longjmp will unlock a lot of packages in Python, plus MicroPython and CRuby

Paul: And PUC-Lua

Alex: Yes setjmp-longjmp by default on exception handling is something I really want to land in wasi-sdk. My recollection is its there but you have to pass a bunch of flags to use it, but it should become a default as soon as runtimes support it.

Joel: WASIp3 CM work is coming along. The action is primarily in wasip3-prototyping repo, which is a working fork of wasmtime. Once we have more stability there I’ll change my focus to upstreaming stuff from that repo to wasmtime. Alex is working on wit-bindgen which will unlock support in wasi-libc and other places.

Alex: LTS Channels, one fine response to that RFC is to put the brakes on it until wasip3 is done. Does anyone think this is premature and we should definitely not do this now? (None). Does anyone object to what is currently in the RFC, which is that every 10 versions of wasmtime we will support that version for 15 months?

Till: It might make more sense to do a yearly cadence (12 months/18 months)

Nick: On one hand yes, but on the other because its at multiples of 10s its easy to keep track of, even if it migrates around the calendar. An LTS is just an ordinary release.

Till: Ok, any additional effort for LTS is not bound to calendars because its fixes as they come up, rather than at time of release.

Nick: I imagine that users will upgrade to the latest LTS every 

Chris: Theres a 5 month window in which two LTSs exist and you have to upgrade during that time. That time will change around the year. 20 months of LTS support is not too much more of a burden than 15, and that means anyone can upgrade to the latest LTS at any time during the year.

Alex: I will think hard about how to tweak this.

Andrew: Are we satisfied with the tooling and processes for supporting LTSs?

Alex: Yes there will be a weekly CI job that makes sure the LTSs are good.


