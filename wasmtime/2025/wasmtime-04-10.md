# April 10 | Wasmtime Project Bi-Weekly

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
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)

## Attendees

* Chris Fallin
* Alex Crichton
* Nick Fitzgerald
* Pat Hickey
* Erik Rose
* Paul Osborne
* Dan Gohman
* (more people who I didn't get a chance to write down in time)

## Notes

* Erik Rose: we did some benchmarking to get the latest idea on overhead of enabling epochs vs nothing and it is about 14%. Starting to investigate using virtual memory for interruptions, this gives us an idea of our best-case throughput improvement.
