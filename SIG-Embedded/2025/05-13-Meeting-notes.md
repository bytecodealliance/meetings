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
    1. **Lime1 & WASI-SDK :** There is a proposal from the [WasmTime team to update the WASI-SDK]([meetings/wasmtime/2025/wasmtime-05-08.md at main · bytecodealliance/meetings](https://github.com/bytecodealliance/meetings/blob/main/wasmtime/2025/wasmtime-05-08.md)
    
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
    
    There are references to this issue on the WAMR repo:
    
    * [Lime1 · Issue #4209 · bytecodealliance/wasm-micro-runtime](https://github.com/bytecodealliance/wasm-micro-runtime/issues/4209)
    * [extended-const · Issue #4272 · bytecodealliance/wasm-micro-runtime](https://github.com/bytecodealliance/wasm-micro-runtime/issues/4272)
    
    Also discussed on the wask-sdk repo here:
    
    * [lime1 by yamt · Pull Request #527 · WebAssembly/wasi-sdk](https://github.com/WebAssembly/wasi-sdk/pull/527)
    
    It looks like Extended Const could be an issue for WAMR AOT: [extended-const/proposals/extended-const/Overview.md at main · WebAssembly/extended-const](https://github.com/WebAssembly/extended-const/blob/main/proposals/extended-const/Overview.md) 
    
    Suggestion from [Dan link that anyone not able to run lime1 should just build thier own libc](https://github.com/WebAssembly/wasi-sdk/pull/527#issuecomment-2847796171): 
    
    > > t's a valid solution. my feeling is it's app developers' choice which set of features it targets to though.
    >
    > Up to a point this is true, but we also what sensible defaults. Developers who want to target specs older than out defaults can/should be asked to rebuild libc themselves, right?
    
    2. **Outline a roadmap / to-do items for setting up LTS**
    
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

* Chris Woods
* Stephen Berard
* Ashish Shrikhande
* Luke Wagner
* Marcin Koln
* Max S.
* Michael Sanchez
* Thomas Trenner
* Emily Ruppel
* Kailasnath Maneparambil
* Rob Wooley

## Notes

### Lime1
 - There's a proposal to default WASI-SDK to target `Lime1`.  This could be an issue for runtimes such as WAMR which does not support all the features in `Lime1`, specifically, WAMR does not support "Extended Const".  There is an issue in GitHub requesting this (reference) but this is estimated to be a fairly large change for WAMR.
 - This is not the only time we're going to see this.  WebAssembly will continue to move forward and there will be new features/options and we'll want to take some of these over time.  How do we handle this??
 - Chris:  one way to would be to have the toolchain output a structured file with the enabled options.  This could be used to validate against a runtime.
 - Stephen:  We're likley going to need to have separate releases for WASI P1 LTS and the latest WASI version.  Not necessarily separate codebases, but at least separate builds.  Perhaps one way to address this is to have a set of options we include in LTS which is inline with the LTS supporting runtimes.
 - Stephen:  another option is that if "Extended Const" is the only thing not supported by existing runtimes (inc. WAMR). We could request that that specific feature be removed from Lime1
 - Thomas:  this is not just a WASI-LIBC thing.  These options are things that would be enabled in the compiler (and likely be the defaults).  Thus this impacts both the LibC library AND the compiler.  Marcin:  it should be possible to disable some/all of these options in Clang / LLVM.

### Action items for LTS
- Need to get clear on what exactly we mean by LTS.  What features are included.  What is supported.
- What is the duration of time for an LTS release?
- Is there a list of platforms/OSes/architectures that will be supported?  Likely we'll have to limit this in some way.  Stephen suggested having a tiered system where Tier 1 is fully supported, Tier 2 is best effort, and everything else is not officially supported.
- Things we need to put in place for LTS:
  - Need to make sure all code submissions are permissable.  Do we need a contributor agreement/license.  Is the DCO sufficient or do we need anything else?
  - Coding standards
  - CI/CD builds, testing, validation
  - CVE / Security process
  - Which Wasm runtimes are supported
  - Community best practices
  - Principles on addressing problems; something we can use to resolve conflicts/make decisions/priorities.
  - Documentation
  - Debugging and observability


## Action Items

* [ ] Chris / Stephen : Improve WASI-SDK Communication with E-SIG

  * [ ] Reach out to Dan, meetings for the SDK? Invite to come to e-sig?
  * [ ] How was Lime1 decided? visibility would help with planning?
  * [ ] Build out a stakeholder map for the WASI-SDK, including the upstream clang, llvm, and google contributions

* [X] Chris: Move the todo list items for the LTS into a google document :
  https://docs.google.com/document/d/1FYaR_pBfO4QlHLVqxw597JCqyp6AhZm7vGd9OsD6Ocs/edit?usp=sharing
