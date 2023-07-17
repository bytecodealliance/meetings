## Agenda
- Updates on [previous meeting's](https://hackmd.io/q5SWcHt1TWaYWcMtt-xI9g) action items
- A design proposal for `componentize-py`
- Thoughts on a Wasmtime-based single-file executable strategy for Python
- Anything else?
- [Indicate availability](https://www.when2meet.com/?19324015-IOqv5) for next meeting

## Design Proposal for `componentize-py`

Goal: provide a `componentize-py` CLI executable which takes a Python app and its dependencies, plus a WIT file, and produces a Wasm component.

This would include four inter-related parts:
- Runtime module
    - CPython + [Py03](https://github.com/PyO3/pyo3) + shim API exports, built for wasm32-wasi
    - Shim API exports would include Wizer init function and canonical ABI lifting helper functions (e.g. `make-python-int`, `get-python-lint`, `make-python-list`, etc.), and also makes them available to Python scripts
- Binding generator (similar to [js-component-bindgen](https://github.com/bytecodealliance/jco/tree/main/crates/js-component-bindgen) and consistent with current [Python host binding generator](https://github.com/bytecodealliance/wasmtime-py/tree/main/rust/bindgen))
    - Generates a Python script which declares all the data types used in the input WIT world, plus functions which use native functions implemented by the runtime to lower Python objects using the canonical ABI
    - Generated script should also declare export function stubs for type checking. (TODO: what would this look like?)
- Shim tool (e.g. reuse, modify, or parallel [spidermonkey-embedding-splicer](https://github.com/bytecodealliance/componentize-js/tree/main/crates/spidermonkey-embedding-splicer))
    - This generates a component from a WIT file, implementing exports in terms of the runtime shim API described above, as well as a table of imports which the generated Python bindings can call
- A `componentize-py` app which embeds the above runtime module, and:
    - Generates a Python script from the provided WIT file using the above binding generator, declaring all the data types, import functions, and export stubs
        - The `componentize-py` could optionally stop here if requested, just generating the script for type checking by IDEs without creating a component.
    - Invokes the Wizer init function on the runtime module, feeding it the above Python script and loading the app and its dependencies
    - Convert Wizer output into a component using [wit-bindgen](https://github.com/bytecodealliance/wasm-tools/tree/main/crates/wit-component) and [the WASI Preview 1 adapter](https://github.com/bytecodealliance/preview2-prototyping)
    - Generate a shim component using the above shim tool and the WIT file
    - Compose the shim component with the app component

## Meeting notes

Kevin's update: got NumPy up and running.  They have an emscripten flag, need a wasi flag too.  Had to create custom wasi-sdk, wasm-ld, etc. builds to debug. Python fix to import nested shared libraries.  Hoping to upstream both.  Hoping to post a repo soon.  10+ libraries for NumPy, 40+ Pandas. Polars probably easier than Pandas.

Pandas had lots of name clashes.  After renames, it builds.  Other things: requires ctypes (not used much, but pulled in on import), mmap (also not used much, but also pulled in on import).  NumPy will use ctypes if it exists, so adding it fixed Pandas but broke NumPy.  Pandas works with ctypes and mmap stubs.  Only when a command, not a reactor.

Wasi-vfs + Pandas had libc collisions.  Solved by dropping in wasi-sdk file to override.

Pyodide has a bunch of patches for common libraries.

Emscripten has dynamic linking, so no precendent for using .o and .a files.  Each tool has custom build tools for native code.  No universal way to handle everything.  They all support CFLAGS, generally speaking.

Kevin hacked wasm-ld to generate .a files instead of .so files.

Jamey proposes creating a dlopen/dlsym that uses static tables and statically linked code instead of the filesystem.  The .so files would be core wasm modules intended to be statically linked.  Could put those in a registry for anyone to use.  Could have wasi "wheels".  Need to intercept stat calls, not just dlopen.

## Action Items

Joel:
- Talk to Kyle about SIG status
- prototype componentize-py
- talk to Alex, Dan, Luke, etc. about module linking and faking dlopen/dlsym/stat/etc
- help Kevin debug Pandas-in-reactor

Kevin:
- Clean up NumPy/Pandas hacks and publish to repo
