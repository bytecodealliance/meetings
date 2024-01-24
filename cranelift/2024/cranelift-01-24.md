# January 24 project call

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
* alexcrichton
* elliottt
* fitzgen
* saulecabrera
* uweigand

### Updates

* fitzgen
  * wasm-gc support in wasm-smith
* saulecabrera
  * lots of fuzzing for winch multivalue
  * focusing on loads and stores
  * got winch working with `wasmtime explore`
* elliottt
  * added support for stack overflow checking in winch
  * working on supporting unwind info in winch
* alexcrichton
  * lldb is gaining support for wasm, maybe we could leverage this to get
    debugging support quicker
  * new constant-time wasm paper <https://arxiv.org/abs/2311.14246>

## Discussion

* abrown: testing mpk in ci?
  * could we have a server that would only run the mpk jobs?
    * yes, but who would own the server?
  * could we run a custom kernel on qemu in system mode?
    * yes, but it seems a bit more fragile than a custom server
    * if ci breaks, who would be responsible for fixing it?
  * alexcrichton: for rust, we would spin up a vm locally, and then run a server
    that would listen for commands on a tcp socket. a custom runner was used
    then to send commands to the runner when something needed to be tested
  * abrown: the third option is to wait for a machine to show up that has mpk
    support, and then turn it on when testing there
    * we would catch bugs, but not quickly, and unrelated prs would likely be
      where the failures would show up
  * abrown: let's do a trial run of option 3, and if it's bad we can investigate
    option 2
