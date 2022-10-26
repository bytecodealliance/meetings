# October 26 project call

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

- uweigand
- elliottt
- afonso360
- jlbirch
- jameysharp

### Notes

uweigand:
- patch to remove regmod from s390x landed
- new patch to clean up remaining non-SSA remnants
- removed all support for lowering non-SSA input
- found a few places that were generating non-SSA
- can't find any more non-SSA
- trevor is working on adding asserts to find non-SSA
- fixed issues Afonso opened about s390x backend:
 - sret arguments: open issue about what the long term approach, since ABI seems to be Intel-specific
 - regalloc checker failures: caused again by register 0; hopefully last of them?

afonso360:
- removed iconst.i128
- simple, but lost some optimizations
- can maybe get them back with egraphs
- adding more opcodes to the fuzzer
- reporting missing lowerings

jameysharp:
- helped Chris merge egraphs two weeks ago, then went on vacation
- resuming PR review now
- planning to try an alternate pattern-match strategy in the ISLE compiler
- planning to try Chris' idea for extending DataFlowGraph to be an egraph rather than copying in and out

jlbirch:
- formatting output from GitHub Action
- annoying to test since half the action works with push, half with PR comment
- both triggers act on different versions of the YAML: currently merged in main versus version in PR
- can't test on main wasmtime repo, have to use a personal fork

elliottt:
- planning to expose regalloc's SSA checker and use it in CI
- revived branch from chris/ulrich to remove non-SSA codegen from s390x
- started removing redundant branch instructions
- removed brif, bricmp, etc
- reworked selectif\_spectreguard to take i8 instead of iflags
- working toward removing both branch instructions and iflags type
- introducing replacement for iadd\_ifcout+trapif which doesn't work without iflags
- discussion about iadd\_cout
- ulrich:
  - these are unused currently and unimplemented on all ISLE backends
  - ISLE has trouble merging instructions that have two users
  - nearly all two-output instructions have two users
  - if we keep iadd\_cout we want a helper
- afonso360: cg-clif wants the cin/cout variants for implementing LLVM intrinsics which are exposed in libstd
