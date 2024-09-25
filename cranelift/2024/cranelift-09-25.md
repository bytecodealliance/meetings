# September 25 project call

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
- elliottt
- alexcrichton
- abrown
- cfallin

### Notes

- Updates
  - abrown: trying to make shared-everything threads work in wasmtime; no
    Cranelift updates
  - elliottt: no Cranelift updates; last meeting (leaving Fastly after next
    week)
  - alexcrichton: 128-bit arithmetic: plan to try to move it to phase 2 at
    beginning of October
  - cfallin: reviewing code
    - isle-veri: almost in
    - RA2: Jakob Doka has a nice speedup with reusing allocations; will review
      and hopefully get it in
  - fitzgen:
    - chat with docs folks; triaging Zulip, regular questions, could we
      update docs
    - WasmGC almost done

- fitzgen: MemFlags and trap codes
  - right now: we pack trap codes into MemFlags
  - CL defines its own trap code enum; Wasmtime defines its own
  - should CL have trap codes? probably not: carry through an index instead
    - cfallin: like how we handle symbols, etc today -- CL is agnostic to
      embedding's notion, just has an index space
    - alexcrichton: make it a u8, we can fit that entirely in MemFlags

- alexcrichton: remove cranelift-wasm as a separate layer?

