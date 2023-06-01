# June 01 SIG-Debugging Meeting

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. _Submit a PR to add your item here_

## Attendees

* Jeff Charles
* Rainy Sinclair
* Nick Fitzgerald
* Saúl Cabrera

## Notes

### Update on Coredump Work

* Core dump support on Wasmtime, if configured, Wasmtime will create a dump.
  * There's a PR coming.

* For the RFC discussion from last meeting everyone agreed that we should do an
  RFC.

### Live debugging

* After core dump debugging, the next step should potentially be live debugging.
  * Single stepping and breakpoints.
* We should discuss if Winch is the right place to start with debugging, since
  normally in most systems debugging is done at the baseline level.
  * The only immediate disadvantage with this approach is that Winch is still on the early stages,
    but if there are no major architectureal changes that are needed in the
    compmiler itself, it could be easier to get help with parallelizing filling in the missing
    opcodes. 

## Action Items

* [ ] Saúl Cabrera will post an issue in `wasmtime` with the list of missing
  opcodes.
