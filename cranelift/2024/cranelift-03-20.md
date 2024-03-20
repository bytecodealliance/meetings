# March 20 project call

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
    1. jameysharp: we should really determine what optimizations are legal for
      `select_spectre_guard`, as it would be really nice to be allowed to constant
      fold them

## Notes

### Attendees

- saulcabrera
- elliottt
- alexcrichton
- fitzgen
- jameysharp
- abrown
- lafp
- uweigand
- cfallin

### Notes

- jameysharp: `select_spectre_guard` legal opts: mostly covered in GitHub
  issues; any concerns remaining?
  - summary: key requirement is that it prohibits all speculation on
    controlling (condition) input. This allows constant-folding because we
    statically know the condition (no speculation).
  - alexcrichton: what's different between this and regular select?
  - cfallin: if we had a speculate-on-value optimization in the future it
    would; for now no difference
  - alexcrichton: why can we rely on no value speculation?
  - cfallin: processor vendors say this is safe (Spectre whitepapers etc),
    maybe with details like `csdb` on aarch64
  - some Spectre things remain; e.g. indirect branch targets
    - cfallin: lfence-before-branch triggered by monitoring could be an answer
      (recent issue filed)
  - abrown to share Spectre whitepapers
    - https://www.intel.com/content/www/us/en/developer/articles/technical/software-security-guidance/technical-documentation/analysis-speculative-execution-side-channels.html
  - alexcrichton: what about hostcalls? they do bounds-checks everywhere
    - we should probably add mitigations to these as well

- updates
  - fitzgen: working on wasm-gc stuff, next PR will have some changes for `cranelift-wasm`
  - lafp: working on aarch64 backend for Winch!
  - uweigand: no updates
  - abrown: no updates
  - jameysharp: callee-saved registers in tailcall calling convention
    - working on using parallel move resolver for setting up new stack frame
  - alexcrichton:
    - high-quality fuzzbugs coming in with differential fuzzing between
      architectures; NaN canonicalization, etc
  - elliottt: callee-saved regs on tailcalls; switching Winch over to Cranelift
    trampolines
  - saulcabrera: Winch fuzzbugs; working on address maps for sourceloc to PC;
    planning to experiment with Sightglass and look at perf
  - cfallin: PCC; worked on dynamic bounds-checking, turns out to be pretty
    hard to make scalable in the face of optimizations; opted to turn on static
    only for fuzzing; fixed some fuzzbugs

- elliottt: tail-calls and callee-saved registers: rather than building new
  frame and copying it in, building a list of `Allocation`s (reg or stackslot)
  and where they need to go
  - (lots of discussion)
  - one intermediate option: put clobbers in the new frame and copy them up
    with the new args
    - let's do that at first to get everything onto tailcall convention
  - eventual solution: new kind of Allocation for RA2 that means "this position
    in stackframe"; let RA2 overwrite its frame as a last action; pushes all
    this complexity down into RA2

- Lowering of externrefs (r64s) and conflicting constraints on bitcasted i64
  from r64 passed as arg
  - ensure bitcast becomes an actual move (copy) instruction

- table lowerings (Wasmtime): 
  - tricks with function pointers to trampolines: issue is that the original
    table index isn't passed through to the point that we might actually call
    the funcref (and what if we store it elsewhere; need to capture)
  - granularity of initialization: page vs individual
  - non-exported tables with custom section noting readonly, and trampolines
    for imports?
  - (lots of discussion of trampolines, various edge cases, ...)
  - settling on: always same representation; two questions:
    - lazy vs. eager init; if eager, can remove the branch that checks
    - init pattern of all zeroes vs all ones, and options for initialization
      (memset vs map pages of all `1` usizes vs...)
