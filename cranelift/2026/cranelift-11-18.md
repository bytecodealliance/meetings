# November 18 project call

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

- adamrk
- alexcrichton
- fitzgen
- cfallin
- bjorn3

### Notes

Updates

- adamrk: no updates
- alexcrichton: no updates
- fitzgen:
  - looking in to speeding up compile-time builtins (Wasmtime); lots of things
    that in combination could unlock nice improvements but any individual opt
    by itself won't. Trying to untangle now.
    - tail-dup, loop unswitching, ...
  - sending PRs to RA2 to improve a few things noticed while looking at
    compile-time builtins
    - handling cold blocks better re: splits
- cfallin:
  - triage of a bunch of issues about quadratic behavior; many N/A because
    fundamentally we're an optimizing compiler but a few PRs from contributor
    to improve things where we can
- bjorn3: no updates
  - question: depending on wasmtime jit debug crate in cranelift-jit? yes,
    should be ok
