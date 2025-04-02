# April 02 project call

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
    1. [Discussion of multi-value returns](https://github.com/bytecodealliance/wasmtime/issues/10488)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

- fitzgen
- alexcrichton
- abrown
- cfallin
- Rahul Chaphalkar
- uweigand

### Notes

- loading return values with separate VCode instructions
  - cfallin: issue came up with exception support: we can't have loads that are
    logically part of the call as separate trailing VCode instructions, because
    the call is a terminator now. Tried splitting edges, way too
    ugly/unworkable; we need to do the loads as part of one pseudoinst.
    - filed https://github.com/bytecodealliance/wasmtime/pull/10488 with all
      the gory details of all the approaches tried
  - this is now actually basically resolved by
    https://github.com/bytecodealliance/wasmtime/pull/10502, and exceptions are
    unblocked
  - remaining issues: do we simplify ABI code by supporting only register
    returns, and pushing stack returns to "userspace" (Cranelift
    user/embedder)? And related: issues with struct-ret
    - probably not for now -- if it works, it's actually nice to have the
      uniform support and not have the frontend have to think too hard about
      ABI-specific issues
    - struct-ret issues are a separate question; follow up with `cg_clif` to
      see what we can do, e.g. provide the primitive to put the pointer in the
      right register (not necessarily one of the normal arg/return registers)
      and let frontend handle those details. This is logically separate from/in
      addition to handling arbitrary numbers of *primitive* returns (as
      Wasmtime uses)
      - https://github.com/bytecodealliance/wasmtime/issues/9510

- Updates
  - uweigand: no updates
  - Rahul: no updates
  - alexcrichton: no updates
  - abrown: some assembler updates; started removing Inst enum variants as
    replaced by new assembler
  - cfallin: exception handling support; PR up; happy-path (normal return)
    should work, exception unwinding path is untested because clif-util doesn't
    have an unwinder yet; that's next
    - https://github.com/bytecodealliance/wasmtime/pull/10510
    - depends on https://github.com/bytecodealliance/wasmtime/pull/10502
    - s390x will need separate work since it has a separate ABI implementation
    - alexcrichton: feel free to skip Pulley for now if becomes an issue.
      Exception-raising libcall is Pulley-specific, for example
      - cfallin: Pulley unwind should be reasonable? no host frames, etc
  - fitzgen: no major updates; will update exceptions RFC with last details
    we've worked out and merge
