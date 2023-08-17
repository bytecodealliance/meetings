# August 17 project call

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
    1. `wasmtime run` CLI and argument parsing (bytecodealliance/wasmtime#6737)
    1. Heads up regarding pooling allocator refactor (bytecodealliance/wasmtime#6835)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

* Nick Fitzgerald
* Andrew Brown
* Bailey Hayes
* Chris Fallin
* Dan Gohman
* Dominique Saulet
* iximeow
* Jamey Sharp
* Johnnie Birch
* Roman Volosatovs
* rwong
* Saul Cabrera
* Trevor Elliot
* Zeeshan Lakhani

### Notes

* (cli agenda item delayed till next meeting)

* nick: heads up on pooling allocator refactor
  * config methods will change
  * upgrading tutorial in RELEASES.md
  * will be in 13.0 release

* andrew: thinking about bringing wasi-nn up to speed with WIT
  * named models and `load-by-name`
  * simple hash map in current implementation
  * but have a trait for customizing in different embedders
  * first question: how to integrate with wasmtime CLI?
  * can we have a CLI flag for preloading models?
    * no one has objections
  * right now, we test with a bash script
  * would like to use `cargo test` instead
  * problem is you need openvino on your system to run the test
  * how to skip tests on regular contributors' systems that don't have openvino?
  * env var
  * want to be able to force on CI tho, so we don't accidentally always skip these tests
  * bailey: how did WIT migration go?
    * seems like it is going well
    * waiting on some resources work

* chris: Wasm memcheck
  * `--wmemcheck` CLI flag and cargo feature `--features wmemcheck`
  * requires wasm export certain symbols
  * lots of TODOs but basic thing is there and works
  * there is documentation under `docs/wmemcheck.md`
