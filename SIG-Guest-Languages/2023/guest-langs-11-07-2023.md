## November 14, 2023 - SIG Guest Languages meeting

|          |      | 
| -------- | -------- |
| Attending  | Robin Brown, Joel Dice, L Pereira, Timmy Silesmo, Brett Cannon
| Note Taker | Robin Brown

## Subgroup Updates

Include general updates, upcoming plans, and anything you want to raise to the rest of the group.

* C#/.net
    * Timmy: The C# group is growing and is outlining plans and goals. The initial plan is to support both Mono and LLVM AOT compilation for C# primarily. We've started some changes to wit-bindgen where we're going to host the C# bindings generator and will be landing the [first official change in dotnet](https://github.com/dotnet/runtime/issues/93824) to add support for imports to link things correctly which is previously challenging for native/aot.
    * Timmy: We've outlined things we're working on in a [wit-bindgen issue](https://github.com/bytecodealliance/wit-bindgen/issues/713).
* Go
    * Joel: Joe at Microsoft added a PR to start adding resource support for the Go bindings generator and is working to add runtime tests for resources to each language (C and Rust done, Go WIP).
* JavaScript
    * Joel: I've been helping Guy with componentize-js resource support. It's mostly implemented and I'm fine-tuning it. There's some nuance to when to drop handles. There's a `using` keyword we're looking at integrating as a way to drop on end of the using scope.
* Python
    * Brett: I'm setting up a build bot which is a way that C Python does CI for other cpus/os'es. This will allow us to keep the level of support that we have.
    * Brett: There is an internal team interested in Python in the browser who may participate.
    * Brett: I'm continuing to improve the dev experience to bring Python up to Tier 2.
    * Joel: I made a big update to componentize-py a couple weeks ago to pull in a snapshot of preview 2. We also now have version support for interfaces.

## General Updates

* Joel: I'm working on getting WASI-Virt and wasm-tools compose working with resources.

## Agenda

* [Robin] - Registry
    * Protocol with multiple storage backends
    * Have established semver system for WASI Previews
    * Registry protocol is also versioned -- has now reached 0.2 
    * Danny M. working on reference impl
    * BA preview instance is up and usable
    * protocol stabilized for lifetime of WASI Preview 2
    * alternative to static inclusion
    * "more ready for Preview 2 than Preview 2 is"
    * Once Preview 2 ships, it will be ready
    * Peter H has created "wit" tool in cargo-component repo for managing wit files via registry
    * Can we publish via github action?
    * Could maybe use git tags to identify versions and publish them.  Need to determine what that looks like.
    * Pull vs. push?  Need to decide.
    * Brett: using OpenIDC for auth on PyPI
    * Robin: have considered that, but Ralph has concerns
    * Signing could happen locally or via some cloud service
    * Registry could sign artifact itself if it trusts origin
    * Priorities: CI publishing, pulling artifacts locally and CI
    * "building" WITs into component binary is easy and well-understood
    * building components from arbitrary language has more variety
    * Key management UX is open question
    * Meets every Wednesday
* [Brett] - Exceptions
    * Brett: Curious about the status of asyncify set-jump long-jump. Does anyone know what's going on?
    * Joel: There are two relevant proposals exceptions and stack-switching but I don't know the status of either
    * Robin: T
* [Brett] - WASI libc compatibility
    * Brett: I'm concerned about the lack of compatibility gaurantees in wasi-libc. Since wasi-libc isn't stable, you'd have to compile a different version per python version.
    * Robin: I think Dan doesn't want to commit to stabilization because there aren't enough resources working on wasi-libc to bring to a stable place and support it and stability guarantees.
    * Brett: What amount of resources do you think he'd need? One person, two people, etc.?
    * Joel: Guessing a little, about what Dan would want. Long term sustainability probably more important than short term burst of resources.

## Action Items
