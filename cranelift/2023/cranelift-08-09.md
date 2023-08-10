# August 09 project call

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
    1. MachBuffer and branch optimizations (alexcrichton)
    1. _Submit a PR to add your item here_

## Notes

1. MachBuffer and branch optimization (alexcrichton) https://github.com/bytecodealliance/wasmtime/issues/6798
- Issue on slow compilation.
    - Not full module, but alexcrichton was able to reproduce from screenshot. A label fixup was quadratic by accident.  First thought: binary heap, worked for that aspect, but did not work for optimizations to the MachBuffer. Original issue fixed by cfallin’s PR changes handling of islands. Might want to in the future stop eagerly promoting branches. 
    - Two problems: linearizing the CFG, and branch/block optimizations. Extended basic blocks are single-in, multiple out. Using these gives some new optimization opportunity, but leads to complexity/changes for standard compiler technology. ~4 years ago Cranelift used this, now moved away from that, Cranelift has an actual CFG including VCode.
        - Question: can we do branch optimizations in the mid end? E.g. branch constant folding. Nothing precluding us from converting an if to a jump. 
        - alexcrichton question: the MachBuffer does nice linearization, does it do more than that? Answer: yes, it does 3 optimizations: jump to fall through to just jump, conditional jump over unconditional jump converted to inverted conditional, redirect branch to unconditional branch to remove indirection (plus cleanup if unused). Design decision in regalloc2, require all critical edges to be split. 
    - The idea either cfallin or alexcrichton wants to do: when you hit the island, not promote if we otherwise would have put it back in the list, and instead put it in a sorted/binary heap. Hybrid approach.

### Attendees

- cfallin
- alexcrichton
- avanhatt
- fitzgen
- jameysharp
- abrown
- jameys
- uweigand 
- jlbirch

### Status Notes

- cfallin:
    - No Cranelift updates.
- abrown
    - Stoped working on MPK implementation, but has a WIP branch (https://github.com/bytecodealliance/wasmtime/compare/main...abrown:pku) that needs more testing/fixup. Would like a review if anyone has time to take a look. (1) how to configure whether to enable, (2) how to configure the Wasmtime API. Would like feedback on those before going back to work on it.
    - alexcrichton will try and get to reviewing it soon
- alexcrichton
    - With the landing of tails call work, tests weren’t running on Arm64 on MacOS. Differences in how stack pointer are handled is what was breaking the tests, alexcrichton has a PR to use the “Z” modifier instead. fitzgen had a comment on how to test this, but sadly can’t really test on CI. QEMU checks pointer authentication stuff, but the Clif config needs a certain feature flag. Off for everything but MacOS. But capturing a native backtrace fails. Currently a bug that segfaults on some unwinding in this case. 
        - jameysharp: the release process creates some text in the auto PR that says to check a CI result, but looks like it has not been passing for some time. Embark need to change their config. Now no Mac CI. In theory Github will eventually have an Arm64 macOS target.
        - fitzgen: can we hack this for now with an environment variable somewhere? Could add a single test that enables signing the return address. alexcrichton will try and play around with this. 
        - Moved Amode stuff into ISLE. 
- avanhatt: 
    - non-code paper updates
- jameysharp: 
    - Doing some work on regalloc2. Want to use parallel moves resolver to improve tail call ABI support. 
- elliottt:
    - Experimenting with regalloc2 redundant move eliminator. Also looking at better support for minimization of test cases. 
        - Libfuzzer has built in minimization. 
        - Using something like creduce is blocked right now because there is no text-based format for regalloc2. 
        - Also, changing the “arbitrary instance” might also help fuzzing be more directed. Took 3 weeks to find the bug. 
        - Are there patterns where early on bytes from libfuzzer correspond more directly to changes in the generated program. It generates only valid code. 
        - Curious about: anything written up on how to generate inputs such that libfuzzer is better at minimizing? Probably not. A little unfortunate that we derive values from the byte buffer. 
        - Other option: use structure-aware mutators instead of structure-aware generators. 
- uweigand:
    - Looked at tail call stuff, part of their ABI where they pass arguments by indirection. Not sure how that can occur with the tail call ABIs. Question: is there formal specification of the tail call ABI and what data types it supports? fizgen answer: Wasm datatypes, basically, not the fancier Cranelift things that `cg_clif` uses. Right now: arguments prepared somewhere, copied somewhere else. Would have to do pointer updates or generate differently. Right now they hardcode for i128. Could switch to using a vector register for that case. alexcrichton: on RISC-V as well there is indirection for arguments that will need to change. 
- jlbirch:
    - No Cranelift updates.
- fitzgen: 
    - No Cranelift updates.
