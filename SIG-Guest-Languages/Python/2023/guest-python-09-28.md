# Agenda
- Updates
- WIT resources: how to handle Python code generation for static methods?
- Minor LFortran update

# Notes
- Joel's update: still implementing resources in `componentize-py`.  Had to fix bugs and/or add features to a few other projects (e.g. `wasmtime-wit-bindgen`, `wit-component`).  Should be done by the next meeting (edit: done!).
    - Also helped Ondřej Čertík on Zulip/GitHub with building LFortran executables using Wasmtime+Cwasm.  He reports good progress on making SciPy compile and getting the tests to pass.
- Regarding WIT resource: use `Self` and `cast` to avoid `Any` in methods that take or return instances of the implementing class


# Action Items
- Joel: finish resource support in `componentize-py` (edit: done!)
