# February 12 | Wasmtime Project Bi-Weekly

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
   1. Find volunteer note taker. Thanks!
1. Announcements
   1. _Submit a PR to add your announcement here_
1. Other agenda items
   1. Reliability of and dependence on GitHub CI
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2026-01-29)
   * [Old, Backlog Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aissue%20state%3Aopen%20sort%3Acreated-asc)

## Attendees

* TODO

## Notes

### GitHub CI unreliability
* Chris: Have been frustrated during [security release](https://github.com/bytecodealliance/wasmtime/security/advisories/GHSA-vc8c-j3xm-xj73). 3 GH outages in the last 3 Mondays.
* Could be a problem if it had an outage during a security release. And blocks merges in general.
* Should we switch platforms? Should we have a backup platform? What would the cost be? How bad would things have to be to make us go shopping in earnest?
* Alex notes that GH has 1 9 of uptime in last 90 days: https://mrshu.github.io/github-statuses/
* Nick: Previous Cranelift meeting agreed that maintaining CI, if not on GH, would be 1 FTE. A company that cares would have to invest.
* Chris: Worst case is if we have an exploited vulnerability and can't issue a build.
* We could model it as a backup system. Rig it so the runner could run on GH *or* another host. But there's no easy solution: there are no docker containers for Windows or Mac.
* Not only have GH's runners gone down but their control plane as well, so Fastly's runners, already set up, wouldn't be a complete solution.
* Till: Realistically, moving our entire CI would be the minimum helpful step. We would probably have to host our own releases as well.
* MS gives us 500 concurrent runners for free atm. It would be costly elsewhere. A full build uses about 300.
* Till estimates $hundreds-of-thousands just for the migration project.
* Is there a line that would be unacceptable? If it's down every Monday, perhaps you could count the cost as 1/5 of our salaries.
* Nick: this is just a group gripe session for now. No action likely.
* Chris suggests we should reexamine if we have another 2-6 months of being down every Monday.
* Till doesn't feel the impetus to fight for the money to migrate until business owners feel the pain. It's not like we can't do anything at all while it's down. He also doesn't think anyone using wasmtime in a security-critical setting depends on our binary releases. Our security announcements go out when the fix is pushed, which means GH is up at the time, so folks can get the source.
* Alex: Releases take perhaps 800 jobs with all the various PRs (version bumps, etc.), which takes a few hours, which is a long time to declare with confidence that GH won't go down.
* Nick would like to split the building and the running of test binaries. The running could be firewalled off the internet.
* Alex: Except the WASI network stuff does need network.
* Chris: Till has noted we should make more use of caching downloaded artifacts.
* Till: Tool downloads are a big source of our flakiness.
* Nick: And apt.
* Alex: It is an art to make a CI system where you can enjoy caching but where it's also easy to change things.
* Till: Optimize for team-member frustration. I believe this is identical with business value.
* Chris: No business users have yet been harmed by this, so it's all about dev velocity. The zeitgeist is turning against GH for their unreliability, so they may shape up on their own.


### Guest Debugging
* Chris: Getting close to guest debugging. Will soon push WIT addition that adds debug entrypoint.

### Components Restrictive for Practical CLI Tools
* Till: Often my Rust CLI tools cannot be components because they need to do things WASI doesn't permit. What if there were an opt-in flag that would let me, e.g., access the FS outside the cwd? It would still be more sandboxed than going outside WASI. Another example: invoking wizer without embedding it.

### OOM Handling
* Nick: How do we document what handles OOM and what doesn't? Atm, you have to be me and know what APIs are covered by OOM tests.
* Most things currently abort on OOM: the stdlib prints a message and then aborts, because stdlib collections aren't exception-safe.
* Alex: Also, panicking requires allocs, and no effort is done to ensure the first alloc would succeed. Perhaps anything that doesn't require the `std` feature could be made to handle OOM. It will likely be a long-term learning process to see what triggers OOMs.
* Nick: In the runtime module and beneath, can we expect we'll use only our OOM-handling collections?
* Alex: Probably no; there will be corner cases we won't want to do OOM handling. For example, all of debugging may not handle OOMs.
* Chris: Turns out cranelift has a BTree, and that's all we need for debugging, so we're fine there. However, your general point stands.
* Alex: It's a drag to tell at a glance between 2 things named "vec", one of which handles OOMs and needs the `?`. Could call one `oom_vec`, but naming is hard.

## Last Old, Backlog Issue

*This tells us where to begin the backlog triage next time.*

TODO
