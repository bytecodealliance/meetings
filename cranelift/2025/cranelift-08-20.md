# August 20 project call

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
- alexcrichton
- bjorn3
- abrown
- uweigand
- jlbirch
- cfallin

### Notes

- Updates
  - abrown:
    - explored giving RA2 ability to constrain operands to a range of
      registers (e.g. first 16 of 32)
  - bjorn3:
    - currently testing update of `cg_clif` to latest Cranelift; don't
      expect anything wrong, but in progress
  - alexcrichton: no updates
  - jlbirch: looking at getting CI for APX/AVX10, waiting for kernel
    support, KVM support
    - cfallin: booting a full VM? we use qemu in usermode emulation
      support for other emulated archs -- does this exist yet?
    - jlbirch: not sure, seems not quite yet
  - uweigand: no updates
  - fitzgen: fixing fuzzbugs in inliner, working on compile-time builtins
    - inliner bug fixed: previously would inline very small callees
      before checking max size of after-inlined body; very small
      recursive bodies would cause indefinite inlining
    - inliner API on Cranelift now has mode to choose whether to
      revisit just-inlined body or not
  - cfallin: Wasmtime exception support about to land
  
- abrown: RA2 OperandConstraint and choosing subsets of registers
  - new kind of constraint called `Range` that specifies max register
    index in a class
  - (lots of discussion about encoding this in the `u32` -- abrown has
    a working scheme)
  - cfallin: sounds great; then in the iterator over possible
    registers during allocation loop, filter out regs above the limit
  - abrown: maybe it's actually a variant of `Reg` where every such
    constraint has a max
    - cfallin: sounds good
  - abrown: if only some insts support it, will moves be inserted
    properly?
    - cfallin: yes: liveranges will be constrained appropriately and
      either whole LR will be in a low-half reg or will split and have
      moves
