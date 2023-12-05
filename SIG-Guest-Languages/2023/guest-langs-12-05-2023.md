## December 5, 2023 - SIG Guest Languages meeting

|          |      | 
| -------- | -------- |
| Attending  | Robin Brown, Mossaka, Joel Dice, Timmy Silesmo, Brett Cannon, Scott Waye
| Note Taker | Robin Brown

## Subgroup Updates

Include general updates, upcoming plans, and anything you want to raise to the rest of the group.

* C#/.net
    * Scott: Added [WasmImportLinkage](https://github.com/dotnet/runtime/issues/93824) for use in addition/instead of ddl import in [mono](https://github.com/dotnet/runtime/pull/94615) and [native aot](https://github.com/dotnet/runtimelab/pull/2444).
    * Scott: Microsoft announced exploratory work with C# and Wasm and WIT in a [blog post](https://devblogs.microsoft.com/dotnet/extending-web-assembly-to-the-cloud/) and published an experimental [WasmComponent.SDK](https://github.com/SteveSandersonMS/wasm-component-sdk).
* Go
    * Mossaka: No meetings this past month due to the holidays. We have a meeting today at the same time.
    * Mossaka: Resources are in for wit-bindgen/go as of today in [wit-bindgen 0.16.0](https://github.com/bytecodealliance/wit-bindgen/releases/tag/wit-bindgen-cli-0.16.0).
    * Mossaka: We're working on the fully go bindgen in [wasm-tools-go](https://github.com/ydnar/wasm-tools-go) (name not final) that will use the JSON IR.
    * Mossaka: The [wasm32 proposal has been accepted](https://github.com/golang/go/issues/63131#issuecomment-1806138218) by golang upstream. They only supported wasm 64 previously. They're working on Wasm exports directory.
* JavaScript
    * Guy: I spoke at NodeConf EU last month talking about [WebAssembly Components and JavaScript](https://www.nodeconf.eu/guy-bedford-webassembly-components-and-javascript). Node.js is the current focus for JCO work building out the preview 2 conformance implementation.
    * Guy: We're working for getting full test conformance against the Wasmtime test programs. If you want to publish components to NPM or treat components as normal dependencies, you can do so in Node.js. We're at 81/99 and are collaborating with Microsoft and expect to be done by end of year. We've been working with Yoshua Wyts & Wassim Chegham.
* Python
    * Joel: Componentize-Py has been updated to the november WASI release candidate.
    * Joel: I'm working on socket support in WASI-libc and I think we've got a pretty solid way forward adding a new target to [wasi libc](https://github.com/WebAssembly/wasi-libc/issues/447) (plus [shared library support](https://github.com/WebAssembly/wasi-sdk/pull/338)) and the [rust compiler](https://github.com/rust-lang/compiler-team/issues/694) which will be the way you target preview 2. This will run wasm-ld and wit-component to build a component natively. I'm working on adding sockets support to wasi libc, the rust standard library, and CPython. I've got a fork of tokio and other projects working idiomatically with wasi sockets. My goal this week is to get full python socket support.

## General Updates

* [Robin] - Claw
    * Robin: I'm working on a simple compile-to-Wasm programming language called [claw](https://github.com/esoterra/claw-lang)

## Agenda

* [Joel] - WASI SDK
    * Joel: We're working on a new version of it that has support for pseudo-dynamic linking which is a key feature in Componentize-Py and it's being investigated as a way to improve other languages. Ideally this can be used to factor out the SpiderMonkey runtime to avoid unecessary duplication.
    * Timmy: Any learnings from that? It's of interest to Mono runtime stuff
    * Joel: The important thing is that you can't load something truly dynamically during runtime to augment functionality. You do need to decide ahead of time what libraries you want to make available ahead of time to your application. For Python, that's important because many key Python packages depend on C, Fortran, or Rust native extensions and we can find those and bundle them into the component. Even if all you did was package the .NET runtime as a library. Ideally you'd be able to use this mechanism to implement any JNI-like .NET functionality.
* [Scott] - [.NET wit-bindgen issue](https://github.com/bytecodealliance/wit-bindgen/issues/777)
    * Scott - we're calling all the class constructors and as part of that it calls into wasi-libc which tries to initialize the environment and fails. When is the correct time to call wasi_libc_initialize_eagerly
    * Joel - there's a function you can call explicitly in your runtime to wasm calls ctors and not add this implicit call onto each of your exports. It'll see that you're not calling it explicitly and call it for you including your cabi realloc. If you don't want that to happen, because you know your realloc doesn't do all these call ctors stuff initializes, then you can add to either your actual exports or your main the call there.
    * Joel - We've been talking about whether there should be 
* [Brett] - wasm32-wasi as tier 2
    * Brett: I've been given the green light to ask for wasm32 wasi to become Tier 2 in January.
    * Brett: I'm no longer going to support the Emscripten target, so if nobody steps up it will end up phasing out.
    * Brett: We're talking with the py-script folks monthly.


## Action Items

None
