# Agenda
- Status updates
- Discuss meeting frequency
- Next steps for `componentize-py`, `CPython`, etc.
- Staffing for data science stack in browser
- Plumbers summit and wasm.io availability
- Anything else?

# Notes
- Joel: resources supported in componentize-py; not planning on doing much else for Python in near future
- Brett: automating cross build for CPython; Wasmtime 14 caused headaches due to CLI changes; Aiming for tier 2.
- Brett: Need to tell anaconda folks not working on emscripten anymore; paternity leave until July
- Jamey: planning on wasmtime point release for better backwards-compat in CLI
- Bailey: also working to use Wasmtime 14
- Joel: thoughts on meeting cadence
- Brett: might have more updates after returning from paternity leave
- Bailey: plumbers conference: End of January, Feb 13th, or colocate with wasm.io?
- Joel: would like remote option
- Joel: stick with every two weeks, but cancel aggressively
- Brett: next CPython multithread build should work
- Kevin: waiting for Preview 2 at work.  Interested in slimming down.
- Joel and Jamey: Preview3 will bring composable async
- Brett: group at MS that wants Python working in browser and desktop (would like WASI if it makes sense).  May throw money and/or people at problem.  Want data science stack (numpy, pandas, etc).  Need WASI .so builds of those things.  Want it solved in a year.  One person, two people?  BA funding?
- Joel: Jco is the polyfill
- Joel: How much Fortran needed? Brett: need SciPy
- Kevin: do you need true dynamic linking? Brett: no
- Summary: Fortran/SciPy is the long pole by far.  Talk to e.g. Hood (Pyodide) and/or Ondrej (LFortran) about how to accelerate.  Contracting Igalia might be an option.  Guy might be able to use more help on Jco, but it's moving along well already.
