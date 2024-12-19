# December 19 | Wasmtime Project Bi-Weekly

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
   1. (@dicej) Discuss CI testing strategy.  Currently, many tests run with all features enabled rather than just the default features, meaning what we're testing isn't necessarily what users are using.  The upcoming [component-model-async](https://github.com/bytecodealliance/wasmtime/pull/9582) feature will enable code paths which differ considerably from the default build, so we should discuss the implications of that.
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)

## Attendees

* Joel Dice
* Chris Fallin
* Alex Crichton
* Nick Fitzgerald
* Till Schneidereit
* Victor Adossi
* Pat Hickey
* Frank (Edgee)

## Notes

* Joel: CI strategy
  * we always test in CI with all features enabled
  * new async CM work introduces new feature that changes code paths when enabled, which means that we might not test all paths in CI anymore
  * should we rely on CI matrix to resolve this issue? and test w/ and without all features enabled? Till had suggested this offline
  * Nick: can the cargo feature just control whether the path is compiled in or not, and then we add a runtime `Config` knob to control which available path is actually taken? this lets us compile with all features but still choose any code path to test at runtime
  * Joel: perhaps, not actually too far from that already, so this may be possible
  * Till: aside from this particular knob, it seems like it would be good to test in CI the exact set of features that people tend to ship in prod
  * Alex: main complication is the combinatorial space, not just features but platforms, release vs debug, etc...
  * Till: maybe we can test just the features we build for release binaries?
  * Nick: many people run wasmtime without a compiler in prod, but we can't run tests without compiling wasm. this is overcome-able but a lot of work, and not clear we get much bang for the buck...
  * Chris: also niche CI configuration debugging has real costs
  * Till: can we add a smoke test for release artifacts in CI before uploading them?
  * Nick: could run a hello world
  * Joel: would these smoke tests have caught a real-world issue we’ve had in the past?
  * Nick: not aware of any
  * Chris: what are our goals here? who is the audience for making these we-test-exactly-this-configuration claims?
  * Till: intuition is that we will need to do this eventually to support compliance stuff for certain embedders
  * Alex: would be good to test the exact C dynamic library
  * (general agreement that running tests in release mode would be good)
* Till: core project application update
  * Bailey, who wasn't involved in writing the application, gave it a review with fresh eyes. Looks great, found minor nitpicks
  * TSC is recommending that the board approve the application
  * Will go to the board for final approval soon
  * Board doesn't meet until the new year, so no final answer until then
