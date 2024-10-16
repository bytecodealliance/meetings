# October 16 project call

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

* Nick Fitzgerald
* Alexa VanHattum
* Ulrich Weigand
* Andrew Brown

### Notes

* Updates:
  * Alexa: intel announced new x86 semantics they are releasing, hope to use
    them in the near-ish future with our ISLE verification
  * Ulrich: none
  * Nick: none
  * Andrew:
    * thinking about rewriting the x64 assembler and encoder
    * possibly adding APX extension in the future
    * general discussion of describing instructions in a table, generating Rust
      and ISLE code from it
      * want to mechanically generate instruction encoding, regalloc
        constraints, pretty printers, etc... from the table, put each
        instruction's semantics in the table, etc...
      * general consensus that generating mach insts in `cranelift-codegen-meta`
        via builders is preferable, rather than parsing text and maybe requiring
        new deps. parsing (and/or building new deps) would negatively impact our
        build times
      * Nick: would kind of like flattening mach insts so that they don't expose
        instruction formats to ISLE, where we don't care about that kind of
        thing. for example, replace `AluRmR` which contains an `AluRmROpcode`,
        with `AddRmR`, `SubRmR`, and etc. Still okay if the encoder internally
        factors code such that it is deduping via instruction formats, but that
        shouldn't be leaked to ISLE
      * Alexa: flattening would make our verification efforts easier as well
