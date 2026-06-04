# June 04 | Wasmtime Project Bi-Weekly

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
   1. Find volunteer note taker. Thanks!
1. Announcements
   1. _Submit a PR to add your announcement here_
1. Other agenda items
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2026-05-21)
   * [Old, Backlog Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aissue%20state%3Aopen%20sort%3Acreated-asc)

## Attendees

* alexcrichton
* fitzgen
* bailey
* cfallin
* pchickey
* posborne
* roman
* Sebastian Guillemot
* Tolumide Shopein

## Notes

### GC / Exceptions

* fitzgen - wasmtime 46 forks tomorrow. Switched from drc to copying on `main`,
  but GC still disabled by default. Make a PR to enable GC by default once that
  forks off -- to release in Wasmtime 47. Then Wasmtime 48 is LTS. Lots of
  fuzzing/bug-hunting/hardening/etc been happening, think it's ready.

* alexcrichton - "counter" reset when new issues crop up.

* fitzgen - interesting interpretation of the 2 weeks of fuzzing...

* alexcrichton - I think let's just reconvene at Wasmtime 47 and revisit this
  and confirm we want to ship.

* cfallin - I think we should ship exceptions at this as well.

* alexcrichton - exceptions/GC are in the C API but not in other languages,
  shouldn't be a blocker though.

* cfallin - most of it is guest language uses it -- just want to run the
  component. Ok I'll plan on sending a PR after wasmtime 46 branches.

### Sightglass / Benchmarking

* bailey - saw PRs going by and been talking recently -- wanted to walk through
  what I've been doing and get some feedback/thoughts. Tried out bencher and 3
  other SaaS solutions and all use micro vms. Probably why rust infra went with
  their own hetzner box. Tried out bencher and it was pretty nice but micro-vm
  scared me off. Not at lot of compute on the free tier. Could go back if vms
  aren't a worry. This is running on amd ryzen 5. Could go with the 6 for
  slightly more expense. Staged with ansible. Ryan may have done some of this?
  Uses a cloudfront API in front of S3 to avoid paying too much for egress. Each
  run is in its own bucket and also has summarization for a dashboard.
  Combination of criterion and gungraun ("AI valgrind/cachegrind"). Wanted the
  ability to trigger this from github. Could trigger sightglass runs from
  github. Testing out p2/p3, ValType vs composed, etc. Does OIDC keyless, got
  most of the layer running for pennies. Only thing to pay for from BA is the
  hetzner box. Probably will want at least arm and x86. Needs work on the github
  actions side of things. Lots of scripts here to get working. Trying to get
  something fully automated. I think I have a path here but want to know if
  we're totally against our own hetzner box. Can use any CDN or a json on github
  or such. Would perfer to avoid having too much in github though. High level
  sketch is having things from a github workflow, runs with your identity, uses
  bot identity to upload to s3. One run is about a megabyte for sightglass --
  it'll grow over time. Well within pennies tier for s3. Feels too much to have
  in github though which motivated the external storage.

* fitzgen - initial reaction is that this feels like a lot more moving pieces
  than expected/hoped. Hoping for static html in workflow and hosting on
  gh-pages

* bailey - could do that for the dashboard side yeah. For json summarization
  could be the output and could commit that to github.

< ... missed a bit ... >

* bailey - one time setup of a box to set things up. Would be sightglass's
  runner.

* posborne - for benchmarking we know the free tier of github actions will never
  be appropriate. Results are too noisy. For example hetzner box. Seems like a
  nice integration point. Someone will need to be on the hook for making sure
  the infra does not disappear. Would prefer to have something with a slight
  problem in the future than nothing today. Reducing dependencies on s3 does
  seems nice though. Easier for sightglass maintainers to frob and run with. For
  example adding new benchmark and want to run against older wasmtimes. Or if a
  new visualization is added. Will need to work through those individually.

* bailey - ok commiting data to repo to start, and won't worry about changing
  that until it becomes a problem.

* posborne - happy to prototype that. Ideas on that nick?

* fitzgen - one other thing is that we could get reliable data on github actions
  from callgrind in theory. Could get interesting PCA metrics in theory too.

* bailey - gungraun was pretty nice, but seems like a big change for sightglass.

* fitzgen - can't use directly but valgrind has easy way to turn instrumentation
  on/off at various times. Could add a "callgrind measure" to sightglass.
  Shouldn't be too hard. Could then put everything in github actions. In theory
  the same as CI. Would be my ideal -- less s3 buckets, dedicated machines, etc.

* bailey - saw variance with what I was running with gungraun. Unsure where that
  was from. Nice to not have to sequence things with github actions. I don't
  think it's the complete solution, just part of it. Enough to know if something
  regressed, but won't give enough for what people want to know -- throughput on
  something for example. Probably going to be a desire for a small set of machines.

* fitzgen - callgrind will have variance due to ASLR and such. Wouldn't expect
  it to be a huge amount. Can you speak more to variance?

* bailey - <...showing desktop...> - can't say how many instructions were off
  but it raised an eyebrow. My heuristic did fire for a regression. Wouldn't
  have expected it to vary. Didn't change bench or what was on main. Was spooked
  though, will get back with more data.

* fitzgen - unsure how easy it is to get raw samples for each run. Could get one
  run one day all the samples, then the next day, and see if there's difference.

* bailey - easy to get all samples yeah, they're artifacts. Not worried about
  being a self-hosted runner.

* alexcrichton - is callgrind x86 only?

* cfallin - should have support, but it's double-emulation.

* bailey - would that give slow but accurate results?

* cfallin - should be deterministic. Cachegrind is measuring a counter it
  increments.

* alexcrichton - want to make sure we don't have to rearchitect the entire
  system to add new architectures in the future.

* cfallin - valgrind/cachegrind should have support for architectures. Almost
  better than perf counters as x86 may give different stats than aarch64.

* bailey - cachegrind/callgrind seems useful, but I think we'll want more
  coverage too.

* cfallin - there's an old paper on variance -- lots of random things can affect
  perf of hot inner loops. They're solution was randomization. That's one path
  and the other is deterministic computation -- just count instructions/cache
  misses. Then you're computing a pure function of an abstract model instead of
  relying on libc. It's a stronger argument for deterministic answer.

* bailey - if you can get a measurment in sightglass to support
  cachegrind/callgrind in sightglass that seems useful.

* fitzgen - callgrind is a superset of cachegrind. It's more deterministic but
  not deterministic in practice. Still affected by ASLR. Unsure if we've agreed
  on what we want to measure and how we want to present things. Leading to some
  X/Y problems. We haven't figured out what the graphs are graphing yet.
  Unsure if this is an RFC or just getting folks together.

* bailey - wanted to run a survey of what's out there. Interesting what rustls
  vs rust infra vs the go ecosystem. Everyone has pretty strong opinions and not
  much agreement. Another good answer with callgrind is that bencher supports
  that well.

* fitzgen - a callgrind measure will still look like sightglass at the end of
  the day. Not going to be just running a binary with callgrind.

* bailey - for wasmcloud wrote a tool to coerce output -- basically what
  sightglass does. No generic answer when surveying what others are doing. What
  comes up here is going to be specific to sightglass. Measurement with
  historical data is going to be important. Would probably benefit from a
  github-based thing. I can make a sketch of what I'd recommend and put in a
  github issues. This has been helpful.

* fitzgen - filing an issue on sightglass and/or wasmtime sounds good.

* fitzgen - one other thing on this topic -- one thing is the graph over time
  and historical trends. Another is a job that runs as fast as it can every
  night that does a statistical test to see if yesterday faster/slower than
  today and auto-file an issue. Won't graph things over time but will file
  issues and @-authors. Maybe larger time windows occasionally too. That's
  another aspect but separate from graph-over-time.

* bailey - using same historical data? I want to get to the point of a callgrind
  comparison between two runs to test for regressions. Would probably be a good
  first step without much work. Biggest work would be getting the measurement
  done. If we only target callgrind that scopes down significantly.

* fitzgen - shouldn't need to use same historical data. If it doesn't it frees
  us up to do real wall-time. It's using same machine so doesn't matter if it
  changes day to day.

* bailey - so check out and do both runs?

* fitzgen - yes, and both at the same time to help randomization. Can pass two
  engines to sightglass.

## Last Old, Backlog Issue

*This tells us where to begin the backlog triage next time.*

TODO
