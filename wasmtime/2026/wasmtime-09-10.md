# September 10 | Wasmtime Project Bi-Weekly

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
   1. Find volunteer note taker. Thanks!
1. Announcements
   1. _Submit a PR to add your announcement here_
1. Other agenda items
   1. erikrose: State, benchmarks, and way forward for [MMU-based interruption](https://github.com/bytecodealliance/wasmtime/pull/12990)
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2026-08-27)
   * [Old, Backlog Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aissue%20state%3Aopen%20sort%3Acreated-asc)

## Attendees

* alexcrichton
* fitzgen
* cfallin
* erikrose
* Adam Bratschi-Kaye
* Daniel Hillerstrom
* pchickey
* rvolosatovs
* Simon Jonathan
* Victor Adossi

## Notes

### MMU Interruption

* erikrose - ... presenting slides ...

* acrichto - agree with conclusion, seems good to land in tree and continue to
  iterate

- erikrose - would like to get the public API settled

- fitzgen - lots of open questions but definitely looks promising. R.e. how to
  wake up stores and how to sleep -- maybe a timer wheel? A priority queue of
  things to wake up and then do all of those at once.

- erikrose - wouldn't mind some help on that. Unsure how to tell how many ms is
  on the cpu.

- fitzgen - each fiber has a deadline of when they should yield by.

- erikrose - that's existing behavior and not using epochs yeah. Punted
  interrupt loop to the embedder for now.

- fitzgen - right now we say epoch deadline is in the future, can we say the
  same for interrupts?

- erikrose - seems reasonable yeah, I'll try

- cfallin - can wasmtime not worry about timers/deadlines and can there be an
  "interrupt" method?

- erikrose - that's what's implemented yeah

- cfallin - embedder probably wants to implement the wheel

- erikrose - yeah that's what `wasmtime serve` does

- cfallin - embedder probably wants to implement the wheel

- fitzgen - what are you aiming for in the immediate next steps? Start splitting
  things out to review/land? Focus more on answering benchmark/follow-up?

- erikrose - want to address some review comments. Need to understand cfallin's
  comment about register choice. I'll grab IPI and perf counters

- cfallin - TLB counters and cache misses

- erikrose - also some shorter workloads.

- cfallin - perhaps also spidermonkey.wasm serving HTTP (nick brough this up in
  chat).

- erikrose - wouldn't have my knobs to test but would be useful yeah

- cfallin - would be helpful to see realistic workload

- fitzgen - might be nice to split out the cranelift changes to its own PR.

- acrichto - would like to make things as small as possible to review to handle
  the unsafe bits.

< ... notes lost some context ... >

- fitzgen - in theory the pointer never changes and could be inlined into the
  VMContext to avoid a layer of indirection. Could store the page pointer in the
  VMContext. Not necessary but would remove an indirection.

- cfallin - paying an additional word of storage per instance though. Probably
  still a big win though.

- fitzgen - have infrastructure to have fields that are optionally present. Can
  avoid changing every disas test as a result.

- cfallin - r.e. per-store -- can a page be shared by multiple different store?

- erikrose - not yet but there's middle-of-road approach where stores have
  adjacent pages to all mprotect.

- cfallin - would affect API as there'd be a thing to pass into the store to
  setup interrupt groups and it would be configured separately.

- fitzgen - seems reasonable to me and feels more flexible. Instead of a timer
  wheel could wake up N stores at a time.

- cfallin - could help the big-O bounds too

- fitzgen - gives embedders flexibility to do what they want

- fitzgen - what platform/architecture support is implemented?

- erikrose - x64 and aarch64 on Linux, requires async and some other
  miscellaneous features.

- cfallin - might ask to get all architectures at once with the Cranelift side.

## Last Old, Backlog Issue

*This tells us where to begin the backlog triage next time.*

Start next week at ~#1313 (best of alex's memory of what was on-screen 30s prior to writing this...)
