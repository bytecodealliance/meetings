# October 10 project call

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
    2. Discuss connect-four example from plumbers summit
    3. Recap of recent security issues (@alexcrichton)
    4. (last) zulip triage for docs

## Notes

### Attendees

* Nick Fitzgerald
* Alex Crichton
* Chris Fallin
* Linwei Shang
* Joel Dice
* Danny Macovei
* Till Schneidereit
* Kate Goldenring
* Dan Gohman
* Roman Volosatovs
* Johnnie Birch

### Notes

* Alex: recap of recent security releases
  * one issue related to a race condition in the type registry
  * another related to tail calls and stack traces
    * this one should have been caught by fuzzing, but we forgot to re-enabled
      the wasm feature in fuzzing after it was temporarily disabled
    * proposal: require bringing "I want to enable proposal X by default" to
      this meeting before that happens. forcing function for double checking all
      default-enabled requirements, such as fuzzing.
    * documentation in wasmtime guide for status of each feature, and what
      requirements are still missing before we can turn it on by
      default. initial PR landed, can be improved going forward.
    * also discussed this a bit yesterday in the cranelift meeting
    * Chris: can also inject bug and ensure that the fuzzer finds it
    * Till: why can't feature enabling/disabling be centralized?
    * Alex: feature flags in wasm-smith are different from wasmtime, but
      different fuzz targets have different feature support (eg winch doesn't
      support all features, differential engines might not support all features,
      etc)
    * (discussion about check lists)
    * Chris: can we formalize and write down what it takes to ensure that
      fuzzing is actually happening? i.e. the bug injection or checking fuzzing
      coverage
    * (discussion of fault injection and mutation-based testing to verify that
      fuzzing is exercising something)
* Kate: connect-four example for wasmtime docs
  * want "architectural" examples for how to use wasmtime
  * starting with plugins for a connect-four game, where each plugin plays the
    game
  * want meaty but still approachable
* (zulip triage; filing issues)
