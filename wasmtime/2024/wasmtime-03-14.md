# March 14 project call

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
    1. WASI versioning, drafts, and `wasmtime::Linker` (Till, Alex, Pat, etc...)
    2. Wasmtime Debugging RFC: https://github.com/bytecodealliance/rfcs/pull/34
    1. _Submit a PR to add your item here_

### Attendees

* abrown
* alexcrichton
* cfallin
* David Justice
* elliottt
* fitzgen
* jameysharp
* jlbirch
* rvolosatovs
* Siddharth Khonde

## Notes

* delaying WASI versioning discussion, as folks are missing
* debugging RFC
* alexcrichton: there are some academics fuzzing wasmtime
  * fitzgen: they're doing differential execution across architectures
* testing discussion
  * alexcrichton: reworking wasm disassembly tests to not exist in the cranelift
    filetests suite, and instead be run through wasmtime. this allows us to test
    any compiler, not just cranelift
* cfallin: is there a reason that we don't use a guard page at the end of the
  wasm stack with stack probes instead of explicit stack checks?
  * alexcrichton: yes, we call wasm with the host stack and having the host
    stack raise an unrecoverable error would be bad
  * cfallin: maybe there's a mid-point here where we can avoid stack checks in
    leaf functions, and check a bit extra everywhere else?
  * alexcrichton: that makes sense. we haven't spent a lot of effort on
    optimizing this stuff in the past, and it would make sense to do that.
  * jameysharp: do we have leaf functions if we're using the epoch handler?
  * cfallin: good point, no
  * fitzgen: we could pin a register for the stack limit to avoid loads from the
    vmctx
  * alexcrichton: yes, but then we'd be burning three registers (caller and
    callee vmctx being the other two)
  * jameysharp: what if we change the wasm abi to only have one vmctx, but
    modify the native abi to take both?
  * cfallin: we would have to generate trampolines for each instantiation, as it
    would have to bake in the caller's vmctx
  * fitzgen: what about trampolines between wasm instances?
  * alexcrichton: that would also enable us to have a pinned register, which we
    haven't been able to do before
  * fitzgen: trampolines between instances would give us a good place to update
    shared data in the vmctx, which would mean that we could inline all the
    runtime limits and avoid a chained load to test things like the stack limit,
    or check epochs
* cfallin: what if `wasm->host` does the stack limit checking, and we otherwise
  avoid limit checks in wasm?
  * alexcrichton: this would work, but we'd need to register a signal stack
    through `sigaltstack`
* fitzgen: if we're discussing changing the wasm abi, it would be really great
  if we could standardize on the tail calling convention. we need to sort out
  performance here by adding callee-saves
  * jameysharp, cfallin, and elliottt, volunteered to do this work
* cfallin: after benchmarking interpreters again, seeing a 4x aarch64 slowdown.
  removing the `csdb` instruction from the interpreter's table switch improves
  this, and it's unclear if we still need it for spectre safety.
  * alexcrichton, fitzgen: we could disable it for the cli, but leave it on by
    default for the embedder abi?
* jameysharp: happy to answer additional questions about the cranelift table
  work
  * alexcrichton: we could move constant table allocations into the vmctx, which
    would simplify calling through the table
