# September 16 project call

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

* Jimmy Brisson
* bjorn3
* Adam Bratchi-Kaye
* Nick Fitzgerald
* Yage Hu
* Johnnie Birch

### Notes

* Jimmy
  * got Sightglass running on mainframe
  * perf looks to be similar to other architectures from a very high level
  * a few upstream modifications to make it work: `perf_event` and `rust_precision`
* Adam: nothing
* bjorn3: nothing
* Yage: nothing
* Johnnie: nothing
* fitzgen:
  * continuing work on new register allocator
  * ironing out fuzz bugs
  * adding it as a (local/temporary) option to `regalloc2::Algorithm` to start getting compile time and code quality measurements from Cranelift and Wasmtime
