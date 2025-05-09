# May 08 | Wasmtime Project Bi-Weekly

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
   1. Winch & Tier 1 status (@alexcrichton)
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)

## Attendees

 - Dan Gohman
 - Alex Crichton
 - Erik Rose
 - Nick Fitzgerald
 - Andrew Brown
 - Chris Fallin
 - Joel Dice
 - Pat Hickey
 - Saul Cabrera
 - Victor Adossi

## Notes

### Winch and Tier 1 status.

Alex: Tier 1 requires supporting all Wasm proposals, including tail calls,
which Winch does not support. Soon it'll likely include GC and EH, and so on.
This makes it difficult for compilers to catch up to Cranelift. We don't
tend to develop all the compilers at the same time. It would be unfortunate
to hold either Cranelift or Winch to enforce parity with the other.

Alex: Winch otherwise seems like a plausible candidate for Tier1; it has lots of
testing, fuzzing etc.

Alex: Do we want to label Winch as Tier 1, for the proposals it supports? This
implies that Tier 1 status is defined in terms of a matrix, with dimensions for
the compiler backend, the target, and the supported Wasm features. We'd need
to figure out how to present this, but what do people think of the overall idea?

Saul: We've been testing it internally.

Nick: This makes sense to me. We just need a way to document the matrix. Perhaps
footnotes, where we can have footnotes noting matrix regions not supported yet.

Alex: 

Nick: Ref types was originally on by default for x86 before it was implemented
for aarch64. Tail calls were similar.

Chris: Could we ever add OS to the matrix? For example suppose stack switching
and Windows. Would we accept that for a Tier 1 documentation?

Alex: That's a good question. I don't know where we draw that line.

Chris: If we allow processor support to differ, OS doesn't seem that different.

Nick: I think it's fine if they differ. Ideally we'll support all the things
on all the OS's, but if that implies a lot of work, it feels bad to hold back
useful features for people that want them on other platforms.

Chris: Could we distinguish between Tier-1 and on-by-default?

Alex: We do have Tier-1 features that aren't always on-by-default, such as SIMD.

Alex: I do like this idea of platform/OS combos. What if we say that a feature
like stack switching has to be Tier 1 on all targets.

Dan: Would that include Pulley?

Alex: I would say Pulley is effectively the same as Cranelift in that regard.

Eric: Would this potentially fragment with features, if feature A needs one
compiler and feature B needs another?

Alex: I think we could do that, but in practice I don't think it'll come up
very often.

Chris: In SpiderMonkey, once they get parity, it can be easy to get the baseline
compiler working first. So Cranelift being first right now is not necessarily going
to always be the case.

Alex: That's why I don't want to make any rules specific to Cranelift.

Nick: We don't necessarily need to have super-hard rules here. We can do things
case-by-case. If someone proposes a specific feature or backend for Tier 1, we can
consider it in context.

Chris: I like how Tier 1 is a signaling thing telling users something is stable
and safe to use.

Alex: Proposal: classify Winch as Tier 1 for the targets and features it supports.
We would also agree that we're going to talk about and analyze what it means to be
Tier 1 over time, but it would probably retain a relatively strong emphasis on
portability and supporting all the platforms.

Eric: This is kind of saying that the OS is dominant; that seems to be the criterion.

Alex: The major factor is Wasm's portability. It seems weird to classify a feature as
Tier 1 if we haven't preserved that portability.

Dan: That would seem to apply to ISAs too.

Alex: Our listing of tier 1 targets is specified by triple, so it includes the OS
and architecture.

Chris: There's a distinction between features and implementations. If a particular
backend doesn't support a particular platform, but a differnet backend does, it's
not as big of a deal, from users' perspective.

Chris: What features does Lime1 include? Looks like MVP + multi-value + a few other features. 

Alex: Yes, we should add a Lime1 to Wasm tools. 

### Lime1 and wasi-sdk

Dan: Does anyone have opinions about wasi-libc and Lime1?

Dan: In particular, do we want to have separate libc.a files to support
different subtargets, or would it be ok to just support Lime1? The issue
is that some engines don't yet support some of the things in Lime1, such
as extended-const and call-indirect-overlongs. Is it worth keeping these
disabled in libc.a or making a separate libc.a build without them?

A discussion followed, and I was active in the discussion, so I didn't take
real-time notes. Here's a summary:

Lime1 was defined slightly optimistically; we gave it features which aren't
completely universal yet, but which we believed shouldn't be a major burden
for any implementation. For example, extended-const just adds 6 simple
integer operations to implement for static initialization. And in return,
it enables much better dynamic libraries. So it seems best to just depend
on it, and oblige engines to add this feature, rather than holding everyone
else back.

So it seems to make sense to have wasi-libc move to just building its libc.a
for the Lime1 target, and not add a new dimension of libc.a builds.
