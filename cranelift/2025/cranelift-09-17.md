# September 17 project call

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
- abrown
- bjorn3
- alexcrichton
- Rahul Chapulkar
- jlbirch
- cfallin

### Notes

- Updates
  - alexcrichton: no updates
  - bjorn3: no updates
  - abrown: working on regalloc2 to add limited-range register constraints (for
    Intel APX)
  - cfallin: working on debugging on Wasmtime side; will need a few tweaks to
    Cranelift IR to carry a little metadata tying stackslots to debug
    (interactions with inlining are interesting)
  - Rahul: no updates
  - jlbirch: no updates
  - fitzgen: Wasmtime refactor: updated tables to look up functions in text
    section; unifies different kinds of functions together. Small regression in
    function lookups (seen in lazy table init benchmark in edge cases) but
    improvement in code size

- discussion of debug metadata and inlining
  - cfallin: inlining is interesting because it pulls IR from elsewhere that
    represents a "virtual frame". Support by:
    - adding metadata to stackslot so when it's copied over, we get a
      description of its layout (locals, etc)
    - having a notion of "debug tags" that attach as a *list* to instructions;
      prepend tags from call to tags of inlined instructions. Tag is enum of
      arbitrary u32 (embedder decides meaning) or stackslot reference that gets
      fixed up. E.g. Wasmtime could use to represent: state stackslot, current
      Wasm PC, current Wasm stack depth.
  - (alexcrichton, fitzgen: discussion about complexity and whether we need
    this right away vs prohibiting inlining)
    - cfallin: didn't want to back us into corner with data structures, and
      generality doesn't seem too bad so far; pragmatically if it turns into a
      huge headache we can always decide not to support it
