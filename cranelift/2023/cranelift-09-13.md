# September 13 project call

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

- afonso360
- fitzgen
- tshepang
- alexcrichton
- elliottt
- jameysharp
- saulcabrera
- cfallin
- avanhatt

### Notes

- alexcrichton: security release on Wed: x86-64 SIMD shift instruction
  miscompile
  - why didn't fuzzing find this?
    - wasm-smith's means of matching up types: dropping values rather than
      using them; alexcrichton changed to add all values into a global
    - oss-fuzz only runs some targets on any given day
      - PR to wasm-tools to create one giant target
      - maybe do this for wasmtime?
  - cfallin: should all incorrect-guest-value bugs be CVEs, or just sandbox
    escapes?
    - fitzgen: process working as intended, gets folks thinking?
      - cfallin: in one sense yes, but has real costs as well, seen in practice
        with this last one (followup discussions needed to clarify no escape)
    - concerns of "alarm fatigue" if every compiler bug is a CVE
      - is a SIMD shift-by-constant off by one as critical as an addressing bug
        that allows a sandbox escape? Thought experiment: FP rounding modes?
    - potential "sandbox escape only" alternative: bugs in compiler ops that
      lead to address computation or control flow
    - RFC for this to get community's input + define an explicit policy we can
      point to 
- updates
  - afonso360: RISC-V compressed instructions, TLS relocs
  - cfallin: no updates
  - jameysharp: no updates
  - tshepang: no updates
  - saulcabrera:
    - Winch: implementing tables support; discussions with Trevor on other bits
  - alexcrichton: BMI2 x86 extensions
  - elliottt: added some FP ops to Winch
  - avanhatt:
    - working on camera-ready (final) version of verification paper
      (VeriISLE) on paper
    - working on parser updates to get work upstreamed/merged
  - fitzgen:
    - WasmCon: talk about security + correctness in Wasmtime (same as blog
      post)

- elliottt: looking at some regalloc2 perf questions
  - redundant moves generated by regalloc2
    - conclusion: second move was a jump target
  - move from one reg to another, then back to original
    - redundant move eliminator, priorities, ...
    - want to remove redundant move eliminator eventually; but needs more
      thought
