# December 24 SIG-Embedded Meeting
## Asia Pacific and US Timzone meeting
**8am China, 8pm US East Coast *$*, 7pm US Central *$*, 5pm US Pacific *$*, 2am Central European Time**
**NB:** *$* - Due to the international date line, 8am on Tuesday, is late Monday evening in the USA

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announce that the meeting will be recorded and hit record as agreed upon in inaugural meeting. 
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. _Submit a PR to add your item here_

## Attendees

* Chris Woods
* Oscar Spencer
* Luke Wagner
* David Bryant
* Andrew Brown
* Liang He
* Tomoya Fujita
* Emily Ruppel
* Merrill


## Notes

### Discussion of Alternative Solutions Document

- Document links
  - [Alternative Solutions Document](https://github.com/woodsmc/sig-embedded/blob/Adding-document-for-comment/ExploringOptions/Alternative_Solutions_(WASI_Wasm).md)
  - [PR discussing Doc](https://github.com/bytecodealliance/sig-embedded/pull/18)

- Observations shared that doc successfully defines needs for Wasm for embedded
  - Goal is to inject a sense of urgency
  - Call to action: make sure that we're tracking what TODOs are
    achievable now

- The document discusses balancing the need to go to market, while dealing with
  the fact that p2/p3 are coming, but p1 is what's available.
  - Tension with messaging: internally, we know WASI will change, externally,
    this messaging makes WASI look like an unachievable moving target

- Big question: what's the supportability plan for WASI?
  - Could be CVEs only
  - Next level is expanding APIs
  - What interfaces need to be added to WASI in embedded devices:
    - Signal handling
    - WASI sockets
    - WASI threads (for real)
    - Thread prioritization, e.g., RT thread
    - Shared memory
    - Other shareable features

- Should some of the new features be defined in an IDL?
  - What's wrong with WIT?
  - Nothing! Could decouple ABI improvements and module interfaces thanks to WIT

- Issue of snapshotting for embedded products:
  - Can't count on consumer owned devices having up to date software
  - Necessarily, the runtime and the tooling to work with it have to be
    preserved
  - Should we snapshot at p1 or p2? P2 has WIT, etc. Reasons for p1:
    - Performance compared to p2
    - Widely deployed and used
    - Relatively stable
    - WAMR implements it
  - Ultimate goal is still to allow for business continuity, so flexibility
    across runtimes

- Who do we talk to about getting a sense of the cost to preserve WASI?
  - Dan Gomen,  Alex Crighton, Sam Claig

- Question: can we get a benchmark suite and discussion of useful metrics for
  embedded companies?
  - Issue is that the runtime budget for real time systems is very tight, don't
    want to spend time on IO

- Has the performance requirement for embedded been clearly articulated in the
  document?
  - Need more details, lots of different IO patterns may be satisfied by
    existing p2 APIs
  - Benchmarking to articulate performance requirements:
    - Can shared everything work?
    - Should add shared everything components to benchmarking list
    - Request to plot out use cases to discuss which ones are easy to optimize
      away in the compiler

- Discussion of developing a LTS version of WASI
  - Easy: support of LTS for p1
  - Hard: support of rc1 because there's no set release date, no sense of when
    it goes into a product
  - Issue: we can't slow down the progress of the WASI-spec, we need folks who
    need p1 to be supporting it.

- Thing to elaborate on:
  - Support "unwrapping" of p2 components so that they'll run on p1 runtime
  - Main idea: stay on thread that other people are working on
  - Idea: could leave core Wasm in control loop, leave component outside of
    tight control loop
  - Need to be able to run a component on a non-component enabled runtime
  - Need to be able to bound level of backwards compatibility

## Action Items

* [ ] Meeting with Dan Gomen,  Alex Crighton, Sam Claig
* [ ] Collect benchmarking efforts
* [ ] "Finish" document and deliver to standards body
