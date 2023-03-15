# March 15 project call

**See the [instructions](../README.md) for details on how to attend**

## Agenda
1. Opening, welcome and roll call
    1. Note: meeting notes linked in the invite.
    1. Please help add your name to the meeting notes.
    1. Please help take notes.
    1. Thanks!
1. Announcements
    1. The `wasmtime explore` command exists!
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. _Submit a PR to add your item here_

## Notes

### Attendees

 - fitzgen
 - elliottt
 - afonso360
 - jameysharp
 - tshepang
 - cfallin
 - avanhatt

### Notes

- fitzgen: `wasmtime explore` tool exists: like Compiler Explorer/godbolt.org, lets you see HTML-formatted information.


Status updates:
- jameysharp: want to start a conversation for folks both on Cranelift and Wasmtime about a process for triaging new issues and PRs. Imagining: pick a random person from a set of selected reviewers, responsibility to decide who it should be assigned to (not to fully review themselves). cfallin was defacto doing this for a long time, but agrees this is a good idea. jameysharp to generate the initial list and set up the auto-assign. 
  - Related: Github has a “code owners” feature, we could do that at a more fine-grained level, e.g., x86 vs. aarch64. fitzgen remembers there being some flaw with how Github does code owners related to their use case (fitzgen to ask Till if he knows what it was). 
  - Related: Jamey knows of an analysis where Rusts’ `highfive` bot had a measurable positive improvement on issue handling in the Rust repo. 
- afonso360: adding ISA extensions to the fuzzer. 
- tshepang: N/A
- elliottt: constraining function types to use in the function generator, regalloc
- avanhatt: looking into running the ISLE verifier on x86 addressing mode rules, finishing solver implemententation tasks for aarch64 rules.
- cfallin: working to get veri-wasm reintegrated (updated for Cranelift/Wasmtime instead of Lucet). 
- fitzgen: working on wasmtime explore. Also added support for “default values unused import” wasm flag, returns default values for unavailable functions. Useful for using wasmtime cli to run sightglass benchmark programs.


