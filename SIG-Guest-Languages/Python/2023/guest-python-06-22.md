## Agenda
- Shared-everything linking and `componentize-py` status update
- Plan for building and publishing WASI wheels
- Add people to meeting invite

## Notes
- Shared-everything linking: everything works; working on upstreaming PRs to LLVM, wasi-sdk, Rust, and wasm-tools
- componentize-py: big renovation to support new wit syntax, shared-everything linking, and component pre-initialization
- https://github.com/python/cpython/commit/d8f87cdf94a6533c5cf2d25e09e6fa3eb06720b9 adds wasi-threads to cpython
- proposal: start repo for building wasi wheels for python packages
    - Brett: could create a JSON index pointing to hosted wasi wheels, which pip can use
    - Joel: could put build scripts in github repo and have CI publish wheels and update index
    - Jamey: GitHub Pages?
    - Brett: might need custom content type
    - Hood: Pyodide started with GitHub Pages, but e.g. scipy is huge, then AWS+cloudflare.  Now AWS+donated JS deliver service
- Hood: for ctypes, could hypothetically extend Wasm to support "fat function references", i.e. closures with function reference + environment pointer
    - could polyfill in browser
    - Jamey: why do these need to be c function pointers?
    - Hood: imagine calling qsort with a curried function
    - Joel: examples of Python packages that might do this? Hood: Shapely pre-2.0 (https://github.com/shapely/shapely/tree/rel-1.5.3, see faq as to why they don't anymore) uses ctypes; Kevin: NumPy in some cases
    - Hood: Lib devs who want to avoid any dependencies and locking into Python version need this kind of dynamism
    - Hood: can conflict with stack switching because can't switch with JS frames
    - Hood: call-indirect will trap if wrong arity; need adapter
- Hood: emscripten uses JS trampolines for exceptions
    - Almost have it working with WASM exceptions proposal; WASI should use same
    - Jamey: bjorn3 might be able to help debug
-

## Action Items
- Joel add Kushal to invite
