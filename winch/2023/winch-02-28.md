# February 28 project call

**See the [instructions](../README.md) for details on how to attend**

## Agenda
1. Opening, welcome and roll call
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. Calling convention for internal functions
	1. `FuncEnv` trait to resolve runtime information
	1. Avoiding arbitrary spills before emitting a `call`

## Notes

#### Calling convention for internal functions

* There's a design space here, but the most important thing is to stick to the
  invariants for the ABI, and do not work on top of assumptions.
* One of the advantages of following the system ABI is that no trampolines are
  needed in the "normal" case; in the case of Winch given the nature of the
  compiler we need trampolines always, so this is not something that we can
  avoid.
* There are some cases in which a new ABI is needed for example for features
  that are not supported by the system abi (e.g. tail calls)
  
#### `FuncEnv` trait

* Offers a good balance between ergonomics and flexibility to avoid a static
  dependency on `wasmtime-environ`. 
* The trait will be fulfilled by `wasmtime-winch` and passed in to to
  `winch-codegen`
