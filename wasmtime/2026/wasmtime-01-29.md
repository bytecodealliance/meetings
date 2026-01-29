# January 29 | Wasmtime Project Bi-Weekly

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
   1. Find volunteer note taker. Thanks!
1. Announcements
   1. _Submit a PR to add your announcement here_
1. Other agenda items
   1. Summary of security release
   1. Reliability of and dependence on GitHub CI
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2026-01-15)
   * [Old, Backlog Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aissue%20state%3Aopen%20sort%3Acreated-asc)

## Attendees

* Pat Hickey
* Chris Fallin
* Tolumide Shopein
* Nick Fitzgerald

## Notes

* Chris: Summary of [recent security release](https://github.com/bytecodealliance/wasmtime/security/advisories/GHSA-vc8c-j3xm-xj73)
  * Cranelift miscompilation of `fcopysign` on x86-64 w/ AVX
  * Implemented via bit ops on floats
  * For one of those bit ops, `vandnpd`, when its operand is a load, we can sink
    the load into that instruction as a memory operand (when considering it in
    isolation)
  * Problem is that the `fcopysign` operates on 32-/64-bit operands, but the bit
    op in question works on 128-bit operands
  * With register operands, this is fine because the high bits of the register
    are simply unused
  * But with load sinking, this means we load 128 bits from memory rather than
    32/64 bits
  * Which means that Wasm could trigger loads just beyond its linear memory
  * With the default config (guard pages) this triggers an erroneous heap
    out-of-bounds trap
  * Without guard pages, we bounds check based on the correct load size, but
    then do a load with the larger size, which allows reading up to 64 bits
    beyond the Wasm linear memory
  * This results in either (1) a segfault (if no page is mapped immediately
    after the Wasm linear memory) which is a potential DoS vector, or (2)
    loading data from outside the sandbox, but note that this data is unused and
    cannot be read by Wasm.
* Reliability of and dependence on GitHub CI
  * (Delaying this topic to next meeting)
* Nick: Update on OOM handling project
  * We handle OOMs during `Config`, `Engine`, and `Store` creation now
  * Working on `Linker`, `Func`, `FuncType`, and the types registry now (all are
    entangled)
  * The types registry is a little tricky because we need to handle rollback
    upon registration failure so that the registry remains valid for other
    `Store`s/threads, since it is in the `Engine` and shared across all the
    `Engine`'s `Store`s and `Module`s and all that
  * This is also forcing us to define lots of OOM-handling versions of common
    data structures, some of which have landed already (`Vec`, `PrimaryMap`,
    `SecondaryMap`), others of which will soon (`HashMap`, `HashSet`)

## Last Old, Backlog Issue

*This tells us where to begin the backlog triage next time.*

Resume old issue backlog triage at #1111 next time.
