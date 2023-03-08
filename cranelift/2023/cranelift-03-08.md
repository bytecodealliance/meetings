# March 08 project call

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
    1. Constraining instruction signatures to exclude vectors (`bmask`, for
       example)

## Notes

### Attendees

- elliottt
- cfallin
- afonso360
- jameysharp
- fitzgen
- abrown

### Notes

#### Constraining instruction signatures to exclude vectors (`bmask`, for example)

- Trevor:
  - How to opt in all instructions to fuzzing
  - Lots of instructions are legal for vectors but not implemented in any backends
  - Did we enable these intentionally?

- Chris: Lets constrain instructions to what is actually supported today

- Andrew:
  - SIMD versions of instructions were added eagerly based on what could be
    implemented, not on what was actually supported.
  - Tried to reuse the same opcode for both scalar and vector versions

- Chris:
  - Try adding as much polymorphism as much as possible to the current instructions
  - Makes vectorization easier

- Jamey: When one backend has one implementation but no one else does, what do we do?

- Andrew: Weird to remove functionality

- Trevor: We can disable those in the fuzzer via the current `valid_for_target` mechanism.

- Afonso: Were you able to resolve opcodes with unbound types in the signature?
  - Jamey: Handled!

- Jamey:
  - Some opcodes such as `uextend` / `sextend` are missing constraints such as the return type must be larger than the input type.

- Chris: We have those validations adhoc in the verifier

- Nick: The fuzzer could add extra logic for those special cases

- Trevor: We can pull it out of the verifier and try to get it into the opcode constraints. There could be cases where it might not be easy.

- Jamey: The interface for opcode constraints is fairly general.

#### Status Updates:

- Trevor:
  - Investigating opting more instructions into fuzzing
  - regalloc stuff

- Chris:
  - Pair with Trevor

- Jamey
  - Pair with Trevor
  - PR reviews / Issue Triage

- Andrew:
  - Is mixing AVX/SSE still bad in modern CPU's?

- Afonso:
  - Looking into adding SIMD to the cranelift fuzzer
  - Had to disable alignment for > 16bytes
    - Currently no way to ensure that with the current cranelift functionalities
  - Should be ready soon
  - Planning on adding stack alignment functionalities to cranelift
  - Andrew:
    - Do we test AVX or other extensions?
  - Afonso:
    - Not yet!

- Nick:
  - Discussing with Chris how to analyze large pieces of code, currently hard since we have to look at the assembly manually
  - Thinking about building a viewer where we look at native code vs wasm code

