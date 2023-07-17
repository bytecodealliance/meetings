## Agenda

- [componentize-py](https://github.com/dicej/componentize-py) update
- [Shared-everything linking RFC](https://hackmd.io/IlY4lICRRNy9wQbNLdb2Wg?both) and [experiments](https://github.com/jameysharp/wasm-ld.so-test)
- Summary of the [WebAssembly Summit at PyCon US 2023](https://github.com/psf/webassembly/blob/main/meetings/2023/webassembly-summit-pycon-us.md)
- Anything else?

## Notes
Joel:
- componentize-py is feature complete but not ergonomic yet
- planning to split the shared parts of binding generation out of wasmtime-py for reuse here
- reserved a PyPI project for componentize-py though it's not installable yet
- may ask for help with making it usable on multiple platforms (Brett offers to provide pointers when ready)
- can't use any native code with that yet, but that's what the Shared Everything work is for

- prototyping with Emscripten is happening
- trying to avoid the situation where a Python package ships an Emscripten library that someone tries to link into a WASI cpython
- Joel is meeting with Emscripten folks on Monday to discuss this further
- current proposal: [Shared-everything linking RFC](https://hackmd.io/IlY4lICRRNy9wQbNLdb2Wg?both)
- when invoking componentize-py the idea is to specify that I want to use a set of wheels and it should extract the .so files to pre-link them into the component
- wasi-sdk doesn't want to provide stable ABI across SDK releases, so a cpython release needs to specify a particular wasi-sdk version that all native code must be built with

Brett:
- right now, tying cpython to a specific wasi-sdk release is not out of the realm of possibility
- on other platforms, C library SDKs are either forward- or backward-compatible so wasm will be different than other platforms
- some people may care?

- Jamey: linker tool doesn't care about ABIs except that they match.  Concern is that libc ABI may change across wasi-sdk release
- Brett: will read proposal and provide feedback
- Jamey: interested in distinction between emscripten and WASI.  Based on experimentation, custom sections are same currently due to shared LLVM, but libc is different and thus ABI is different.  Could hypothetically design a unified libc ABI which both emscripten and WASI can use?  Not there today or soon, so we need to be explicit.
- Brett: also consider future changes emscripten might make
    - Standardize lowest common denominator?
    - Could emscripten tools have a WASI flag to target that lowest common denominator?
- Jamey: ideally everything is the same except libc ABI, and someday we can unify that too
- Brett: could have both: move fast and break stuff but also have option to target a standard

- Brett: thought about avoiding shared library filesystem paths, but can't avoid it without something isomorphic.
- Jamey: link in agenda to ongoing experiment including component glue in WAT

- Brett: WA summit at PyCon; Kevin was there
    - invite only to avoid noise
    - inaugural
    - told people about Discord, etc.
    - Created PSF repo linked to above for meeting notes, etc.
    - Emscripten world well funded and staffed; wasi not so much, but everyone's supportive
    - Complicated to explain
    - EuroPython in Prague in July
    - See notes above
    - Future pain points: packaging, etc.

## Action Items
- Joel will look into either using the PSF repo or the BA repo for notes and links
- does wasi-sdk expose a C preprocessor symbol or something with the SDK version so we can extract it while building cpython/wheels?
