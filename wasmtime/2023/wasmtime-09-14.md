# September 14 project call

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
    1. More about wmemcheck (acrichto)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

* fitzgen
* Till Schneidereit
* Chris Fallin
* Saul Cabrera
* iximeow
* Alex Crichton
* rwong
* Jamey Sharp
* Yordis Prieto
* jlbirch
* Dan Gohman
* Andrew Brown

### Notes

* wmemcheck
  * Alex: do we plan on maintaining this long term? if so, I think we will want
    more end-to-end unit tests. also just looking for more information.
  * Chris: yes, we want to maintain it. written by an intern initially. needs
    polish, but we have C guest code that wants tooling like this.
  * Alex: just don't want a situation like with our caching mechanism. just want
    end-to-end tests to avoid that.
  * Ixi: a handful of things we need to file follow up issues for too.
  * Chris: for example, we hook malloc but not calloc
  * Alex: would be cool to start with a C program that has a use-after-free or
    something and then compile that and run it and have wmemcheck report the
    error.
  * Ixi: we should talk about this feature more widely once it is more polished
* Till: policy for backporting features and bugs (non security) to stable
  branches when there is interest in that?
  * wasi preview 2 bug fixes we would like to have in 13
  * fitzgen: no problem backporting bug fixes when there is demand. bigger
    features require more care, since they are more likely to introduce security
    bugs
  * Chris: if things cherry pick cleanly that is super easy. if not, and
    requires and intermediate development point, then we should tread more
    carefully
  * Alex: I like that as a litmus test
* Andrew: going to try and start a wasi-nn SIG in the BA or a WASI sub-sub-group
  * let me know if you are interested in joining
  * or have opinions on which it should be
  * fitzgen: if it is about standards, it should fall under the W3C umbrella. if
    it is about implementation, it should be a BA thing
  * Till: SIG-machine-learning may make sense under the BA
* Andrew: legacy wasi-threads and wasi-libc
  * think they want a new target for legacy wasi-threads, does that make sense?
  * Dan: yeah that's probably the best approach
* Jamey: fyi for PR auto assignment round robin, I can't deal with code review
  right now, assigned myself as busy, so I won't be auto assigned. will be back
  soon.
* Alex: CLI changes just landed on main, will be part of 14
