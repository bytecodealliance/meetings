# August 01 project call

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

- Alex Crichton
- Andrew Brown
- Bailey Hayes
- Chris Fallin
- Nick Fitzgerald
- Taylor Thomas
- Victor Adossi

### Notes

#### WasmGC Update (Nick)
- Going well, most of work in cranelift
- Working on stackmaps ("user" stackmaps in particular)
- There is a wasmtime branch that uses it (passes all but a fuzzing test case), found some problems with stackmap involving loops and irreproducable control flow
  - You can allocate structs from the host, but not from WASM
  - Also add arrays from host (but not WASM)
  - After that, it's just working through the opcodes
#### Pulley Update (Nick)
- Pulley interpreter landed (https://github.com/bytecodealliance/rfcs/pull/35)
- Cranelift backend that emits pulley not landed yet
- Great place for folks to get into contributing if they'd like (after skeleton is in place)
  - There are unimplemented opcodes still (there are a bunch of somewhat isolated/non-involved/easy-to-implement opcodes)
  - Andrew: how many opcodes are implemented right now?
    - Nick: wasm opcodes or pulley opcodes?
    - everyone: ???? (we don't know)
    - Nick: closer to 10%, only enough to do basic control flow for proving things out
- Chris: you connected all the way to WASM right?
  - Nick: yes -- still some more work like handling traps or calls to host, but shouldn't be too bad
#### Shared Everything threads update (Andrew)
- Big push last month to get everything into wasm-tools
- Mostly there, big missing piece is the syntax for marking functions as "shared"
- Most places where you'd use a shared thing are doable
- It's a bit difficult -- spec is changing (as Binaryen changes)
- This quarter hopefully landing initial support in wasmtime
  - Not all instructions will be present (especially atomics)
  - Most important are the spawning bits
    - One unmerged branch is the CM addition for intrinsics for spawning
    - Getting that part working in wasmtime (expressing spawning threads & getting wasmtime to do it) is important
- TLS is still an issue
  - Chrome is changing their minds on what they want, possibly waiting for implementation to be tried in wasmtime
  - Aiming for simplicity
- Nick: most interested in how we're going to do TLS (Thread Local Storage), VMContext, state
  - So far no written down conclusions, we should probably nail down some of the decisions before submitting PRs
- Alex: One thing that is stable in the spec is the shared stuff
  - Worried more about the Rust API design sort of things, will have large ramifications on wasmtime
  - Nick: could kick the can down the road by only allow non-shared stuff
- Andrew: so the embedder API needs to be straightened out, I hear the same with the TLS and other stuff
  - want to do some experimentation and try some stuff, then write up and move forward
  - both on the embedder API and the TLS/VMContext bits
  - will try to document this so we can all understand it before moving forward
- Alex: seems like embedder API and VMContext
  - Would be willing to accept a partially complete Shared Everything Threads but is known to be memory unsafe? Personally I'd say no
    - Nick: same. Was hoping we'd be able to keep things safe by not giving new powers to the new host
      - (i.e. forcing host to still think of everything as single threaded, using atomics where necessary)
    - Alex: this means that can't run anything non-trivial (no shared host functions)
  - Alex: so maybe -- we'll never accept any un-memory-safe code, but we *could* take something under a feature flag?
  - Nick: tokio doesn't really work with TSAN
- Andrew: no great answer on testing/fuzzing yet
  - Alex: Joel might have thoughts on this
    - We probably could write a bunch of simple examples that are obviously correct and test those
- Chris: sounds like we want TSAN, but it wouldn't work for JIT'd code, but valgrind has a thing for races (lgrind?)
  - Andrew: how would you integrate that
  - Chris: there is some integration required (needs to know the ready primitive), but assuming you can add annotations, it can check races
    - helgrind (https://valgrind.org/docs/manual/hg-manual.html)
- Andrew: parallel research community has probably been working on this for a while
- Nick: https://crates.io/crates/loom might be helpful for host code
  - Could run the mini threaded programs under TSAN just for host coverage
- Alex: rust should help a lot here, just due to hwo the
#### New wasi:keyvalue store bringing in Redis (Nick)
- Did we audit this? Was it in-tree already?
- Taylor: Brought this up because we got new implementations (runtime config & keyvalue) but champions didn't know
  - Would be nice to be able to give context to the implementers beforehand
  - Other keyvalue things should be plugins rather than including them in the dependency tree
  - Alex: that sounds fine, and the saving grace is probably that everything is still marked @unstable
  - Nick: it would be good to have an in-memory table, add a trait + feature that forwards the calls
  - Taylor: Yep, we should remove the redis stuff, and then for any new implementations let's include the champions of the relevant interface
- Andrew: in wasi-nn we depend on crates that should not bring in large frameworks, but connect to external stuff via dlopen()... Are we changing that or still good with it?
  - Alex: I think it's fine on a per-proposal basis (avoiding the redis dep is kv specific)
  - Taylor: linking against something should be fine, not quite the pluggable case
#### WASI-NN update (Andrew)
- There is a desire to push wasi-nn to phase 3 and get it in a release
- The problematic stuff needs to be removed yet
  - Being able to pass around blobs of bytes that are models
  - In the future, we might just refer to models by name instead
  - This reduces the observability of the binary encoding of models, but those changes might make wasi-nn more palatable
- What do people think? any other things that make it hard to digest?
- Alex: Have some opinions, but haven't reviewed it very seriously just yet, low impact on the repo and vaguely people might prefer it being a plugin
- Nick: no strong opinions, not my area of expertise -- one thing that's annoying might be the way features are working (they're not purely additive, tests won't run with all features)
  - some handling of configs & feature combinations might be nice
  - CI needs special handling right now because of this, but that's a relatively minor issue
- Andrew: ideally everyone would be able to run cargo and run the tests, but users may not have stuff installed on their system
  - did some work recently on trying to refactor it, libtest-mimic (https://crates.io/crates/libtest-mimic) was helpful
- Nick: what about some sort of context builder?
  - Andrew: yes, you can pass in backends, could use that, but no way to control that from the command line
    - Nick: that's probably fine
- Bailey: would you mind doing a little presentation on the status to give people a chance to ask questions
  - Andrew: will try to create a presentation that explains the intent
  - Bailey: probably not the *only* new world, wasi runtime config also looks ready to go as well
- Andrew: how would users compose worlds together?
  - Bailey: `include` should work
  - user in this case being the embedding
#### Who updates wasmtime to use WASI 0.2.1? (Bailey)
- And roughly what is the process?
- Alex: I can do this, generally the process is:
  - Basically we update all the wits
  - Keep a copy of the older WITs
  - Percolate to the rest of the ecosystem via distribution of the adapters
- Bailey: How do we currently pull the WITs? Do we use wit-deps or anything similar?
  - Alex: right now we have a script that copies or uses the files, kind of like a wit-deps.sh
- Bailey: Would be nice if we could use the wkg tooling for that
  - Alex: once it's a bit more stable we would like to move to that
- Bailey: on also @since/@unstable, without @deprecated?
  - Alex: I'll get an example that might be able to work as-is
  - Bailey: if we're going to have it in the next release that would be fine
  - Alex: we should be able to use 0.2.1 adapters on current version without issues, since we're maintaining backwards compatibility
#### Two wasi-preview1-component-adapter repos? (vadossi)
- The *-provider one contains the bytes for the adapters to make things easier for Rust, but I thought that might go into the other one... what does the -provider suffix mean?
- Alex: so the -provider is the bytes of the adapters packed to make them easier to use from Rust
  - https://crates.io/crates/wasi-preview1-component-adapter will probably not be used, but it *would* have contained source code for the adapters
  - It could probably use a README
