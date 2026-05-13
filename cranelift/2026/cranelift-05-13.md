# May 13 project call

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
- Rahul Chaphalkar
- jlb6740
- cfallin
- Jacob Denbeaux
- thejimmybrisson

### Notes

Updates

- jlb6740: no major updates. Team is making plans for Q3; want to work on Intel
  APX.
- Jacob Denbeaux: curious to learn how other compilers incl. Cranelift work!
- alexcrichton: no major updates
- cfallin: three things on plate to look at
  - MachBuffer deferred-traps out-of-range issue (12489)
  - egraphs rewrite fuel
  - spidermonkey-json.wasm regression issue
- Jimmy: no updates
- Rahul: no updates
- fitzgen:
  - idempotent store elimination landed
    - may not be quite enough for component-model adapters in Wasmtime as
      currently done: we set a bit then clear it; would need to analyze to show
      that no one else reads
    - Jacob: interesting viewpoint from ZJIT: also worrying about control-flow
      effects and dead-store elimination
  - added ability to simplify (fold) conditional branches in
    `simplify_skeleton`
  - benchmarking slowdowns in generic alias regions branch: made MachInsts
    bigger. Removing that removed the slowdown!
    - cfallin: 16-bit alias region limit matters for large functions?
    - fitzgen: can always wrap around

- cprop slowdown issue
  - want to understand the interactions first before we jump to conclusions
    about deleting rules; maybe a bad cost function interaction or something?

- simplify-conditionals
  - alex: can we factor out the logic? lots of duplicated simplification of
    conditions for operators that take conditionals (select, trapnz, brif, ...)
    in the backends to fuse together compares with consumers
  - cfallin: yes, `simplify_condition` in mid-end; start from what ggreif's
    recent PR did in allowing `simplify_skeleton` to simplify brif
    conditionals, and wrap the simplifier logic for that conditional in a
    helper, and invoke it from rules for icmp, trapnz, etc

- egraph fuel
  - cfallin: plan is to meter by LHS multi-etor matchers; that cuts off loops
    in the generated ISLE code once iterators start returning `None`
  - alex: run out of fuel in one path; correctness? 
    - cfallin: no worries here: always valid to have fewer or no opt results
    - cfallin: heuristics-wise, this is a Hard Problem (how to direct effort)
      but we could have pragmas or ...; not at first, goal is just to
      contain/sandbox the blowup
  - fitzgen: rulesets in egglog are a nice way to separate rules into
    higher-and-lower-priority sets; maybe do similar
