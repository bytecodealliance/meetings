# June 28 project call

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
- cfallin
- avanhatt
- uweigand

### Notes

- alexcrichton: no updates
- avanhatt: no updates
- cfallin: EGRAPHS talk since last time
  - [slides](https://cfallin.org/pubs/egraphs2023_aegraphs_slides.pdf)
- fitzgen: tail-call PRs landed
  - probestacks, VM stack limits in progress
  - would be nice to have interpreter support for this as well
  - working on using tail convention for all Wasm on x86-64

- alexcrichton: ColorGuard (in Wasmtime), may experiment with this
  - cfallin: yes please, have been trying to get someone to work on this
  - cfallin: main issue is with trampolines to switch available colors between
    instances?
  - alexcrichton: can make a store allocate memories only in a single color
    (shard allocator by color); then we only need to enable color on entry from
    Rust
  - alexcrichton: single entry point from Rust to Wasm; leaving can go via
    traps
  - cfallin: piggyback on the async support and fiber library and let that do
    the color-register switches?

- uweigand: PR re: little-endian vectors on s390x with ABIs
  - ABI switches the preferred vector endianness: either little-endian or
    big-endian bitcasts are expensive (permute), other are no-ops
  - why remove Wasmtime ABIs?
    - Alex: cleanup in general, now that we have trampolines to handle many of
      the issues (multivalue etc)
    - current PR leaves Wasmtime ABI; only difference is s390x lane ordering
    - eventually we'll move to tail CC; do we want to make tail convention also
      little-endian-native?
    - yes; and for now let's leave WasmtimeSystemV
