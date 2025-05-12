# May 13 SIG-Embedded Meeting
## US and Europe Timzone meeting
** 11am US East Coast, 10am US Central, 8am US Pacific, 5pm Central European Time, 11pm China **

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call

1. Announce that the meeting will be recorded and hit record as agreed upon in inaugural meeting. 

1. Announcements
    1. _Submit a PR to add your announcement here_
    
1. Other agenda items
    1. There is a proposal from the [WasmTime team to update the WASI-SDK]([meetings/wasmtime/2025/wasmtime-05-08.md at main · bytecodealliance/meetings](https://github.com/bytecodealliance/meetings/blob/main/wasmtime/2025/wasmtime-05-08.md)
    
    > Dan: Does anyone have opinions about wasi-libc and Lime1?
    >
    > Dan: In particular, do we want to have separate libc.a files to support different subtargets, or would it be ok to just support Lime1? The issue is that some engines don't yet support some of the things in Lime1, such as extended-const and call-indirect-overlongs. Is it worth keeping these disabled in libc.a or making a separate libc.a build without them?
    >
    > A discussion followed, and I was active in the discussion, so I didn't take real-time notes. Here's a summary:
    >
    > Lime1 was defined slightly optimistically; we gave it features which aren't completely universal yet, but which we believed shouldn't be a major burden for any implementation. For example, extended-const just adds 6 simple integer operations to implement for static initialization. And in return, it enables much better dynamic libraries. So it seems best to just depend on it, and oblige engines to add this feature, rather than holding everyone else back.
    >
    > So it seems to make sense to have wasi-libc move to just building its libc.a for the Lime1 target, and not add a new dimension of libc.a builds.
    
    [Details on Lime1]([tool-conventions/Lime.md at main · WebAssembly/tool-conventions](https://github.com/WebAssembly/tool-conventions/blob/main/Lime.md)) (highlights below)
    
    >The Lime1 target consists of [WebAssembly 1.0] plus the following standardized ([phase-5]) features:
    >* Mutable Globals
    >* Multivalue
    >* sign-ext
    >* nontrapping-fptoint
    >* bulk-memory-opt
    >* extended-const
    >* call indirect overlong
    
    This appears to mean that the WASI-SDK would drop support (again?) for already deployed embedded runtimes?! - Is this going to be a significant impact for WAMR users? Was this discussed at the WAMR TSC meeting? Would it make sense of the E-SIG to fork the WASI -SDK to provide stability for existing runtimes, and provide a legacy SDK ?
    
    There is a reference to this issue on the WAMR repo:
    [Lime1 · Issue #4209 · bytecodealliance/wasm-micro-runtime](https://github.com/bytecodealliance/wasm-micro-runtime/issues/4209)
    
    2. Outline a roadmap / to-do items for setting up LTS
    
       1. Definition of LTS?
    
       2. Processes / Community Practices which need to be established to achieve LTS
          e.g. 
    
          1. CI/CD processes / resources required
    
             1. Qemu discussion for WAMR  - for target specific validation
             2. Test coverage of WASI-SDK?
             3. Validating WASI-SDK output works with common set of WAMR configurations?
    
          2. Contributor License Agreement
             Don't need to write it, but need to define it's purpose, if we need one - then ask for legal support to draft:
    
             1. Patent agreements (making sure there are none)
             2. Copyright agreements, etc.
    
             
    
    1. _Submit a PR to add your item here_

## Attendees

* TODO

## Notes

* TODO

## Action Items

* [ ] TODO
