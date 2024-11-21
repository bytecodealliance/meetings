# November 21 project call

**See the [instructions](../README.md) for details on how to attend**

## Attendees

| Name              |
|-------------------|
| Chris Fallin      |
| Danny Macovei     |
| Till Schneidereit |
| Alex Chrichton    |
| Johnny Birch      |
| Andrew Brown      |
| Victor Adossi     |

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

Since there was nothing on the agenda, we did some discussion

### New Backends (Alex)

- Alex: While working on pulley, found that it was somewhat difficult to create new backends
- Better support for in-progress backends would be great
- Chris: Used to be psosible to remove some tests
  - Alex: Used to have htis in the past but current system is that we have some wasm features that are known to possibly panic
  - WAST tests
  - winch arm64 is not in a great spot -- there's no support for supporting "half" of a proposal
- Andrew: Reviewed the PR with wasmtime test macro, we were ignoring tests instead of should_panic'ing. Is part of the problem that it's too hard that it's too get an error anywhere?
  - Alex: this was a specific test, that PR was mostly for sharing logic on backend selection
  - Alex: We should probably assert no segfaults (i.e. error or panic)
- Alex: To sum we should do more to enable development on new backends (which don't support everything).
  - If you don't support anything, maybe just report a first class error.

### Performance (Till)

- Till: Alex, maybe you want to talk about some other
- Alex: Ampere gave us a large machine (128 core) -- checked spin, wasmtime & tokio
  - Findings:
    - Memory management was not our friend
    - kernel upgrade 6.8 -> 6.11
    - wasmtime was much faster with turning off the mmap syscalls & instance reuse
    - We have a mode where we can turn off mmap syscalls (i.e. turn off guard pages, reservations, bound checks)
      - special boolean check in allocator if you're shrinking and will never access it
      - STILL saw locking inside of pooling allocator (async stacks were still being processed)
      - There is now a single stack in the store
    - Helped: Instance reuse
      - We have no way of *not* doing instance reuse
    - Helped: memset rather than cow
    - Helped: switching to jemalloc
    - 1.28 million requests (10k per core), but w/ same system loadgen
      - Same for WASM or NOT, so almost "free"
      - Dominated by TCP readwrites, not on CPU
    - Chris: How close to running in userspace did this get?
      - Alex: mode of doing zero syscalls is probably not reasonable
        - i.e. if you're using Spidermonkey and have to allocate then that will domainte
    - Chris: on the same hardware do you have a known upper bound (ex. NGINX etc)?
      - Alex: Used wasmtime serve *except* hacked out the WASM stuff to only be tokio
- Victor: Would be nice to have this in the main wasmtime repo, at least to have *a* number?
  - Alex: Yep, would be great to have but it is a lot of work to get right (e.g. Rust has this but put a LOT of work into it)
  - Till: did run into this (https://docs.codspeed.io/what-is-codspeed)
    - Haven't tried this but free for F/OSS, etc
    - Alex: would be great to have this set up
    - Andrew: Johnny worked a lot on benchmarking for PR, haven't seen many people using it yet
      - Johny: Alex helped with this, haven't used it in a bit
      - Alex: have it but no has typed /bench in probably 2 years.
    - Andrew: Also built some automation for getting stuff out of sightglass and getting into kibana
      - Stopped at the point where becoming a sysadmin would have been necessary to maintain the infra
      - Could ressurect this, but it's a ton of work
    - Till: Example dashboard for codspeed (https://codspeed.io/pydantic/pydantic-core)
    - Andrew: More explanation on the TechEmpower benchmarks?
      - Andrew: Wasi HTTP could be a point in this list?
      - Till: https://tfb-status.techempower.com/
        - Latest run: https://www.techempower.com/benchmarks/#section=test&runid=7c4d8c23-ca8d-4c77-be5f-3fd4f18687b5&hw=ph&test=fortune
      - Andrew: this could increase trust in the benchmarks as many tests are component specific, but would be nice to have this -- other people consider this "real world" enough
- Andrew: We probably don't want to lose access to sightglass, but one problem is that we haven't done the work for components
  - Core modules still work but it might be good at some point to convert those into components
  - Alex: In general the modules are the internals of components, but we would need something new for component <-> component communication
- Victor: which repo is this? wasmtime-sightglass-benchmarking?
  - Andrew: https://github.com/bytecodealliance/sightglass
  - Victor: ahh I thought this was wrong (https://github.com/bytecodealliance/wasmtime/blob/7968af624ae5bb79e99e8a49aa09b0bfe4b70659/.github/workflows/performance.yml#L65C64-L65C115)
    - Alex: Yes, this is required, becuase GH recommends not using a bare metal runner on a public repo, to avoid arbitrary code execution
