# December 11 project call

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
    1. tintoedoardo: supporting checkpoint and restore in Cranelift (aiming for live migration of components). 
    2. _Submit a PR to add your item here_

## Notes

### Attendees

- fitzgen
- saulcabrera
- alexcrichton
- abrown
- cfallin
- tintoedoardo
- uweigand
- mmcloughlin
- jlbirch
- bjorn3
- Alex Hansen

### Notes

- tintoedoardo: checkpoint/restore work in progress implementation
  - segment application into regions, take checkpoints after each one
  - Qs: where to store checkpoints; how to interrupt; ...
  - cfallin: Cranelift-based or Wasm instrumentation: A: Cranelift
  - fitzgen: save native stack? A: avoid saving native stack, save a checkpoint
    of live values
  - cfallin: how to reenter into middle of function? and how to rematerialize
    stack?
    - A: assume one function for now; and entry block that jumps
  - cfallin: CPS would resolve question of calls on stack; but perf penalty
  - fitzgen: or restrict checkpoints to points at which control returns to top
    level (no calls on stack)
  - fitzgen: stackmaps to rewrite native stacks would be nice too. Feeds into
    record/replay work possibly (which we want to build eventually).
  - cfallin: tie-in to Wasm debugging -- if we had a way / all the needed
    metadata to materialize the Wasm VM-level state (operand stack, locals)
    that would be what you would need for a snapshot, and we also want this for
    the Wasm debugger.
  - fitzgen: Wasm coredumps are the format we want -- they record exactly that!
  - bjorn3: analogy to opt/deopt in JITs, mapping state between different
    compiler tiers
  - cfallin: yes, OSR is exactly what we want too (reenter in the middle)

- Updates
  - alexcrichton: working on Pulley lowerings. For end: reading registers and
    widths.
  - Alex Hansen: no updates, just wanted to join, open meeting, trying to use
    Cranelift for own language
  - bjorn3: working on improving ABI situation on rustc side; and vendor
    intrinsics in cg\_clif by pre-compiling with LLVM
    ([issue](https://github.com/rust-lang/rustc_codegen_cranelift/issues/1547))
  - jlbirch: no updates
  - mmcloughlin: no updates
  - uweigand: no updates
  - abrown: prototype Cranelift assembler defined by a DSL; RFC posted
    ([link](https://github.com/bytecodealliance/rfcs/pull/41))
    - lots of questions, but one Q is how to handle other ISAs -- all switch
      over, migrate gradually, let x64 try it first, ...
  - saulcabrera: helping to push Winch aarch64 impl forward -- three or four
    instructions away from complete
  - cfallin: no updates
  - edoardotinto: no updates
  - fitzgen:
    - writing blog post about portability of Wasm and Wasmtime, call to
      action in finishing Pulley
      - issue for finishing Pulley:
        [link](https://github.com/bytecodealliance/wasmtime/issues/9783)
    - writing blog post about year-end summary of Wasmtime progress as well

- alexcrichton: how to track undefined upper bits in registers?
  - cfallin: "demanded zero-extend" mechanism: as we see uses, track widest
    demanded uextend; make that so on producer side. Instructions can say how
    many bits they define.
  - (fitzgen, alexcrichton, cfallin: lots of discussions about tradeoffs, safe
    defaults)
  - uweigand: lowering rules do this today?
    - cfallin: key idea: make `put_in_reg` give full width by default, rather
      than let ISLE rules manage this, to make it hard to make a mistake;
      lowerings can explicitly ask for narrower if they can deal with undefined
      bits.
  - abrown: can we keep it in ISLE?
    - cfallin: need to go to framework because we're tracking metadata on Regs
      and also crossing single lowering entry points -- scope is whole lowering
      algorithm
