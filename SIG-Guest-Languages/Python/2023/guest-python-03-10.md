## Agenda
- Introductions and interests
- What should we work on?
    - Proposal: Python equivalent to [componentize-js](https://github.com/bytecodealliance/componentize-js), including:
        - Runtime (e.g. CPython + wasi-sdk + component adapter)
        - Binding generator (similar to [js-component-bindgen](https://github.com/bytecodealliance/jco/tree/main/crates/js-component-bindgen))
        - Shim tool (e.g. reuse, modify, or parallel [spidermonkey-embedding-splicer](https://github.com/bytecodealliance/componentize-js/tree/main/crates/spidermonkey-embedding-splicer))
    - Anything else?
- Divy up the work

## Notes
Joel: Fermyon, wants to help make BA Python->Component tools
Bret: VS Code, Python core developer for 20 years, steering council, in charge of WA in python
Calvin: want a native dev env in browser for CM
Kevin: SingleStore; python guy there; UDFs, UDAs, want python, lua, ruby, etc. after C and Rust.  Working on third party C python extensions, e.g. numpy, pandas.  Struggling with circular imports.  Compiling and linking works.
Jamey: WA team at Fastly.  Busy focusing on JS, Go, and Rust right now, but curious about Python work.

Do we want both dynamic and static bindings?  CM wants you to declare interface statically, but you can build a dynamic interface on top of that if you want.  Writing UDFs in Jupyter?  Could use canonical ABI type serialization if you want, but that's no better or worse than others, since the host won't be aware of it.

Static vs. dynamic linking extension modules?  No official story in Python-WASI land yet for dynamic linking.  Everyone's currently using "built-in", statically linked module.  Dream is the CM allows equivalent to dynamic library linking.  Would like to make each dep a separate component which can be linked with app.  Each release of Python changes interface; would need component for each "wheel".

Will shared-nothing CM mesh with extension modules.  Python already moving toward shared-nothing model, but it's opt-in for now.  Theoretically, it's doable.  Not everyone has opted in yet, though.  Maybe need a proxy C API for CPython?  Can we model it as a world?

Currently very simple dlopen-based, call-a-function model.  Data science world is very old-school and messy (cf Fortran).

Pandas has like 40 C extensions, Numpy has a bunch.  What about static name collisions?  Fortran name restrictions are small, so collisions more likely.  Have to edit source to change names.  Less of a problem for dynamic linking.

Calling between components has a cost (allocation, copying).  Might not want extensions separate from interpreter.  Whole point of C/Fortran extensions is performance, after all.

Can we ship wasm-ld as part of the componentize tool and let them choose which pre-compiled extensions they want to pull in?

Any options for AOT'ing Python to Wasm?  Can do "freezing", AKA embedding bytecode.  Bret giving talk about minimum viable Python.  Fastly is experimenting with Wizer-based interpreter loop unwrapping to make a compiler out of it.  MicroPython is restricted.

Standard library: 200+MB module sizes suck.  Wizerizing can help, but is dangerous because you might not touch everything you need at init time.  Can freeze just bytecode, not source, which might cut size in half?

Are there ways to feature flag std lib?  Not really -- that's not how it's designed, and there are a lot of interdependencies.  There's a module-finder tool to generate an import DAG.  Import search paths are dynamic, so static analysis is tough.  "It's an exec call".  Django apps do a lot of dynamic imports e.g. by strings.

!PEP11 describes tiers. Want to make WASI Tier 2 (it's tier 3 now).  Want to differentiate vs. emscripten, which has different goals.  Would like WASI download on python.org for 3.12 or 3.13.

Toolchain compatibility needs to be considered, but new minor releases can move to new toolchains.  Might want to stick with given wasi-sdk version for given Python minor release.

Usecases Python community are interested in: browser execution, expecially education (new python users wanting to play with it without installing anything).  E.g. Pyodide and Jupyter lite.  Does Wasm allow more platform-agnostic distribution?  Need a WASI runtime installed, though.  Could solve single binary cli app story?

Is there a standard location for files for WASI apps?  Doesn't matter much since we can virtualize, i.e. WASI doesn't care, so just pick one?  But tools like IDEs might care, so follow what they want?  Dot folder?

Maybe we'll create a BA SIG for this at some point.

Chat:
10:01
BC
Brett Cannon
Brett Cannon says:I can also give an update on where WebAssembly in CPython stands
10:11
avatar
Calvin Prewitt
Calvin Prewitt says:Anyone experiment with
https://micropython.org/
 ?
10:12
BC
Brett Cannon
Brett Cannon says:Not personally, but I know the person in charge of making that work with PyOdide (Emscripten)
10:12
avatar
Calvin Prewitt
Calvin Prewitt says:I have some communication with Peter Wang / Anaconda that is working on PyScript and experimenting with both runtimes:
https://pyscript.net/tech-preview/micropython/index.html

10:15
BC
Brett Cannon
Brett Cannon says:Yep, specifically Nicholas Tollervey, who works for Anaconda, is driving the micropython work
10:16



## Action items
Brett will join BA Zulip
Joel will schedule another meeting in two weeks and do more component tool research
Kevin will keep battling numpy
