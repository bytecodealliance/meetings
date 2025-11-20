# November 20 | Wasmtime Project Bi-Weekly

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
   1. Tier status of threads proposal https://github.com/bytecodealliance/wasmtime/pull/12036 (@alexcrichton)
   1. Fuzzing methodology for exceptions (https://github.com/bytecodealliance/wasmtime/pull/12037#issuecomment-3543614553)
   1. OOM-friendliness in embedded environments
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)

## Attendees

* Alex
* Roman
* Till
* Chris
* Victor
* Nick
* Joel

## Notes

- Tier status of threads proposal
  - Alex: prompted by CVE.  Technically threads is tier 1 and on by default.  Want to downgrade to tier 2.
    - It's not at level of testing, quality, commitment that justifies tier 1
    - uses wasi-common which is largely unmaintained
    - but still want to leave on by default.  Want new category of features that are on-by-default but not tier 1
    - exceptions should probably be in this category, too; stable, proposal not changing, want to dogfood
  - Till: should disable it, not just demote to tier 1
    - nobody reads about tiers
    - it's a footgun to enable things like this by default
    - threads proposal is not stable nor has a clear future
  - Nick: it's in spec?
    - Alex: no, still in phase 4
    - Nick: morally phase 5, though
    - Till: quality of implementation still a problem
  - Nick: agreed should not turn things on by default if not tier 1 given user default behavior
    - Willing to make exception in this case
  - Chris: if exceptions good enough to be on default, should consider tier 1
    - If it's popular, will have security reports anyway
  - Alex: agreed
    - One counterpoint: will be pressured to turn things on before implementation is ready due to popularity
      - Maybe that's virtuous about directing efforts, though?
  - Till: have rules about tier 1 to avoid jeopardizing users
    - turning feature on by default without meeting those criteria would be bad
  - Alex: will both disable threads by default and demote to tier 2
  - Till: what error message will they see if they try to use threads without enabling?
    - Alex: nobody uses it anyway, but maybe atomic instructions could be produced by popular toolchains
    - Chris: comes up when porting popular libraries like protobuf
      - Alex: llvm should be lowering atomics to non-atomic when singlethreaded
      - Nick: maybe people enable things without thinking about them
      - Till: added flag to enable atomics to wizer because there was a use case
      - Nick: we could allow instructions in wasmtime but don't create shared memories?
        - Alex: use validation-time check that no shared memory but enable everything else
          - precendent for getting subset of e.g. gc
          - Nick, Chris: thumbs up
        - Alex: engine-level disabling of shared memory
          - leave wasm proposal enabled by default, but make shared memory tier 2
          - Nick: two knobs, right?  Alex: yes, one for threads, another for shared memory
- Fuzzing for exceptions
  - Chris: noticed red Xs for exception fuzzing and C API
    - currently fuzzed in sense that everything is fuzzed
    - Alex: no V8 differential test coverage for exception?  Chris: yes, we have that
    - no gc-style torture tests yet
      - Should that be part of the standard for fuzzing?
      - Want to do that in this case, but what's general standard?
        - Nick: current rule: if you think wasm-smith is good enough, fine, but otherwise should add tableops fuzzer for in-depth fuzzing
          - depends on security implications
          - should we change the rule?  Probably not; we're smart people who can make informed decisions
          - for exceptions, does make sense
          - another option: extend existing stack-trace-checking fuzzer
          - Alex: agreed, keep case-by-case, and exceptions qualifies for extra attention
          - Chris: agree that stack checker is a good fit; will plan on that
    - Chris: remaining issues: C-API and gc interaction
    - Nick: make sure all instructions covered and any special case logic hit
- AI notes
  - Nick: generally in favor as long as someone proofreads before posting
    - and will take responsibility for doing that
    - don't like distraction of notes during meeting, but don't mind five minutes afterward
  - Chris: concerned about transcripts vs. capturing keep points, but should be covered by proofreading
- OOM friendliness in embedded envs
  - Chris: product folks at F5 say never crash on OOM
    - non infrequent in our env
    - must handle gracefully
    - make or break for using wasmtime
    - discussed with Nick: big lift
      - e.g. forking deps like anyhow
    - want temperature check
  - Nick: focused on data plane, not compiler
    - could classify parts of wasmtime as being OOM-safe or not
    - not proposing others do this work
    - but creates shared maintenance burden
    - have ideas about testing and fuzzing to check for regressions
    - are people willing to take on burden?
  - Alex: depends on what this entails
    - just adding some more `?` here and there is fine
    - worry that it's a lot more than that
    - how does verification work?
    - not worried about third-party code (not much in no-std parts anyway)
  - Chris: disallow box::new, vec::push, etc. in favor of fallible variants
  - Nick: clippy lints; will need to update them over time
    - also dynamic checks during fuzzing with custom allocator that does pseudorandom OOMs
    - must ensure good coverage
  - Alex: just need a good place to start
  - Nick: first, identify what we need to fix, then decide how to fix it
    - less sure about second part
    - idea: pooling allocator builder based on traits and combinators
    - other, complementarly idea: create extension methods like vec::try_push, etc. and add to prelude
    - regarding anyhow: have discussed no-std-friendly wasmtime error type
      - will still need escape hatch for wrapping arbitrary errors
      - enum of anyhow or oom to start with; oom requires no allocation or boxing
        - cost is loose original error
  - Roman: would this extend to wasi?  How does async work?
    - Nick: only care about already-no-std stuff to start with, then talk about async
      - Imagine latter can mirror Pat's wasi:io no-std work
      - probably no more tokio
      - F5 has own event loop
      - abstract away runtime with traits
      - maybe upstream stuff to futures, e.g. alternative alloc options
      - Alex: out of the loop with futures crate
      - Nick: will cross cm-async bridge after initial pass
  - Chris: place to start is core runtime only, no-std, p2-style async
    - pretty minimal deps
    - next step after that: p3
    - once we have that, will be popular for embedded scenarios
    - in a nutshell: only care about no-std stuff
  - Alex what parts to you need?
    - Chris: runtime, async, component model, gc
    - Nick: don't need compilation but do need cwasm deserialization
      - need something serde-like that's no-std-compatible
    - Alex: component-model-async stuff will be challenging
      - will soon become integral part of component-model
      - leaning heavily on futures-unordered, etc.
      - big lift to make it work in this mode
  - Nick: it's a major effort, but doable
    - as long as we can make precise cut and excise e.g. tokio, seems plausible
  - Chris: brings value for embedded users
  - Alex:
    - anyhow: want to feature flag it rather than fork or wrap it
    - internal code: wasmtime-collections crate -- give no access to infallible APIs instead of just linting
    - Chris: paraphrasing: e.g. custom, fallible HashMap, Vec
      - Alex: yes, where e.g. vec::push returns result
    - CI: would love to build without alloc, but that's not what we want anyway
      - Nick: lean on fuzzing
    - Would like a lint that denies alloc -- strict guarantee that everything uses wasmtime-collections, etc.
    - Alex: I was there for different ideas in Rust std for fallibility; tough problem
    - Alex: will need to change or add public APIs, e.g. store::try_new
      - Nick: config::new probably not a big deal
    - Alex: don't want to penalize ergonomics for all users
  - Nick: Rust4Linux blazing a path here; will only get better
  - Alex: can't do uninhabited error without feature
    - Nick: might need special set of features for fuzzing?
  - Alex: design fuzzing first
