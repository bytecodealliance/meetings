## Agenda
- Status updates
- Who will be at WasmCon next week?
- What should be our priorities going forward?
    - (and who has bandwidth to work on them?)

## Notes

- Kevin: porting more packages, publishing them as wheels.  Big benefit
- Jamey: don't know what the pain points are
- Brett: whose problems do we need to solve? SaaS users
- Joel: want to collaborate with pyodide
- Kevin: people want sandboxing (and want to bring their deps, like Pandas); also want size optimization, but package support is table stakes
- Brett: how do people get CPython-WASI?  No official way yet.  Exceptions support would unlock micropython.  CPython using dlopen/dlsym for WASI.  Pyodide is using hacks which may become obsolete as people move away from setuptools.  Define standards for wheel naming and compatibility expectations.  
- Kyle wants to be able to generate components without wasi imports
- Brett's post on dev experience: https://snarky.ca/tag/wasi/; should be able to pass platform flag (including python version) to `pip install` _and_ should always use virtual env.  Working on an installer with a stable API.  _or_ run pip inside wasi, in which case it can just assume native.
- Brett: can ask Python for where site-packages is; Python discovers venv based on where it is launched from; https://snarky.ca/how-virtual-environments-work/
- Brett: ideally, user would just use `python` which knows it's for WASI runs everything in e.g. wasmtime
- Kevin: want customers to use a single tool that does everything for them
- Brett working on a lockfile standard including policy (e.g. always give me wasi)
- Brett needs browser REPL for running IDE in browser, but others need other things
- Joel: componentize-py can handle dependencies itself; no need for venvs because sole output is wasm file
- Brett: can prototype with componentize-py deferring to pip for downloading and unpacking and later replacing with something else (e.g. Rust library)
- Kevin volunteers to investigate fortran options
- Brett: short term: ask hood about porting pyodide-build to wasi-sdk; long term: determine new, better python cross compile story
- Kevin: most projects not that hard; just the fortran stuff

Joel's summary: Our desired user experience to begin with is that componentize-py uses the project's pyproject.toml to discover dependencies, download the WASI wheels for them (and transitive deps), unpack them into a temporary directory, link any native libraries into the produced component, and load everything else into a wasi-virt VFS.  For a first draft, just defer to pip with a virtual env; later we could do everything in Rust and eliminate the Python and pip deps.  Note that the virtual env is an implementation detail tied to using pip; we'd delete it once the VFS is created.

## Action Items

- Kevin will investigate current best way to compile Fortran to Wasm
