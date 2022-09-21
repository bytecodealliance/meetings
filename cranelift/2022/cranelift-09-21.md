# September 21 project call

**See the [instructions](../README.md) for details on how to attend**

## Agenda
1. Opening, welcome and roll call
    1. Note: meeting notes linked in the invite.
    1. Please help add your name to the meeting notes.
    1. Please help take notes.
    1. Thanks!
1. Announcements
1. Other agenda items

## notes

* Meta-logistics:
  * No predetermined agenda this week
  * This meeting went from biweekly to weekly, are we ok with weekly?
    * Consensus of attendees: everyone is fine with weekly.
* At least one PR open from someone at ARM, should do review of waiting PRs

### Attendees

* abrown
* afonso360
* avanhatt
* bjorn3
* cfallin
* elliottt
* fitzgen
* jameysharp
* uweigand

### Status

* uweigand: 
    * bitcast PR, agree with feedback but haven’t had time to finish followup.
    * regalloc rework, getting rid of mod (?) registers. One blocker might have been fixed by cfallin's merge yesterday. 
* cfallin: 
    * Lots of work on regalloc2. Allocator now supports constraints in a more flexible manner, call side of ABI update works. 
    * Worked with elliottt on overlap checking for ISLE, backend work to change priority handling. More complicated than anticipated, dropped that PR for now.
    * Working on several PRs to land e-graphs work, piecewise. Updating to not use recursion. 
* avanhatt:
    * Refactored verifier to use explicit width variables, think this allows us to handle more cases.
* elliottt:
  * Working on ISLE extractors in prelude, `And` term construction in building the trie (correctness with respect to overlap checking, not just performance). 
    * Discussion on semantics of `And`: does the language allow `And` terms to be reordered during compilation? Note: this does not affect correctness WRT a single rule. 
    * Overlap checker will (currently) say: there is some order for which no overlap occurs. Maybe what we want is a conservative overlap checker.
    * There is a case where the overlap checker correctly says no overlap, but the trie is wrong.
* jameysharp:
  * Some covered already in overlap checking work. 
  * PR merged Monday trying to fix quadratic behavior in Cranelift frontend, doesn’t account for set of preds to basic block changing. fitzgen to revert that PR in the meantime before fix lands.
* bjorn3: 
  * updating cg_clif to new version.
* afonso360: 
  * Looking at why dividing by 0, trapping so much. PR to fix.
* abrown: 
  * N/A
* fitzgen:
  * Working on compiler speed, building arena-based BTrees for regalloc2. Forked std lib BTrees, threading through arenas, should be ready to test soon.
  * wasmtime 1.0 release and announcement yesterday!