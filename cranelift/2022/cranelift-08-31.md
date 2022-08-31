# August 31 project call

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
    1. ["Winch" baseline compiler](https://github.com/bytecodealliance/rfcs/pull/28): factoring/sharing of Cranelift's machine-code layer
    1. _Submit a PR to add your item here_

### Attendees

* cfallin
* fitzgen
* uweigand
* akirilov
* tshepang
* bjorn3
* eliottt
* alexcrichton
* jlbich
* abrown
* jsharp
* saulecabrerra
* afonso360

### Notes

#### Winch

* saul: Posted an RFC, high level overview + motivation, Shopify very interested
  in this. One idea is to reuse the code emission parts of Cranelift for Winch.
* chris: Hypothetical refactor is to reuse the `MachInst` definitions and
  `MachBuffer` and a few related pieces. Split those out into a separate crate
  (`cranelift-asm`?) to use in Winch as well. Nothing should change too
  significantly. Requires ISLE compiler to be used for both crates and will
  split ISLE files. Hopefully not too big a problem overall. Concerns?
* johnnie: Is this the first step in a tiered approach or a different
  configuration?
* chris: Tiering out of scope for now.
* saul: Yes don't want to tackle complexities. Plan is to have two separate
  compilers.
* johnnie: Use cases for this style of compilation for Shopify? Or just in
  general and see where it goes?
* saul: Specific use case of extending checkout processes and execute wasm
  inline. Currently we precompile wasm but requires keeping artifacts up-to-date
  and would prefer to only rely on wasm binaries and have fast start times.
* andrew: With `cranelift-asm` users could create a list of instructions using
  `MachInst` enum variants to emit?
* chris: Thought to model after the macro assembler in SpiderMonkey. API with
  methods for each instruction and calling methods creates the code, simply
  building machine code as you go with affordances for branch targets. Perhaps
  similar to `dynasm`.
* andrew: Will this change to have the abstraction at the instruction level
  instead of the machinst level?
* chris: Could start with generated straight from the machinst arms.
* andrew: Using machinst historically would have been more helpful for the
  abstraction to be at the instruction level instead of figuring out which
  variant needed.
* ulrich: Enum values may want to be refactored and exposing them to external
  users will have to have everything get changed. Instructions in an ISA won't
  change though.
* chris: Copatibility concern?
* ulrich: yes
* chris: Good point but is somewhat orthogonal. Intra-repo deps are currently
  internal. Should be ok for us and for external users we could say instruction
  constructors are stable and well-defined.
* nick: Can consider this new crate still an internal crate and Winch part of
  the internal family.
* anton: Saw Chris's refactoring of machinst for AArch64 and fine for Cranelift
  but a public interface for all kinds of users should be as close as possible
  to the ISA semantics or could otherwise be confusing for anyone reading ISA
  docs.
* chris: Good point, x64 backend had constructors that "undid" SSA-ness. Could
  do the same thing for the instruction-level API. MachInst has to be public at
  the crate level be used by cranelift-codegen but could document that it
  shouldn't be used.
* anton: True mostly whatever users are using should be as close to the ISA as
  possible.
* chris: Saul for the refactor probably want to write these constructors by
  hand to make it easy to use like the ISA.
* trevor: Lots of helpers in ISLE which are basically what you want. Could
  possibly help avoid duplication.
* chris: Perhaps a trait? Like `InstBuilder` which invokes instruction API and
  either goes to `MachInst` or straight to buffer.
* andrew: In the RFC copy & patch was brought up, that seems like a good idea?
* chris: We don't want to do want now and could be a good source of ideas in the
  future. Different enough from what we want to do though. Understanding is that
  C++ template language generates gadgets compiled by GCC/Clang and then
  annotate the machine code with fixup records and glue it all together. Very
  significantly more complex than what we probably want. Feels like it's more
  than what we want to do by pattern-matching more combinations of ops. We want
  something that's direct-from-wasm though.
* nick: Need to build your own linker cutting out epilogues and prologues as
  well.
* chris: Appeal of the baseline compiler is that it's fairly simple to look at
  and prove correct. Trying to avoid complexity and surgery on machine code to
  get to the basic implementation.
* saul: My measurements internally were taken for SpiderMonkey and Liftoff which
  are both relatively straightforward. Those measurements are good enough for
  our use case and is rationale to not deviate.
* andrew: Makes sense not trying to add complexity. This sounds like simplified
  copy and patch though and wondering if there's a way to still have minimal
  complexity but have the possibility of more complex patterns. Nick's point
  about the linking seems like the big issue though.
* nick: Matching multiple expressions and having macro-ops is something we could
  do in the future. Compiling C++ gadgets is the hairy bits.
* andrew: Lightbeam 2.0?
* chris: Sort of, more traditional though. Not trying to innovate.
* andrew: Where did Lightbeam diverge?
* nick: Mostly changed focus and the maintainer stopped working on it. They
  wanted to port SpiderMonkey's baseline compiler to Rust, similar to where we
  are with this RFC. They also wanted to make optimized machine code with linear
  compilation time, which while interesting was a very different use case with
  lots of open questions. Became a different project despite starting in the
  same place.
* johnnie: Looked at Lightbeam originally and was hard to debug and understand.
  Had a different way of creating instructions with templates (?) which made it
  more inaccessible.
* chris: Goal is to have something that is simple and straightforward.
  Eventually want new instructions to be implemented in winch as a requirement.
* nick: Wasmparser was recently refactored with a different API for validation
  and Winch can ideally "just" implement that trait. Should help here.
* saul: Motivated by speed?
* nick: Less dispatching yeah.
* anton: On modified registers, not against having a form with separate
  destination and source where assembler adds moves for convenience. With
  Chris's patch we have both params and assert the same but that's not a great
  public interface.
* chris: That's a regalloc interface thing where the assertion only makes sense
  after regalloc.
* anton: Thinking about SVE many instructions are destructive but ISA defines a
  move code prefix where moves one of the sources to the destination which feeds
  into opcode. Processors supposed to fuse into one micro-op. For
  macro-assembler it could have a constructive form and generate normal form or
  move-prefix form.
* ulrich: Everything below MachInst, emit part, is to be moved out?
* chris: Yes
* ulrich: Places where machine emission has environment or context requirements,
  things like relocations or constant pool accesses. Things like GOT or PLT or
  TLS. This'll all reside in new crate too?
* chris: Planning on either being a straightforward cut or if needed have some
  trait abstractions for possible circular dependencies.
* ulrich: One violation I see is where we need a single machinst that creates
  multiple instructions because we can't insert branch labels. They won't make
  sense in this cranelift-asm crate so may make sense to solve underlying
  problems.
* chris: Been thinking about this as well in refactorings. Option where we could
  keep those but not expose in the assembler API but there for Cranelift. Not
  great but an option. Basic reason why we can't expand control flow is how we
  transfer over block IDs, could take another look though.
* ulrich: Does it have to be a full block or could we just have labels?
* chris: potentially!

#### Status updates

* trevor: x64 is fully ported to ISLE! Will continue with a minimum viable
  overlap checker for ISLE driven by x64 work and discovering there were some
  cases where rules fired unexpectedly. Will develop analysis to find those case
  and will help drive thoughts of how to address them.
* ulrich: Getting all patches upstream for s390x in cg-clif. Everything upstream
  in cranelift and two minor patches in rust codegen. Doesn't work yet as we're
  waiting for release processes, but should just work afterwards. Passes full
  test suite. Looking again into bitcast vs raw_bitcast. Been investigating
  big/little endian bits and summarized all findings in issue.
* nick: Working on a blog post for Wasmtime 1.0 related to correctness and
  security. Does touch a lot on Cranelift and if anyone's interested in review
  let me know. Looking into profiling compilation again and put up a patch for
  1-7% speedup by cleaning up and deduplicating ABI signatures. Should be more
  room now to take advantage of arenas. Plan on looking into making an
  arena-based BTree for regalloc2.
* jamey: Fuzz bugs! Thanks to Afonso for improvements to cranelift-fuzzgen.
* saul: winch!
* nick: Is winch happening in Wasmtime?
* saul: Will be in Wasmtime, doing an MVP right now to exercise data structures.
  Once that's ready will submit a PR. Idea is to integrate that an then interate
  in-tree.
* afonso: Fuzz bugs! New capabilities to fuzzgen as well. The fuzzgen target is
  getting slow at 15/s, looking to add stats reporting.
* chris: Is anything dropped that doesn't verify?
* afonso: Try to get most everything past the verify, but testing parts the
  verifier drop as well. Try to test verifier sometimes but not drop much. Can
  also fail in the interpreter.
* chris: In regalloc2 it worked to by construction not drop cases. Perhaps have
  a mode to avoid instructions interpreter doesn't support?
* jamey: Works that way not. Interpreter can fail due to traps right now.
* afonso: Can also fail for things like loading addresses outside of the stack.
  Want to test that the interpreter fails as well though.
* nick: Can use `Unstructured::ratio` to make things possible-but-rare.
* jamey: Does that work with coverage fuzzing?
* nick: Still is important it seems. Theoretically no though.
* alex: ... many words about fuzzing and wasm-smith and such ...
* andrew: Fuzzing! Working on the differential fuzzer.
* johnnie: Fixed a fuzz-bug for float-to-int conversion. Working on github
  actions-based benchmarking. Should have PR updated soon.
* anton: ISLE cleanup. Moving AMode which had a lot of changes due to ISLE only
  supporting named fields in enums. Also fixed atomic instructions for
  respecting flags. Working on updating forward CFI patch, determining which
  basic blocks are indirect branch targets. Working through code still.
* chris: Companion blog post to Nick's. Lin will also write a 1.0 post, I'm
  writing one about performance. Let me know if you'd like to see a draft. Doing
  a lot of paying down tech debt, cleaning up regalloc semantics. Pinned vregs
  require moves to encode constraints and shifting backends to operand
  constraints instead which cleans things up. Landed x64 first and AArch64 is
  in a PR with s390x queued up next. Helping to push AArch64 ISLE over the
  finish line. E-Graph RFC in FCP and going to merge next week (please leave
  comments!). Starting to build a cleaned up patch series to land.
