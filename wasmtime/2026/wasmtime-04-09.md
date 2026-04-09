# April 09 | Wasmtime Project Bi-Weekly

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
   1. Find volunteer note taker. Thanks!
1. Announcements
   1. _Submit a PR to add your announcement here_
1. Other agenda items
   1. fitzgen: what quality-of-implementation issues do we consider blockers to shipping GC by default?
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2026-03-26)
   * [Old, Backlog Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aissue%20state%3Aopen%20sort%3Acreated-asc)

## Attendees

- Nick Fitzgerald
- Paul Osborne
- Alex Crichton
- Tolumide Shopein
- Chris Fallin
- Pat Hickey
- Victor Adossi
- Yordis Prieto

## Notes

### Releases / LLM Security

- Nick: Several security releases going out today.
- Alex: 12 Advisories
  - Blog post today
  - LLM tool assisted bugs found most of these, many panics finding DOS
  - May want to require tier1 LLM review requirement; difficult to define
    exactly what this looks like
  - Several in favor; how public do we be with the prompt? Probably no special
    sauce there, model access might be for a window.
  - Funding/Pricing has open questions -- generally, we think we can find
    solutions and it won't be a problem.
  - Copilot for external contributions?
    - Better model is often reviewer running an LLM on the change
      - Probably don't want this as a mandate at this point.
    - Copilot is noisy, false positives.
    - Till: maybe have a CI job that separately posts review feedback maybe?
    - Nick: want to be careful about contributions modifying prompts as an
      attack vector or burn tokens.
    - Pat: maybe consider this as more of a fuzzing thing; running in the
      background discretely when vulns are found.
    - Chris: there are real questions on token budget per PR or somoething.
    - Alex: we can't avoid these tools given the concrete findings, but lots of
      open questions.
- Till: to explore some options with Alex for what to put in tree to faciilitate.
- Focus on validation of options turned on by default.
- https://bytecodealliance.org/articles/wasmtime-security-advisories

### Fuzzing

- Till: have an early working implementation based on the rgfuzz paper.
- Till: LLM used to improve fuzzers; probably a fruitful area for future
  improvement.
- Till: Aarch64 fuzzing seems more critical again; no easy solution like with
  x86 (via OSSFuzz), probably need to go with Hetzner or similar infra. We had
  waffled on this and formal verification for Tier1, revisiting Till feels like
  we probably should require fuzzing.
- Till: There's enough gaps that can't be verified formally that we should fuzz.
- Alex: Infra separate from OSSFuzz (or other service) is probably required or will quickly fall into disrepair -- we really would like a managed solution like OSSFuzz (which doesn't support aarch64).
- Chris: or it would need to be someone's job.
- Till: check in with Google to see if they may be able to assist.

### CVEs Classes

- Alex: two classes
  - Winch
    - Fuzzers probably should have found; they were told that winch doesn't support reftypes entirely.
    - Winch supports _some_ reference types.
    - Ended up with gaps around table fill
    - Nick: might be able to relax compilation signals for winch (allow failed compilations for variants)
  - Component Model Encoding
    - There are some fundamental problems around string transcoding that we should rewrite
    - There are gaps in fuzzing around "invalid" cases
    - Probably replace libcalls for components with less privileged alternatives.
- Wasmtime 42 will go out first, then backports to follow shortly.

### Host Heap Tracking

- https://github.com/bytecodealliance/wasmtime/issues/12984 up for review
- Generally, approach makes sense.

### GC By Default Discussion

- https://docs.wasmtime.dev/stability-wasm-proposals.html
- Nick: Looking at steps to move toward proposal stabilization.
- Nick: There are gaps in host reosurce usage now.
- Nick: Issues remain around funcref interactions via hostcalls.
- Nick: Performance issues remain, but probably not a blocker?
- Chris: In favor of semispace collector. For production, want/need something
  that collects cycles, even if otherwise primitive.
- Alex: CVE free is key.
  - Host memory consumption / DOS is of concern.
  - Audit against potential excess resource consumption in malicious case. On
    board with semispace collector.
  - Not as concerned with funcref case.
- Chris/Nick/Alex: Self-hosted solution is interesting but creates some
  problems, would probably always involve some host work; focus more on
  sandboxing fixed point loop and allocator, etc. Don't fully block on this.
