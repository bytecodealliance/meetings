# April 11 project call

**See the [instructions](../README.md) for details on how to attend**

## Agenda
1. Opening, welcome and roll call
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. _Submit a PR to add your item here_

## Attendees
- Chris Fallin
- Saúl Cabrera

## Notes

Fuzzing:
  - Saúl looked into fuzzing last week. It's not straightforward to get
    something working due to the current support for all the instructions in
    Winch and due to the categorization of instruction support in `wasm-smith.`

  - One approach that has been used in the past is to manually (via
    `wasmparser`) filter out the a set of accepted modules according to the
    Wasm features supported by Winch. This could be useful for local verification,
    in the case that we don't want to enable such fitering in cluster-fuzz (to
    avoid wasting fuzz time).

Other updates:
  - As part of https://github.com/bytecodealliance/wasmtime/pull/6119 we had to
    disable some test on Windows. Saúl is focusing on improving support for
    other calling conventions so that we can enable all tests in x86_64.

