## Agenda

- Status updates
- Kevin's Fortran update

## Notes

- Joel: WasmCon and BACon went well; found some bugs, started fixing them
- Joel: working on resources support for `componentize-py`; aiming to finish early next week
- Joel: Dan and Alex working to update WASI WIT files to use resources
    - https://github.com/orgs/bytecodealliance/projects/10/views/1
- Brett: still working on making CPython WASI Tier 2.  Maybe Tier 1 someday if it's shown to be easy to develop for.
- Kevin: LFortran seems to be the best/only thing on the horizon for SciPy, etc.
    - SciPy is an explicit goal for them: https://github.com/lfortran/lfortran/issues/1377
    - Also, the SciPy maintainers are working to clean up and modernize their Fortran, which may help
    - Maybe LFortran will be able to handle SciPy within the next year or so?
- Kevin and Brett: LPython looks interesting.  Brett: lots of promises, not a lot delivered so far
    - Still very early
    - Brett: also MyPyC, Cython; generaly requires type annotations that make static checking pass
    - PyPy project also looking at AOT compilation
    - https://github.com/spylang/spy
- Joel: is JIT an option for dynamic languages (as opposed to just AOT and pure interpreters)?
    - Jamey: Fastly really doesn't want a compiler on the deployed fleet for security, but it's tempting
    - Andy Wingo has written about this and built prototypes (maybe Python?)
    - Brett: would be nice to target Wasm with JIT
    - Jamey: could approach it from a Wizer 
    - L: Python has quickening phase which we could maybe plug into, but may still need to deoptimize and reoptimize dynamically
    - Chris F working on SpiderMonkey; has ideas about inline caches.  Idea is that we can get most of the benefit AOT.
        - Early signs show it helps, but not sure how much
- Brett: chatting about Mojo

## Action Items

- Joel: remember to record next time
