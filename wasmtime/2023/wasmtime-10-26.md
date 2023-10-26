# October 26 project call

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
    1. [Plugin system for the Wasmtime CLI](https://github.com/bytecodealliance/wasmtime/issues/7348) - acrichto
    1. _Submit a PR to add your item here_

## Notes

### Attendees

* fitzgen
* cfallin
* alex crichton
* roman volosatovs
* jamey sharp
* saul cabrera
* abrown
* rob wong
* till schneidereit
* bailey hayes

### Notes

* alex: CLI breaking changes
  * attempted to communicate this, but lots of people surprised
  * what to do? do we want to backout this change? re-do in an incremental way?
  * or just figure out what we could have done better in the future?
  * till: on one hand, we should have done an RFC and didn't do enough
    outreach. on other hand, some of the feedback was out of line and not really
    acceptable.
  * till: might make sense to do some sort of remediation in a patch release,
    like aliases
  * alex: we could add some aliases for flags or make them errors
  * fitzgen: use chris's original idea of falling back to the old CLI parser if
    the new one fails and having big warnings?
  * (discussion about CLI arg order and how to support the old thing and the new
    thing)
  * till or chris: can default to old CLI parser and fall back to new CLI
    parser
  * chris: add a config file for whether to use the new CLI parser or not?
    assume old if missing, then users can upgrade on their own time
  * alex: use env var; if set to old use old, if set to new, use new; if unset,
    emit warning, run both CLI arg parsers and figure stuff out or default to
    old; eventually default to new when unset; eventually delete old
  * chris: want to make stuff work by default, and since software is released,
    we can't make that software update to set this stuff
  * alex: should be able to support that via these env vars and choosing
    defaults when unset
  * (agreement that these changes should have been an RFC)
  * till: might make sense to do a retro blog post
  * chris: proposed constraints: never reuse / change semantics of a CLI flag;
    for deprecated flags, always accept them and ignore them rather than fail;
    for new behavior, always choose a new flag name
  * (agreement that we should guarantee N releases of deprecation before
    removing anything)
  * bailey: Warning and not erroring is still scary for CI automation (where
    folks might not be watching the logs) if we don't or can't migrate to the
    new option
  * till: we obviously should ensure that these warnings aren't the only channel
    by which we communicate these changes
  * bailey: should we make guarantees about not releasing on fridays and
    holidays
  * (agreement that we can just delay merging the release PR)
  * jamey: reiterating that purpose of these changes was to provide a foundation
    for more permanent stability
  * Agreed upon action items:
    * Make old parser work again and make a patch release (alex)
    * Communicate an update (till)
    * Retro blog post (till)
    * Make an RFC (till and alex)
      * Adding env var to choose parser thing
      * Enumerating the stability guarantees of the CLI going forward
* plugin system
  * agreement that this will need a very stable interface
  * agreement that we should continue discussion in the issue
