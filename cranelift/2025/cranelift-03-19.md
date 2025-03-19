# March 19 project call

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
- abrown
- bjorn3
- alexcrichton
- cfallin

### Notes

- Updates
  - erikrose: no updates
  - abrown:
    - new assembler causing some unaligned memory accesses again; propagated
      through XmmMemAligned to re-fix it
  - bjorn3: no updates
  - alexcrichton
    - lots of fuzzbugs in the last week (mostly GC related)
  - cfallin: starting to implement exceptions

- cfallin: exceptions. Recapping [some issues with RFC's current
  model](https://github.com/bytecodealliance/rfcs/pull/36#issuecomment-2735268674)
  in CLIF mechanism
  - (lots of discussion about tradeoffs, which thing do we make "weird" --
    edges, value defs, target blocks, ...)
  - we can't get rid of payloads -- need them for native code
  - define rets and payloads as results of `try_call` and try to limit in the
    validator where they're used? (As args only to the block calls?)
  - end result: let's actually use placeholders in the block calls (`ret0`,
    `ret1`, `exn0`, `exn1`, ...); block calls' arguments are semantically a sum
    type over normal values or one of these placeholders.
    - This makes the invariant local: block calls are well-formed if their args
      conform to the signature of the `try_call`'s target and the exn payload
    - This keeps the 1-to-1 correspondence between block call args and block
      params of the target block
    - This puts the special logic exactly where it needs to be handled -- where
      do the block call values actually come from -- rather than complicating
      all edge handling or disallowing certain kinds of code motion or
      block manipulation, as our other ideas would have done.
    - cfallin: will post another comment in RFC


- alexcrichton: wasmtime objdump, new tool
