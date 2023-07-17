## Agenda
- Guest Languages SIG administrivia
    - Open to moving meeting to friendlier time and/or staggering; need feedback to choose a time
    - Haven't set up shared calendar yet
- Progress on shared-everything linking ([demo](https://github.com/dicej/component-linking-demo))
    - Next steps: dlopen/dlsym support, micro-json proof-of-concept
    - Perhaps Brett could walk us through the process of loading a native extension?
- Next steps on `componentize-py` (updating to new WIT syntax, resources)
- Conformance test and/or demo
- WASIX
- Anything else?

## Notes
- Kevin: original WASIX (not wasmer one) from singlestore adds more POSIX stuff (e.g. stubs) to WASI, and Dan is interested in adding it to wasi-libc
- Brett demos import process for native extensions
    - sys.path is load path
    - import ujson: look at sys.path, check each dir to find ujson.cpython-31-x86_64-linux-gnu.so (or compatible?)
    - dlopen, dlsym PyInit_ujson -- return value defines the module
        - that's the _only_ thing it dlsyms (unless library uses ctypes to do its own dlopen/dlsym)
        - https://docs.python.org/3/extending/extending.html#the-module-s-method-table-and-initialization-function
    - numpy has multiple .so files
        - import numpy.core._umath_tests
        - numpy `__init__` files define `__path__` where libs can be found for each package and subpackage
            - aka `__spec__.submodule_search_locations`
    - https://github.com/python/cpython/blob/4ff5690e591b7d11cf11e34bf61004e2ea58ab3c/Python/dynload_shlib.c#L53
    - https://docs.python.org/3/c-api/module.html#initializing-modules
    - https://docs.python.org/3/extending/extending.html#a-simple-example
    - just care about file name; no need to have content or metadata
    - https://www.tweag.io/blog/2022-03-17-libffi-wasm32/
    -

## Action Items
- Joel post when2meet link on Zulip for potentially move SIG meeting time
- re-invite Brett and invite Kushal
