# August 16 project call

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

* abrown
* avanhatt
* cfallin
* elliottt
* fitzgen

### Updates

* cfallin
  * Valgrind project has been merged into wasmtime
* elliottt
  * Looking into a text format for regalloc2 fuzz bugs so that we can use
    `creduce` to minimize complicated cases (thanks to fitzgen for this
    suggestion, it's much simpler than reworking the arbitrary impl!)
* fitzgen
  * Looks like we can continue using virtual memory, and can hold off on
    improving bounds checks for now
  * Reworking the pooling allocator could have implications on abrown's
    colorguard work
    * Switching from striping to using 16 allocators per pool seems like the
      right next approach
