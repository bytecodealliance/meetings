# January 22 project call

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

- erikrose
- fitzgen
- alexcrichton
- saulcabrera
- cfallin
- Rahul 

### Notes

- Updates
  - alexcrichton:
    - steady stream of fuzzbugs, mostly Winch and Pulley related
    - divergence between backtracking and singlepass
    - fix: sign-extending args in riscv64 ABI
  - saulcabrera:
    - PR reviews on winch
    - finishing aarch64 on winch
    - handling of shadow/aligned stack in aarch64 causing segfaults with signals, ...
  - cfallin:
    - gave talk at WAW 2025 on Cranelift/Wasmtime correctness; hopefully will
      get back to PCC at some point
  - erikrose:
    - new hire at Fastly on Cranelift/Wasmtime; welcome!
  - Rahul Chaphalkar
    - no updates
  - fitzgen
    - no updates

- discussion about regalloc divergence
  - root problem: we have a "compound branch" with some kinds of floating-point
    conditions that uses a one-armed branch instruction followed by the usual
    terminator. This breaks our basic-block invariant. Single-pass allocator
    wants to insert spills before "the terminator" and ends up putting them
    between the branches. Backtracking (production allocator) doesn't because
    there are multiple successors so it inserts edge-moves in successor blocks'
    headers.
  - cfallin: two fixes: no compound branch / restore BB invariant (we should do
    this anyway); also should single-pass put edge-moves in successors in this
    case?
    - no, turns out the moves are spills logically associated with the pred
      block, so they need to go there; so we need to fix the branches
  - fitzgen: can we detect/assert against this?
    - not really, always possible to smuggle a branch in and lie about whether
      it's a branch; we need to audit the MachInst defs and get rid of any
      non-terminator "hidden branches" (unless within a closed sequence)
