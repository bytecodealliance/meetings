# March 30 project call

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
    1. Note: meeting notes linked in the invite.
    1. Please help add your name to the meeting notes.
    1. Please help take notes.
    1. Thanks!
1. Announcements
    1. N/A
1. Other agenda items
    1. N/A
    
## Notes

### Workflow: auto-assigning pull requests to reviewers in the wasmtime repository is live

- **Jamey**: We’ve turned on auto-assigning pull request to reviewers in the `wasmtime` repository
  - **Till**: Is the idea to encourage triage, etc?
  - **Jamey**: Yes, someone should ensure that the right reviewer gets ping, give contributors some kind of feedback promptly
  - **Jamey**: want to do something similar for issues but haven’t figured out what it will look like yet

### Shared WASI Implementations/relation to wasmtime

* **Till**: There have been some conversations in the past and @ Barcelona about a shared BCA implementation of upcoming WASI APIs – Microsoft folks discussing wasi-cloud core (http, key-value, etc) – things cloud hosters care about.
   * There’s only so much differentiation to be had, it’s an excellent thing to be shared.
   * Question is: where should that code live? Assuming it’s stuff that’s pulled into the linker
   * One possibility: fully downstream repo with cargo dependency on wasmtime
   * Another possibility (and likely a default):  have it live in the wasmtime repository, same as the other WASI implementations
      * Downsides to this approach:
         * WASI implementations are less stable than wasmtime itself
         * Eventually, all of it needs to meet the same standards of quality, supply chain security, etc – but all of that doesn’t need to happen right now
            * It’s similar to WASI on the standardization level – if it becomes part of the core project, does this mean core contributors need to grok all the APIs?
         * It brings many dependencies
   * Could be a feature flag as well/optionally compiled in.
   * Also, the stuff there is very useful/would be nice to have in the CLI
      * A lot of moving parts/it’s hard to say where things should fit
   * **Nick**: If you want integration with the CLI then it *has* to be in the crate – need path deps
   * **Chris**: the current WASI implementation is a submodule, maybe that approach works here
   * **Joel**: crates/wasi-common/wasi is indeed a submodule
   * **Nick**: having the CLI have the HTTP stuff built in would be nice. Seems fine to have it there, if the stuff is relegated to its own directory, and we can manage who reviews things with the CODEOWNERS stuff set up by Jamey.
      * **Nick**: Big issue is likely the supply chain stuff – do we want to disable the directory in cargo vet config. Needs review & audit otherwise.
   * **Till**: As long as it’s not on-by-default maybe it doesn’t need some of the heavier supply chain features
   * **Nick**: For example with wasi-sql, we’re not likely to bake in a specific engine – maybe the one that’s enabled in wasi-cli by default is a barebones implementation.
      * key-value is a simple case of this
   * **Steve**: This splits it in two – the WIT files are supposed to be broad and not wasmtime specific… Should there be somewhere in the BCA world where the interfaces are and the implementations should be somewhere separate.
   * **Nick**: it’s possible to depend on this *inside* wasmtime-cli but without including the implementation – the trait crate gets used by wasmtime-cli and integrations can adhere to those same traits/crates and other engines can reuse.
      * The traits have to exist either way, you need a trait for swapping out different implementations
      * If the traits are in the same repo then people can all code against the same trait
      * **Till**: want to think more about the boundaries/service of a wider ecosystem
      * **Till**: we care about another runtime, WAMR
      * **Till**: is this a service we want to offer everyone or have everyone provide themselves
      * **Till**: Microsoft has requested/would like the ability to provide a set of APIs and being able to plug implementations, possibly by using an RPC mechanism
         * Nick: Do you need RPC for this when you can do Dylibs for this? Can use the exported
   * **Joel**: Being able to keep the WIT files as sources of truth is advantageous, having a C binding option would be valuable along with the Dylib option
   * **Victor**: Kubernetes is a good thing to think about here
      * **Nick**: Trait crate has to live outside of tree in that world, and it could live in Cargo, and people could pull this from there and depend on that.
      * **Till**: this would make development very annoying, needing to rev the libraries 
         * **Nick**: You could have a non-committed patch locally to do it
         * **Till**: for deploying, you could have a new version of the API crate gets published, then implementations, then wasmtime gets published
         * **Nick**: Cycle would most likely not have to be tied to wasmtime release cycle.
* **Victor**: Is the trait crate needed?
   * **Nick**: there’s a bit of glue that is needed, and the stuff that actually powers the interface – and to share those, there needs to be a rust language mechanism
   * **Victor**: So plumbing individual arguments to calls
   * **Nick**: So more like sharing implementations, WIT would be the canonical source for the implementations
   * **Joel**: To riff, what can’t we achieve with a submodule that has WIT files and wasmtime when it’s ready would generate traits… Maybe there’s more to have wit-bindgen do (the one that’s living in wasmtime right now). Other projects use those WIT files and generate interfaces that they can use from their runtimes.
      * **Nick**: there’s no strict reason it *couldn’t* work. The benefit of the trait crate is to swap out implementations, you’d get slightly larger code size, but it’s not that big of a deal.
      * **Till**: ideally we would have as dependencies for implementation wit bindgen and WIT files, and then they generate host bindings and implement host bindings. Pairing that with the C-level API, and automatically generating a Rust API on the other-other end making it seamless. Point it to a WIT file and generate a sufficiently new version of wasmtime.
         * How exactly to do this is the question – it might be wit-bindgen
         * Bindings generation for host bindings could possibly live out of tree.
         * We don’t need to have a repository that contains the *specific* trait for all the APIs
      * **Joel**: Maybe you want to work on something that will never be standardized, would be nice to be able to work on that the same way the standardized stuff works.
         * This comes back to the CLI interface, do you have to have something that is dynamically loaded
      * **Nick**: The question is how do we fit this into the wasmtime CLI , people would normally have to make a custom embedding of wasmtime
         * **Joel**: There are certain people who would want to see the loading in of native code
         * **Joel**: I like the WIT oriented approach to this, keeps the door open to non-Rust runtimes, etc.
   * **Nick**: There are a lot of options here and no obvious solution, this is probably good for an RFC, so that stakeholders can give feedback.
   * **Till**: Taking a step back and summarizing, it sounds like there are 2 options
      * Having the implementation live inside wasmtime repository as a subproject if it is tied tightly to wasmtime
      * Having a mechanism for extending wasmtime with runtime-linkable implementations, and/or compile time linkable solutions.
         * **Nick**: hesitation around full dynamic load any interface at any time, but the north star of having all the WASI interfaces in the CLI makes a lot of sense.
         * **Nick**: Alex just went on vacation and he should definitely be in on this.
         * **Till**: When I said runtime linking I didn’t mean runtime linking of arbitrary interfaces – more wasmtime have knowledge of APIs and give concrete implementations. 
            * **Nick**: Makes sense at the wasmtime level, maybe not at the CLI level
            * Having wit-bindgen generate it so that you *can* totally makes sense
            * **Till**: detail would be hashed out as part of an RFC.
   * **Roman**: Wanted to mention in favor of implementation in wasmtime – wasi-http is not compatible with snapshot preview 1 adapter. If we did have all of this in wasmtime, it would be e2e tested and it would be impossible to get disconnected there.
      * **Nick**: the more things are integrated in one place, the more testing they get as well.
      * **Joel**: this is also an argument for having all the stuff in one repo (not necessarily the wasmtime repo)
   * **Victor**: RFC should have some stuff about all the options on how to integrate functionality @ the wasmtime level (guest implementation vs host implementation vs dylib vs code-level trait object) 
      * **Nick**: would be good in rationale and alternatives – if you say we should use trait crates, explain why. Could have wasi-kv and very different alternatives, dylib/etc vs trait object. Good place to enumerate options
* **Steve**: Who would write the RFC
   * **Nick**: This is something Till should likely take
   * **Till**: will take care of this/find the appropriate person to write

### Trampoline overhaul implementation progress

* **Nick**: reached a milestone of compilation, will turn into a big speed up for crossing wasm/host boundary
* **Nick**: most of the handwritten ASM trampolines will be gone, only the ones for lib calls but they’ll be gone in the future (not part of this PR)

### Exception handling

* **Till**: another question about the WASI proposal – any recent movement on exceptions/handling?
   * **Nick**: No, not really, but there are some plans to change some things about the exceptions proposal, gated somewhat by reference types
   * Exceptions on cg-cliff are being worked on by Bjarne (?)
   * **Johnnie**: What version is it currently at?
      * **Nick**: Exceptions proposal is @ stage 3
      * **Johnnie**: it’s in V8, people are using it breaking changes would be detrimental
      * **Nick**: The changes should be non-breaking/are being made in a non-breaking way.

### Attendees

### Notes

* Victor: send some documents on the in-tree/out-of-tree decisions history for k8s CLI
  * [Out-of-tree CSI Volume Plugins KEP #178 (2017)](https://github.com/kubernetes/enhancements/issues/178)
  * [Release blog explanation during v1.17 of out-of-tree migration (2019)](https://kubernetes.io/blog/2019/12/09/kubernetes-1-17-feature-csi-migration-beta/)
  * [Kubernetes 1.23 update on CSI out-of-tree migration status (2021)](https://kubernetes.io/blog/2021/12/10/storage-in-tree-to-csi-migration-status-update/)
