# November 09 project call

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
    1. How do we hand out commit bits for github.com/bytecodealliance/wasmtime? (Pat Hickey, 15 mins)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

### Notes

#### Commit bits & Wasmtime

* pchickey: What's the process for adding folks as contributors to the Wasmtime
  repo. Want to add a few people but doesn't seem to be a process. Do we want a
  process? Would want to add Dave Bakker due to his work on WASI sockets. He can
  review that code and adding to the reviewer pool is valuable. Certainly other
  folks that we could add too.
* fitzgen: Never had a process yeah, mostly when it's "obvious" we do it. Seems
  pretty obvious in this case. Other thoughts about more process?
* acrichto: I'd propose we bring it up in a meeting and see if anyone vetoes.
* pchickey: Sounds like not too much process but enough that it's not
  unilateral.
* fitzgen: Any triage you'd like to do? New reviewers for WASI stuff?
* pchickey: Roman you're in here! Would you like to become a reviewer?
* roman: Sure!
* pchickey: There's one more!
* tschneidereit: WASI bits a specific part of Wasmtime it seems. Would it make
  sense to use CODEOWNERS to differentiate reviewers?
* pchickey: Don't currently distinguish in owners.
* fitzgen: Would be nice to start, I don't know much about WASI but I get
  flagged for review.
* pchickey: Can move `crates/{wasi,wasi-http,wasi-nn,wasi-common}` and make a
  group for Wasmtime WASI reviewers. Let's add a group and do this.
* acrichto: Any objections to allowing self-nominations? None!
* tschneidereit/pchickey: Should also require that the contributor is a
  recognized contributor in the Bytecode Alliance.
* tschneidereit: should not be hard to clear this bar.
* pchickey: What's the process for that again?
* fitzgen: In the governance repo. Also documents about how to apply or
  nominate.
* pchickey: Good list of folks!
* tschneidereit: Probably missing some as well. Also as a call to action, if
  you're aware of someone who's done good work be it docs-to-code-or-whatever
  please nominate them.

#### CLI Plugins

* tschneidereit: Alex put plugins on the last agenda. I'd like to give an
  overview of the goals of this. Chris brought up questions about security and
  isolation. Dan had notes on this as well. My view at least is that we should
  have a plugin system optimized for performance and can layer anything else on
  top of them. Plugin remoted to its own process could be built on top of a
  plugin system that doesn't have that. Most important aspect is building a
  small core runtime and expand on that functionality via dynamically loaded
  plugins. Loading only if needed can be attractive. With the ability to layer
  seems like a good first step to prioritize configuration management complexity
* acrichto: agree! Regardless though I think this should go through an RFC.
* fitzgen: agree we should do an RFC to agree on which goals. Given the goals
  the decisions here are probably going to be more obvious. Not sure there's
  agreement on goals yet.
* tschneidereit: For example I don't think this should just apply to the CLI and
  it should also be easy for embeddings to make use of it.
* fitzgen: Generally very little in the CLI and not in the library itself yeah.

#### Misc

* fitzgen: wasm-gc implementation in wasm-tools almost done! Might work on how
  types are going to be changed in the embedding API first. Will need API
  changes to surface types across modules.
