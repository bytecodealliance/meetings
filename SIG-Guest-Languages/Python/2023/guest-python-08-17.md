## Agenda
- Status updates
- Planning for [WasmCon](https://events.linuxfoundation.org/wasmcon/) and [Componentize the World](https://www.eventbrite.com/e/bytecode-alliance-componentize-the-world-tickets-681895717447)
- Representing [resource types](https://github.com/WebAssembly/component-model/blob/main/design/mvp/WIT.md#item-resource) in Python
- Contributing `componentize-py` to the Bytecode Alliance
    - [Draft proposal](https://hackmd.io/Pc-nugxnThK3F1x7T1poKA)
- Anything else?

## Notes

- Brett: gave recap on Guest Languages SIG meeting
    - https://github.com/bytecodealliance/meetings/blob/main/SIG-Guest-Languages/2023/guest-langs-08-08-2023.md
- Joel: Aiming to tag state of `wasi-cli` and Wasmtime's implementation by end of month and align language tools (`componentize-py`, `jco`, `cargo-component`, etc.) to be compatible ahead of WasmCon
- Joel: See above proposal to contribute `componentize-py` to BA and leave comments and/or let me know if you want to be listed as a "Supporting Member"
- Joel: planning to generate bindings for resources using Python classes.  Should be straightforward.  Let me know if you want to collaborate on implementing it and/or if you have opinions on how to make it idiomatic.
- Brett: working on build bot doing CI testing on commits to main
- Brett: working to make it easier to install pure python wheels using pure python (benefit: potentially could run in Wasm)

## Action Items
- Joel: copy notes to https://github.com/bytecodealliance/meetings/blob/main/SIG-Guest-Languages/Python/2023
- Joel: submit proposal to make `componentize-py` a Bytecode Alliance project
