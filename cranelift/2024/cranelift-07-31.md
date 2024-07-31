# July 31 project call

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
    1. Discussion of [pointer provenance vs Pulley vs Cranelift](https://github.com/bytecodealliance/wasmtime/issues/9015)
    2. liveness analysis and user stack maps
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- fitzgen
- alexcrichton
- uweigand
- elliottt
- avanhatt
- abrown
- cfallin

### Notes

- Pointer provenance and Pulley
  - alexcrichton: recap: Pulley has arbitrary loads/stores from integer
    addresses; doesn't work with strict provenance
  - turns out Rust won't require strict provenance; breaks lots of things
  - under loose provenance, we do need to mark pointers that escape to Pulley
  - fitzgen: things we need to do for escapes to Pulley are the same we do for JIT code
  - cfallin: PCC provides the same thing with memory types -- could use this as
    a way to get provenance in the future
  - abrown: how did we get on this quest?
    - alexcrichton: originally, UB bug found by miri; then Pulley, getting
      tests running in miri

- liveness analysis and user stack maps
  - fitzgen: all tests passing with user stack maps now, but debugging a
    fuzzing failure in table-ops test
  - fundamentally need either a fixup pass or full fixpoint loop for liveness
  - cfallin: third way: IonMonkey regalloc's approximation: when we see a loop
    edge, take the loop end's liveness and smear it over the whole loop body

- updates
  - jameysharp: Cranelift libcalls and stack limit checks; setting on Cranelift
    to tell it to emit stack-limit checks in such cases
  - elliottt: no updates
  - avanhatt: no updates
  - abrown: no updates
  - alexcrichton: no updates
  - uweigand: done with tail calls on s390x! PR pending
    - indirect calls need a register for the target; can't be a callee-saved
      register because restores happen before the call. Need a register that is
      not an argument and not a callee-saved register. Using spilltmp for now,
      which requires an extra move (because it is not allocatable).
    - do we want to remove the tunable that disables tailcalls? (consensus: yes)
  - cfallin: no updates
  - fitzgen: modified Imm64 canonicalization to be zero-extended rather than
    sign-extended
    - discussions about proper printing format
