# October 02 project call

**See the [instructions](../README.md) for details on how to attend**

## Agenda
1. Opening, welcome and roll call
    1. Note: meeting notes linked in the invite.
    1. Please help add your name to the meeting notes.
    1. Please help take notes.
    1. Thanks!
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- fitzgen
- avanhatt
- elliottt
- alexcrichton
- uweigand
- cfallin
- abrown

### Notes

- alexcrichton:
  - removed wasmtime-specific trap codes; now it's a u8 index with some
    reserved values
  - merged cranelift-wasm into wasmtime-cranelift; cascaded into some wasmtime
    cleanups too
- elliottt: no updates
- uweigand: no updates
  - overflow ops: someone doing them for s390x, neat
- avanhatt: merging isle-veri: failed parser ISA-spec test
  - fails on main too; will file an issue
- cfallin: no updates
- abrown: no updates
- fitzgen: 
  - thinking ahead to optimizations spawned by WasmGC; filed some issues
  - optimizing trapping insts in the mid-end; issue filed describing how to do
    it by jameysharp, just need to implement
  - generalizing alias-analysis regions: from a few named regions to a
    user-managed index
