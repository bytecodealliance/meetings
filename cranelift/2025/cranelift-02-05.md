# February 05 project call

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
- fitzgen
- cfallin
- Rahul Chaphalkar

### Notes

Updates

- Rahul: no updates
- abrown
  - initial Cranelift assembler merged; want to fuzz it, not sure how to get it
    running in oss-fuzz
    - fitzgen: we've factored fuzz targets so everything is in one target
- cfallin: no updates
- erikrose:
  - orientation tasks
    - PR to try to make GVN normalize argument order on commutative args;
      learning how to benchmark
- fitzgen:
  - investigating some GC bugs that were reported that involve Cranelift
    optimizations; not CL bugs, just generating too-optimizable CLIF


Topic: perf optimizations as starter issues?
- fitzgen: Souper-generated superoptimizations
- cfallin: profile and see where issues might be, bottom-up
- cfallin: at least historically, thought has been that for Wasm workloads,
  Wasm coming in is already somewhat optimized; regalloc is the biggest lever;
  regalloc3 would be the easiest thing to reach for there
- fitzgen: GC is starting to make the semantic gap between Wasm bytecode and
  CLIF wider; optimizer might become more important
- cfallin: inlining could become more important with multiple Wasm modules
  (components)
