# August 4th Wasmtime project call

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
    1. Proposal for a 1.0 release schedule. (Alex)
    1. [Baseline Compilation](https://github.com/bytecodealliance/rfcs/pull/28)
    1. Ruby bindings for Wasmtime (wasmtime-rb)
    1. _Submit a PR to add your item here_

## Notes

### Attendees

### Notes

- Agenda Item 1: 1.0 release schedule
    - Alex wrote [an RFC](https://github.com/bytecodealliance/rfcs/blob/main/accepted/wasmtime-one-dot-oh.md) a while back about criteria
    - Project has now met those criteria
    - Want publicity for release
    - Chris, Nick and Lin have volunteered to blog, etc.
    - Planning for Sept 20th release
    - 1.0 blog post planned for that day
        - Nick would like to see testimonials for production use
            - Shopify and Fermyon have volunteered
    - 2 other blogs in 2 weeks prior
        - First: calling out commitment to security
        - Second: performance -- summary of optimizations
            - instantiation speed, etc.
    - Blogs will be posted on bytecodealliance blog
    - 2.0 in October, 3.0 in November, etc.
- Agenda Item 2: [Baseline Compilation](https://github.com/bytecodealliance/rfcs/pull/28)
    - Saúl proposes dropping reference types and bumping ARM support per feedback
    - Nick says don't need full AARCH64 backend to be useful (e.g. don't need FP for EC2)
- Agenda Item 3: Ruby bindings for wasmtime
    - Shopify mostly ruby shop
    - Different teams have been wrapping wasmtime in different ways
    - Want to unify that and make official bindings
    - Alex says to start by using and mirroring C API -- Python bindings are good example to follow
        - Dealing with GC is challenge as always
        - Don't try to use Rust API directly
    - Nick would like to see it as bytecodealliance project, at least eventually
    - Chris says bar is low for creating repo
    - Maintainance shouldn't be difficult
    - May find C API is missing stuff, but that can be fixed
  
