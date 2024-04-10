# April 10 project call

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

- @jameysharp
- @avanhatt
- @elliottt
- @saulecabrera
- @fitzgen

### Notes

@elliottt

- we previously discussed always reserving the maximum outgoing argument area
- now implemented that for all backends
- riscv tests no longer run with "target riscv64" in qemu, now seem to need "target riscv64gc"
- plan for tail calls is to resize the incoming argument area during prologue
- we hope stack frame layout will then match s390x
- we want to remove "nominal SP" after this because there'll always be fixed offsets
- @fitzgen: is there anything still blocking using the tail call ABI all the time in Wasmtime?
- we haven't yet fixed the performance regression on aarch64/riscv64
- @fitzgen: so it's not worth applying the same fix we have on x64 as an incremental improvement?
- it'll be better to apply Ulrich's suggestion from a few weeks ago instead
- @fitzgen: we have technical writers to help write blog posts and it'd be great to talk about tail calls when we turn that support on

@avanhatt: no major updates

@saulecabrera:

- working on slimming down the macro assembler so it'll be less work to add more backends
- checking on aarch64
- want to see which fuzz targets we can enable for winch
- @elliottt: Nick had suggested the `table_ops` fuzzer which Trevor looked at but hasn't determined whether it's actually fuzzing Winch yet

@fitzgen:

- landed more wasm-gc work but new fuzz bugs came in (https://github.com/bytecodealliance/wasmtime/pull/8317)
- Cranelift optimizations produced wrong code
- it's not clear whether the optimizations were wrong or whether the frontend was producing invalid CLIF
- if an r64 is bitcasted to an integer type, so we can use it, then all uses of the r64 might become dead and it gets removed from the stack map and may get garbage collected
- worked around by disallowing GVN of a bitcast from r64 to i64
- better fix is to allow integer ops on reference types without bitcasting
- @jameysharp: I'd like to extend integer ops to work on reference types anyway, such as using `icmp` instead of `is_null` so existing lowering peephole optimizations apply
- would like to move safepoints out of RA2, as an explicit part of IR instead

@jameysharp:

- fixed stack limit checks
- cleaning up the target-independent `StackAMode`

Further discussion:

@fitzgen: more about safepoints and stackmaps

- many of our CVEs have been due to GC bugs from getting stackmaps wrong
- is there some formal verification thing we can do?

brief overview of safepoints and stackmaps:

- we have a separate type for GC values, r64, which is basically integers but separate types so RA2 knows about them
- certain instructions are marked as safepoints (calls and traps), because in our system GC can only happen by calling out of the generated code
- regalloc looks at which references are live at safepoints and creates a bitmap indicating which stack slots have live references in them, which we call a stackmap

- @avanhatt: does RA2's checker cover stack maps?
- @elliottt: it does check safepoints; it has caught many bugs due to safepoint handling

- @fitzgen: maybe there's something we can do in the CLIF verifier; check that all values which are live across a safepoint appear in that safepoint

- @fitzgen: https://gchandbook.org/ is a great book for background on this
