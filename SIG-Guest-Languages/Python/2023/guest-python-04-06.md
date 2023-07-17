## Agenda
- Update on [`componentize-py` prototype](https://github.com/dicej/componentize-py)
- Update on ["dynamic linking" for Wasm](https://hackmd.io/6ilCS4eCRO-rWn17uVcY5g)
- SIG status
- Schedule next meeting (after Kubecon, probably)
- Anything else?

## Notes

https://github.com/bytecodealliance/governance/blob/main/SIGs/SIG-dynamic-languages/proposal.md

- Asen: VMWare WASM labs
  - looking at PHP at the moment
  - want legacy apps to move to Wasm seamlessly
  - validate tools (e.g. find omissions)
  - limited cycles for Python for next month or two
- Kevin: PHP under wasm or vice versa? Asen: PHP under wasm
  - some stuff working, some not
  - SingleStore is interested for DB UDFs
  - all open source in GitHub - https://github.com/vmware-labs/webassembly-language-runtimes
  - Asen interested in helping with all dynamic languages
- Brett having success with PHP and Ruby builds
- Brett posted latest 3.11 and 3.12 build artifacts as part of tier 2 work
- Jamey spoke with Luke about dynamic linking
  - started to morph into something else
  - spoke with manager about original version and going from there
  - Luke would like component-oriented version, but Jamey would like more core module focused
  - Key idea: library has symbols which might conflict with other libs
    - take static lib and preprocess and remove all exported symbols and put them in a separate table
    - make above table available to dlopen/dlsym
    - concat function tables
  - Brett: for context, this is solving the native lib packaging problem
  - Luke would like:
    - separate compilation, resident in memory once (e.g. multitenant)
    - should "feel" more like the rest of the component linking
    - weird to message "dynamic linking but not really"
  - Perhaps Jamey's approach could lay foundation for Luke's vision
  - Kevin: dlopen really needs to work in order to make it seamless
  - Brett: agreed; don't diverge too much from existing platforms
  - Jamey wondering about dlopen taking file paths; would rather not be in filesystem basis
    - could be abstract string
    - Brett: need to be able to implement both a finder and a loader
        - could be custom for Wasm without file paths, but would diverge significantly from other platforms
        - but how does that work with packaging
  - Joel: needs to be position-independent code


## Action items
