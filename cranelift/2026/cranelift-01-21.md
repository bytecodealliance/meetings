# January 21 project call

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
- jlbirch
- mmcloughlin
- jlbirch

### Notes

Updates
- alexcrichton: no updates
- mmcloughlin: user-controlled in ISLE rules -- after 12333, looked for
  additional user-controlled recursion; filed issue, Alex fixed
  - ISLE verifier can find recursive cycles
  - cfallin: remaining cases -- bounded how?
    - bounded by e.g. reducing one type to another
    - cfallin: so parameter space is finite and we reduce within it; could
      reason about this but annotation with "safety comment" is probably easier
  - alexcrichton: we should maybe look at these bounded cases (e.g.
    `splat_const`) and see if we can move to mid-end, or split by types
    - fitzgen: one can always stratify if it's already finite
- fitzgen: thinking about e-graph extraction and cost functions
- cfallin: also thinking about e-graph extraction and cost functions; good
  discussions with Alexa and Nick; some experiments; no real wins yet (a few
  speedups e.g. bz2 by 4-6% but also compile time cost of 1-2%); hard to avoid
  NP-complete traps, dynamic programming approach we have currently can
  probably best be extended a la Nick's prototype that keeps sets, question is
  whether we can do better on data structures. No real urgency -- a few "cost
  overflow" cases but no huge code quality impacts with current `main`

- cfallin: Arrival upstreaming -- any further thoughts on upstreaming timeline?
  - mmcloughlin: hoping to find time this semester; Alexa may get to it first
