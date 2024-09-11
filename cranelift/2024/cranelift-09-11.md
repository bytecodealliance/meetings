# September 11 project call

**See the [instructions](../README.md) for details on how to attend**

## Agenda
1. Opening, welcome and roll call
    1. Note: meeting notes linked in the invite.
    1. Please help add your name to the meeting notes.
    1. Please help take notes.
    1. Thanks!
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- fitzgen
- alexcrichton
- avanhatt
- uweigand
- Dimitris Aspetakis
- elliottt
- abrown
- cfallin

### Notes

- Updates
  - elliottt: no updates
  - abrown: no updates
  - uweigand: no updates
  - avanhatt: working on upstreaming isle-veri; got review, responding to
    comments
  - alexcrichton: delayed fuzzbugs appearing (oss-fuzz wasn't filing bugs for
    three weeks)
  - cfallin: reviewed upstreaming of isle-veri; RA2 fuzzbug
  - fitzgen: published blogpost about new stackmaps system
    - https://bytecodealliance.org/articles/new-stack-maps-for-wasmtime

- Dimitris: instruction scheduling, still looking at different heuristics
  - see convo at [Zulip](https://bytecodealliance.zulipchat.com/#narrow/stream/217117-cranelift/topic/Better.20Heuristics.20for.20Instruction.20Scheduling)
  - looked at spill/reload count; actually gets worse
  - dependency-driven approach tends to put chains of ops together
  - cfallin: would be good to understand why we *improve* in the cases we do as
    well. can we look at spill count in XNNPACK? does it decrease?
  - (some discussion about OoO scheduling, whether ILP per se will be affected
    or is it just spills/reloads)
  - cfallin: would be good to look at perf counters as well
  - alexcrichton: try VTune too
