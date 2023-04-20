# April 20 SIG-Debugging Meeting

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. _Submit a PR to add your item here_

## Attendees

- Rainy Sinclair
- Nick Fitzgerald
- Ulrich Weigand
- Jeff Charles
- Saúl Cabrera

## Notes

#### Ongoing work on Coredumps in Wasmtime and in `wasm-tools`

- This will enable an initial debug experience, to be exposed through a protocol
  like DAP.
- There's some existing coredump Core Wasm tooling that we might want to loop in.
    There are some open questions/thoughts behind this statement:
	
  - How to handle core dumps with multiple Wasm modules? https://github.com/WebAssembly/tool-conventions/issues/204
  - Bring the existing tooling into our own ecosystem (of tools that we maintain)

- The objective of this work is to side-step the hard questions for now and get something working as first step.
- All this work depends on Wasmtime's ability to generate coredumps, which is a
  possiblity right now, but we might want to  enhance that process and make it
  available throught the Rust embedding API too.

## Action Items

* [ ] TODO
