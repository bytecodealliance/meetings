# April 17 project call

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
    1. [x64, fcmp, and load-op merging](https://github.com/bytecodealliance/wasmtime/blob/b26fd0af77ddce3431ce97065380241b20ef76a1/cranelift/codegen/src/isa/x64/inst.isle#L2837-L2838) (@alexcrichton)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

* Nick Fitzgerald
* Alex Crichton
* Jamey Sharp
* Andrew Brown
* Trevor Elliott

### Notes

* x64 and fcmp and load merging
  * Alex: only one inst we don't do avx version of
    * in theory, don't want to mix sse and avx
    * don't think this matters too much in practice
    * had branch to resolve this, but one comment made hesitant
    * turns out comment is outdated
    * will land soon
  * discussion of operand ordering for vcode/ISLE helpers on x64 (att style vs intel style)
    * currently inconsistent
    * general consensus to move towards intel style, since that is what is in
      the intel manual and on felix coultier's site
* status updates
  * Andrew
    * none
  * Alex:
    * not related to cranelift, but
    * reviving async fuzzer for wasmtime
    * (address? cov?) sanitizer's intrinsics for marking context switching broken
    * but can avoid that (somehow; missed it) with a pool of stacks
  * Trevor:
    * working with Jamey on tail calling conventions stuff
    * x64 is eagerly resizing the incoming arguments area to largest tail
      callee, potentially resizing down at tail call sites since it may now be
      too large
    * similar to what s390x is already doing
    * this is generally a large improvement in code quality
    * aarch64 has argument area resizing landed, and PR for adding callee-save registers
    * riscv64 has argument area resizing PR, next up will be adding callee-save registers
    * once that's done we can enable wasm tail calls by default in Wasmtime
    * also hope to get s390x using the same shared ABI code as other backends
  * Jamey:
    * working with Trevor on tail calling convention stuff, see above
    * also spinning out little clean up PRs in the background
    * made `RealReg` a `PReg` rather than `VReg`
  * Nick:
    * none
