# September 28 project call

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

* abrown
* afonso360
* avanhatt
* bjorn3
* cfallin
* jlbirch
* jsharp
* ulrich

### Notes

No agenda.

- I lost my notes on avanhatt's status update

- jamey: working on Cranelift performance, cleaning up cranelift-frontend

- ulrich:
  - not much time for Cranelift this week
  - bug reported by Afonso
    - in s390x ISA, things that look like addresses have several optional parts which may be zero
    - modeled in Cranelift by using GPR 0
    - GPR 0 isn't defined by any instruction, so regalloc checker complains
    - shift count resembles an address (but isn't used to access memory) and uses the same pattern
    - local fix merged for shift instructions: don't report register uses of `zero_reg`
  - question: should we always enable regalloc checker?
    - chris: I built it for debugging, so it isn't slow but it isn't fast (10% slower maybe)
    - afonso: currently evaluating whether turning checker on in fuzzing affects execution rate
    - chris: lower overhead possibility: check that used regs are allocatable
    - chris: could change design to filter out unallocatable registers in regalloc2 instead of changing s390x backend
    - afonso: could also turn on regalloc checker in all filetests and maybe wasm spec tests
  - also need to fix other places that `zero_reg` is currently passed to regalloc

- afonso360:
  - added some ops to the Cranelift fuzzer
  - looking into always-correct instruction selection, which is the last thing needed for fast Cranelift fuzzing
  - helping upgrade cg-clif to latest version of Cranelift; running into some issues

- bjorn3: debugging a miscompilation for cg-clif
  - chris: something weird going on with call-argument register changes maybe

- chris:
  - merged 21k lines in yesterday: thanks to yuyang-ok for risc-v support!
    - no simd support yet: would be fantastic if somebody wants to work on that
    - otherwise, risc-v passes all wasm MVP tests; cg-clif might hit missing pieces
    - like other non-x86 architectures, risc-v is tested in CI using qemu
    - no performance evaluation
  - did a lot of work on getting the mid-end (e-graph) patches cleaned up
    - first two pieces are merged in
    - did a walk-through with Jamey on Monday
    - un-recursifying the code now
    - hopefully ready in the next couple of weeks; maybe merged?
  - profiling the compiler with Nick and Jamey
    - trying to do a differential comparison with Spidermonkey
    - Spidermonkey compiles the same code 3-4 times faster than Cranelift and generates similar-quality code
    - how is that possible?
    - one takeaway: four times as many register bundles in Cranelift than in Spidermonkey
    - if and when we have SSA-only input to regalloc2, should be able to use fewer bundles
    - still need to look at outstanding s390x PR for SSA-only instruction emission
    - may be other unknown non-SSA pieces: plan to look for them by adding SSA asserts
  - plan going forward: mid-end and compiler performance

- abrown, jlbirch: no updates
