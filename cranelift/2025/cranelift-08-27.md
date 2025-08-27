# August 27 project call

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
- erikrose
- abrown
- cfallin

### Notes

- Updates
  - alexcrichton: no updates
  - erikrose: no updates
  - abrown: no updates
  - cfallin:
    - exceptions landed on Wasmtime side; no Cranelift-specific
      updates
    - d_sonuga updated fastalloc to support constraints needed for
      exceptions; pulled back in; then fuzzbug so Alex disabled again
  - fitzgen:
    - fixed one more inliner fuzzbug
    - shout-out / thanks to Bongjun Jang for submitting new mid-end
      rules with correctness proofs and runtests

- ABI and softfloats (`x86_64-unknown-none`, etc)
  - https://github.com/bytecodealliance/wasmtime/issues/11506
  - the "none" targets are softfloat; see issue
  - do we want to add an option to disallow libcalls/emit error on
    these targets?
  - cfallin: softfloat requires we don't use hardware XMMs at all --
    is this worth building out?
  - should we not support the `none` target? should we fail only if no
    SSE (so allow the registers still, just not ABI boundaries)?
    - fitzgen: seems like a lot of work for niche use -- compile-error
      seems right
  - alexcrichton: disallow loading `x86_64-unknown-none`-targeted code
    unless you set a config option saying hardfloats actually turned
    on (Rust doesn't have a way to detect it statically)
