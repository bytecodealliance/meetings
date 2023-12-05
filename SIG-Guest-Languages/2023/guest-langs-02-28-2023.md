## Feb 28, 2023

|          |      | 
| -------- | -------- |
| Attending  | Robin Brown, Peter Huene, Luke Wagner, Calvin Prewitt, Roman Volosatovs, Kevin Smith, Daniel Macovei, Pete Vetere
| Note Taker |

* [Robin] - Overview
    * Context: [original sketch](https://hackmd.io/soZ3n7fmTP2sEzK1_Y7MCw)
* [Robin] - Introductions
    * Bailey - WasmCloud integration with JS work by Guy. TSC member, employed by Cosmonic.
    * Calvin - JAF labs, focused on dev experience and leveraging the browser/JS
    * Dan Gohman - Fastly, interested in what happens here but mostly focused on WASI
    * Danny - JAF labs, looking into JCO / componentize JS
    * Guy Bedford - JS developer, working on Wasm Components. JS work is aiming to support components to be written in dynamic languages (componentize JS) and help it grow to more users.
    * Joel Dice - Fermyon, working on Spin (serverless Wasm apps) and supporting more languages (Python SDK today, JS already out).
    * Kevin Smith - SingleStore, working on Python in Wasm and trying to figure out how that can work (e.g. dynamic loading? capabilities).
    * Luke Wagner - Fastly, working on component model here to collect feedback
    * Peter Huene - Fastly, working on cargo-component, interested in fs virualization
    * Pete Vetere - SingleStore, embedded Wasm in the database, currently working towards Stored Procedures. Interested in what people are doing for interpreted languages.
    * Roman - Cosmonic, working on WasmCloud.
    * Saul Cabrera - Working on baseline compiler for wasmtime. Working on Javy with VMware, suborbital, etc. Getting started on Ruby
    * Till - Fermyon, TSC member. Same interests as Joel, want to run as many languages on Spin and in the component model. Wrote original JS with spidermonkey guest.
    * Sam - SingleStore, looking into how to make Wasm Extensions in the database work well.
* [Robin] - Projects
    * JS
        * BA - [Componentize-JS](https://github.com/bytecodealliance/componentize-js)
            * Embeds SpiderMonkey
            * Embeds JS Source
            * Uses splicing/wizering
        * JAF Labs - Experiments with SpiderMonkey and QuickJS and AOT
        * SingleStore - Experiments
            * Using QuickJS
            * Experimenting with serialization & dynamic approach
            * Experimenting with codegen and static exports
            * Using WASI VFS
        * Shopify - [Javy](https://github.com/Shopify/javy)
            * Uses wizer to pre-initialize
            * Targeting low component sizes
            * Module only contains bytecode
            * Partnering with Igalia
        * Fermyon - [Spin JS SDK](https://github.com/fermyon/spin-js-sdk)
            * Javy-based
            * Uses same general technique
            * Added Web APIs and fs functionality (uses rust-to-js bridge)
            * Uses Canonical ABI
            * Transitional? may switch to componentize-js
            * Less size-sensitive, more speed-sensitive
    * Python
        * SingleStore - Experiments
            * Kevin created Python WASI
            * Trying to get C extensions into Wasm file
            * Hoping to attend PyCon to figure out general plan
            * Able to link things statically, but not ergonomically
        * Cosmonic - Private SDK
            * Using wasi-nn
            * Interested in Python
            * Private SDK work
        * Fermyon - [Python SDK](https://github.com/fermyon/spin-python-sdk)
            * Javy-style
            * PyO3 based
            * Builds into module
            * Bundle application and dependencies and pre-init
        * JAF - Experimental
            * Talking with Anaconda folks about Pyscript
    * Other languages?
        * Ruby
        * Lua
        * php (vmware)
        * R
