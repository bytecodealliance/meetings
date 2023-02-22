# February 22 project call

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

- elliottt
- cfallin
- uweigand
- afonso360
- jameysharp
- avanhatt

### Notes

No agenda, status updates:
- Afonso: working on the interpreter, codegen for RISCV, fuzzbugs from RISCV hardware
- Alexa: verification engine uses solver for widths now, can pretty-print found counterexamples back to an ISLE-like syntax, 
- Chris, Ulrich, Trevor: nothing Cranelift-specific
- Jamey: looking at changes in block lowering using the reverse dom tree. Opened PR for changes to br_table on x64, curious to do performance testing. This might let us do midend optimizations on bounds checks.
    - Chris: Clif is already unsafe, have raw loads and stores. Chris looking at interpreter performance; could be nice to not bounds check addresses that we already know are in range. Related: switch is slow on x64, even slower on aarch64 (maybe because of Spectre mitigations). 
    - Other Jamey thought: could pad out jump tables to e.g. 256 instead of doing bounds checks.
    - Other Chris thought: could put minsize on jump table in the instruction (e.g. 256), the backends could be responsible for filling in padded cases with default case. 

Topic from Zulip: branch optimizations. 

- Jamey: would like to be able to write branch optimizations in ISLE, but figuring out how to express this is nontrivial. Can’t do in aegraph formulation because branches don’t have values. 
    - Clarification: would this be a new ISLE prelude etc just for branches? Answer: no. Rewrites may want to introduce new nodes that *would* be part of the aegraph. Natural division: do instructions have results or not? Idea: use prelude_opt for instructions with exactly one result, something more like prelude_lower for instructions with no results. Same context, two entry points: simplify, simplify_branch. 
    - Chris point: bidirectional dependence between branch opts and other opts. You want them in the same fixed-point loop. 
    - Plumbing questions: 
        - How do we make the extractors in particular work? They will see e-classes as args. 
        - Right now, can’t call multi-extractors from non-multiterms. Might need to change that. Concern with reverting this: non-determinism. Maybe fine if we make clear that this could be unsafe/the author is ok with any option. 
        - Trevor: similar to nondeterminism in Haskel monadic cases (?). Instead of artificially picking the first one, instead have something like a predicate to choose between. Could be called “choose”: predicate + multi-extractor. 
        - Jamey: what would we want that choice to be for this specific case? Have e-class, calling multi-extractor to ask if any nodes in that e-class match the pattern. The result we want: the newest value in the e-class if we matched the pattern. 
        - Implementation detail about Clif’s e-graph: not just an e-class, but also state of e-class at a particular point. Important for acyclicity. In multi-extractor: looking back through the history of a particular e-class. We want to say we matched the current version, based on something in its history. 
    - Patterns we will actually write for something like branch: 
        - branch-if-constant -> folded to unconditional branch. If multiple constants, they better be the same constant. 
        - br-if where both arms are the same -> unconditional jump 
        - br-table with empty table -> unconditional jump to default
        - optimization over conditionals: input to branch is comparison to constant that is 0 or 1 -> branch if value or branch if value with arms inverted
        - other case: extend of icmp to a branch, generated a lot by wasm.
    - When we apply the rules, do these successively apply? 
        - Other Jamey thought: thinking about composition as a feature of the ISLE input language. Might want: the ability to say: whatever result this term rewrites to, see what happens if you provide it as input to the term again, expand that statically, require that it terminates. Could also do for aegraph-simplify pass, instead of recursion, statically match all expanded patterns. But, the intermediate state doesn’t stick around for extraction? Efficiency advantage: avoid re-checking the patterns. 
        - Could this help with commutativity? Incremementalist bias: instead do the alternative we’ve discussed of having a flag for commutative that generates both/all variants of the rules. 
