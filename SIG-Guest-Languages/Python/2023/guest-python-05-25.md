## Agenda
- Administrative stuff
    - Hosting agendas, notes, etc. at https://github.com/bytecodealliance/SIG-Dynamic-Languages
    - Shared Google calendar for SIG
- Update on action item from last week: `wasi-sdk` build-time detection
- Update on `componentize-py`
- Update on "dynamic" linking, `wasi-libc`, etc.
- Syncing with other tools: https://github.com/bytecodealliance/wasm-tools/wiki/Component-Model-Tooling-Compatibility
- Open discussion

## Notes

- Brett has new build: https://github.com/brettcannon/cpython-wasi-build/releases/tag/v3.12.0b1
    - built with wasi-sdk 20!
    - official builds!
    - Windows build: patches or PR welcome
- Plan to meet every two weeks
- Jamey: how can I help with dynamic linking?
    - Joel: making `wasi-libc.so` have all the symbols it needs
- micro-json might be a good simple native extension to test with early
- support maturin?

## Action Items

- Joel talk with SIG about demo with CI checking tool compat across languages
- Joel talk with SIG about staggering SIG meeting times for timezone friendliness
- Send invite for recurring meeting
- Post componentize-py and demo to WebAssembly discord
