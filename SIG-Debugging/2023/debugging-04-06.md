# April 06 SIG-Debugging Meeting

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. Thays presenting the .NET and WASI debugging support
    1. _Submit a PR to add your item here_

## Attendees
* Thays on debugging for Mono/WASI
  * different versions for WASI and browser-based WASM
  * Ralph: experimental .NET WASI support separate from browser stuff
  * will focus on WASI today
  * TCP-based protocol already worked; needed to make it work on Wasmtime
  * WASI socket support primitive, so took some effort to make that work
  * Mono is an interpreter on WASI, so breakpoints pause interpreter, not Wasm
  * using proprietary Mono-specific protocol
  * VSCode extension for handling that protocol: https://marketplace.visualstudio.com/items?itemName=ms-vscode.mono-debug
    * not sure if it uses DAP at any level; EDIT: yes, it does
    * extension is open source: https://github.com/microsoft/vscode-mono-debug/
  * https://cloudnativewasmdayna22.sched.com/event/1AUDS/cl-lightning-talk-implementing-wasm-debugging-with-net-in-vscode-thays-tagliaferri-de-grazia-microsoft
  * lightning talk: https://www.youtube.com/watch?v=OJjv3_PY8lE&list=PLj6h78yzYM2PzLhPvZIihwPShNuXP01C5&index=8
  * another challenge: no multithreading (at the time) in wasmtime, so had to make single thread multitask
    * Nick mentioned new experimental wasi-thread support, but still buggy
  * Nick: we're considering using DAP; have also looked at GDB's protocol
    * focusing on guest debugging for now
    * existing support allows host+guest debugging, but buggy
    * Rust or C/C++ can rely on DWARF
    * for interpreted languages, need to collaborate with interpreter
    * could there be a standard way to let module handle source translation
  * Thays: probably wouldn't adopt that since MS already has something working
  * Ralph: really likes idea, though, speaking for his side of the company, and would work to promote it
  * Nick: anything you wish the runtime provided that would make it easier (e.g. hardware watchpoints?)
    * Thays: no, but better wasm-level debugging would help debug Mono runtime
       * have worked around by disabling assertions
    * Ralph: might have resource to help with that
    * Nick: PR disables assertion, but there's a deeper bug that needs fixing, e.g. in cranelift
    * Ralph: can provide help, but would need to ramp up
    * Nick: needs a rewrite anyway; original engineer no longer working on project
    * https://github.com/bytecodealliance/wasmtime/issues/5537
    * https://github.com/bytecodealliance/wasmtime/pull/5553
    * https://github.com/bytecodealliance/wasmtime/issues/3999
    * Ralph: will talk to MS Chrome devs with wasm debugging experience to see if they have bandwidth and experise
  * Nick: does Mono debugging support watchpoints
    * Thays: no, but would like to support that
    * Would need API to support copying collectors for watchpoints
  * Nick: Fastly devoting some resources to experimenting with e.g. DAP

## Notes

* TODO

## Action Items

* [ ] TODO
