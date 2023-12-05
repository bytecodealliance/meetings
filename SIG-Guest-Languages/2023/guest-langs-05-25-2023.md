## May 25, 2023


|          |      | 
| -------- | -------- |
| Attending  | Robin Brown, Joel Dice, Calvin Prewitt, Danny Macovei, Bailey Hayes, Kevin Smith, Saúl Cabrera 
| Note Taker | Robin Brown

* Updates
    * Calvin: There's a link to join a google group for JS subgroup [link](https://groups.google.com/g/ba-sig-dynamic-languages-javascript)
    * Joel: We're meeting later today. Last week I worked with Dan Gohman to build WASI-SDK and wasi-libc to create a shared library to support things like Python native extensions and am working on making wit-component build components. `wit-component` will need a refactor but we're implementing it in a short-term way first. `Componentize-Py` is available on PyPI. There's a demo with `wasmtime-py` and `Componentize-Py` to run Python inside Python to run untrusted Python code with isolation, time/resource limits/etc. [demo](https://github.com/dicej/component-sandbox-demo#examples).
* [Robin] - Subgroup Proposal Vote
    * Vote passed
* [Robin] - Chair Selection Process
    * Selected Robin Brown and Guy Bedford as Co-Chairs
* [Saúl] - Javy
    * Saúl: Javy is now a Bytecode Alliance project. It uses a different runtime than existing tools.
    * Guy: Do you have a roadmap for Javy?
    * Saúl: Componentizing is the next big step. It might make sense to have everything in one toolchain so that users can choose which option they want to affect binary size. It might also be interesting to look at how ahead of time compilation can be added into Componentize-JS.
    * Guy: I've been working on refactoring componentize to simplify and crystalize the minimal things that need to be done here. In a way the codebase/techniques are becoming trivial. I'm looking to share those experiences with other guest languages and make it easier to integrate and maintain integrations.
    * Joel: We were also talking about how to avoid alienating Python people when everything changes. We might be able to sync up with Wasmtime releases to have an "edition" for each version that you know will work together. It'd be good to see something like that.
    * Guy: On that topic, I did set up a [wiki](https://github.com/bytecodealliance/wasm-tools/wiki/Component-Model-Tooling-Compatibility) on the `wasm-tools` to set up a live version matrix. Whenever there is a major version change to the component model or WASI we can make a new entry. Otherwise we update the existing ones so you know the best previous snapshots. It's grouped by tooling, guests, and producers. That's kind of a distinction between Rust which has low-level bindgen and then there are end-to-end tools.
    * Guy: Having shared tests is a big way to help onboard new languages. In componentize-js we have some worlds and make components against that.
* Action Items:
    * Robin: Propose rename to SIG-guest-languages
    * Robin: SIG-Dynamic-Languages repo and in the charter
    * Robin: Update calendar invite to show the agenda link
    * Robin: Updating the meeting notes
    * Robin: Add show-and-tell / deep dive section to meeting template
