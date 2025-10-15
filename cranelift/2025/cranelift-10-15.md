# October 15 project call

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

- bjorn3
- alexcrichton
- fitzgen
- Ayrton Chilibeck
- cfallin
- Rahul
- uweigand
- jlbirch

### Notes

- Updates
  - Rahul: no updates
  - alexcrichton:
    - s390x native runners had long queue times; switching back to qemu
  - bjorn3: no Cranelift updates; been working on rustc
  - cfallin: landed Wasmtime instrumentation that uses Cranelift debug tags;
    working on debug stuff mostly on the Wasmtime side now
  - uweigand: no updates
  - jlbirch: no updates
  - fitzgen: no updates

- fitzgen: stackmaps; discussion about efficiency when many stackmaps are
  shared (debug metadata use-case)
  - can we do sharing to avoid the quadratic overhead when we have many live
    values and many safepoints? Debug metadata does this in a specific way for
    its needs: localsshape is stored once per function, and stack shapes are
    deduplicated
    - conclusion: yes, "bundle" of stackmap slots we can refer to

- alexcrichton:
  - fuzz failures between differential execution and native in
    cranelift-fuzzgen -- implemented libcalls with same backend (Rust stdlib)
    to avoid mismatches
  - segfault due to single-pass regalloc + exceptions -- question about how we
    document support tiers and what is or isn't covered by
    security-vulnerability disclosures. Conclusion: we should document which
    config knobs are covered somewhere.
