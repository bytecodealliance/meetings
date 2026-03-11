# March 11 project call

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

- alexcrichton
- fitzgen
- uweigand
- avanhatt
- cfallin

### Notes

- uweigand: no updates
- alexcrichton: no updates
- cfallin: no updates in CL, just some reviewing; landing debugging in Wasmtime
- fitzgen:
  - in reviewing mid-end PRs, put together a Z3 SMT harness for testing things;
    any interest in putting it somewhere? stopgap until we can land full
    verification work
  - playing with effects-as-values for explicit dependencies; still in
    progress. control-flow effects def'd on edges like we do for exception
    payloads etc
  - cfallin: super cool; what's the experiment we can do to show this unlocks
    new potential?
    - initially trying to get it to work; eventually, control flow in
      elaboration to show more freedom in doing control-flow optimizations
  - cfallin: in fullness of time would be good to see how far this could take
    us in removing other special-case / ad-hoc control-flow transforms
  - ensure compile-time overhead is low or none in no-opt config
  - working on slides for Wasm I/O -- talking about Cranelift + Wasmtime
    end-to-end to run a "helo world"
- avanhatt: PR to add type arg to instruction ctors/etors in backend ISLE
