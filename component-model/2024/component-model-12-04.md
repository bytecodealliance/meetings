# December 04 Component Model Implementation Bi-weekly

**See the [instructions](../README.md) for details on how to attend**

## Agenda
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. Planning & Coordination for work towards WASIp3

## Attendees

| Name              |
|-------------------|
| Oscar Spencer     |
| Brian             |
| Danny Macovei     |
| Hung Ying-Tai     |
| Luke Wagner       |
| Roman Volosatovs  | 
| Till Schneidereit | 
| Alex Crichton     | 
| Victor Adossi     |
| Bailey Hayes      |
| Joel Dice         |
| Yoshua Wuyts      |
| Dan Gohman        |

## Notes

### WASI p3 (Till)

- Till: [Ship Wasi p3 Project board](https://github.com/orgs/bytecodealliance/projects/16)
  - We've put together this project board to stay aligned and have a reasonably complete view of important & blocking tasks
- Coordination will happen in the [#wasi channel on Zulip](https://bytecodealliance.zulipchat.com/#narrow/channel/219900-wasi), with separate conversations branching off into their own topics as necessary
- Joel: As an update
  - Work has started on the Rust bindings, others who want to jump in on other bindgen targets can get in touch
  - JCO support discussions have started with JAF Labs
  - Defining WITs for WASI P3 (i.e. what does `wasi:filesystem` look like in a P3 world?)
    - Once this is there someone can start implementing in `wasmtime-wasi`
    - There is a poor man's version of WASI 0.3 in Joel's repo
    - We're working on a tokio POC test that will enable use of POSIX
    - Creating an adapter that works for P2 -> P3 also needs to be started
    - WASI FS changes (namepsaces, etc)
      - Dan: This isn't urgent for P3
  - `wasm-tools` PR is up and ready -- there are some breaking changes that might need to go in a follow-up
  - `wit-bindgen` PR is mostly ready, in draft state due to use of wasm-tools fork
- Till: Is there anything that isn't in here that *should* be?
  - Yosh: we should take the `wstd` crate and make sure it works as a test, targetting something WASI specific might work as a decent baseline
    - Till: The real acid test is probably ecosystem compatibility, but this works as a test as well
    - Prioritizing ease of use for real-world is also important, and reducing friction for ecosystems
    - Yosh: What is our opinion around libc as a route for compat? If we have that do we need to test tokio quite as hard?
      - Till: most important bit is whether tokio works or not (whether through libc or not) -- with/out guarantee of tokio merging or not
      - Dan: Problem with libc is that the interfaces that they support have linear time properties
        - Joel: epoll doesn't suffer from that, should we consider doing that to help people as well?
        - Till: Might make sense to discuss this in an issue
      - Joel: What we have learned (e.g. interacting with `mio`), it is easiest for upstreams to treat this via a library they already support (POSIX, epoll, etc)
        - Doing all the work in `wasi-libc` could magically unlock a lot of downstream things
      - Till: If changes to `mio` are needed
      - Joel: Doing most of the work in `wasi-libc` seems to be the way forwward after some discussion with Dave and others -- turning wasi request/response futures into pollables and then into file descriptors, along with other higher level notifications
      - Alex: This is probably one of the biggest risks of p3 -- it's neither correct to take epoll into the component model or component model into `mio` as-is.
        - It is likely that `tokio` and the CM will have to change (although not radically)
        - Till: Have had some discussion with maintainers and there is a willingness to upstream changes
        - Alex: minimizing the amount of WASI specific code is likely important
    - Bailey: I can make sure that the Go drive is resourced.
  - Yosh: Would like to change what the WASI acronym stands for "WebAssembly System Interface" -> "WebAssembly Standard Interfaces", proposing something in January likely
    - Till: How about something on the list about sorting how we communicate P3 as a new thing (we need to do this before we announce it anyway)
- Bailey: Ideally next time we meet we can go through this like a standup
- Alex: One thing thats not on here is some sort of resourcing evaluation -- runtime impact of the changes.
  - We want to avoid the assumption that WASI P3 will always work/be absolutely better than P2. P2 -> P3 adapter may introduce some inefficiencies
  - We will have to come back and do performance evaluation
- Luke: `nonblocking` attribute for functions that can be added to (ex. `constructor`s, getter/setter syntax)
- Luke: Thinking about `_start` functions -- are they async, and do they do blocking stuff
- Luke: Tweaks to make `wasi-libc` work are important
  - Victor: What about Wizer `_initialize` ? Maybe it should be considered *always* not async? Or should it be possible to
  - Luke: Probably important to think about how that should fit in as well with the `_start` context
