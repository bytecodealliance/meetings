# October 18 project call

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

- elliottt
- afonso360
- abrown
- jameysharp
- fitzgen
- Viktar Makouski

### Notes

- Status updates
  - abrown: working on MPK stuff in Wasmtime; looking at failing tests,
    hardcoding Store layouts etc
  - afonso360: reviewing Alex's RISC-V PRs; reading about unwinding and how
    this might change with dynamic stack realignment
    - fitzgen: why dynamically realign?
    - afonso360: if embedder requests large stackslots with alignment
      requierments, we need to align if greater than ABI-guaranteed alignment
  - afonso360: user with slow Wasm (recent issue) -- looks like uarch issue
    maybe. Any progress on VTune?
    - https://github.com/bytecodealliance/wasmtime/issues/7246
    - seems to be an issue with moves between GPR and XMM, or something similar
    - abrown: working on it recently, talking with Raoul
    - issue with wiring up jitdump mapping for VTune
    - elliott: test also slow on my workstation

  - elliottt: no updates
  - jameysharp: no updates
  - cfallin: proof-carrying code! have end-to-end working for static memories
    in a nontrivial Wasm on aarch64; x86, and dynamic memories + tables are
    next
  - fitzgen: pairing with cfallin on PCC
