# Agenda

- Status updates
- Best way to control resource lifetimes in `componentize-py` (AKA please educate Joel about Context Manager and `with` statements)
- SciPy and Flang 17: An answer to our Fortran woes?
- Anything else?

# Notes

- Joel update: new version of `componentize-py` supports WASI Preview 2 snapshot 0.2.0-rc-2023-10-18.  Working to update `wasi-virt` to use same so we can use it to provide e.g. a virtual filesystem etc.  Planning to add sockets support to `wasi-libc` starting next week or the week after.
- Kevin: haven't gotten around to adding WASI stubs to `wasi-libc` yet; will be out most of November
- `with` statements: must provide `__enter__` and `__exit__` methods in class you want to use.  Not clear how to match up the `__enter__` part with resources; can we just do nothing there?
- SciPy and Flang 17: nobody knows of a reason why it couldn't work for Wasm -- just need to try and see what happens

# Action Items
