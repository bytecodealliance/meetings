# May 18 SIG-Debugging Meeting

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. _Submit a PR to add your item here_
    2. (fitzgen) are we ready to begin drafting an rfc?

## Attendees

* Nick Fitzgerald
* Andrew Brown
* Johnnie Birch
* Jeff Charles
* Saúl Cabrera

## Notes

### Status on the coredump work

- Rainy: Has been working on coredumps.
- Nick:
    - In parallel working on the spec pieces for the coredumps.
    - Right now a coredump can be generated, but there's still work need to
      be done (e.g. give Wasmtime the ability to read the coredumps
      compliant with the DAP protocol)

### Initial RFC

- Nick:
    - Are we ready for an RFC?
    - The first RFC would detail the roadmap for guest debugging, which at
      a high level would be:
        - Producing Coredumps
        - Serving coredumps
        - Extending the DAP for live guest debugging
        - Eventually have record and replay of the guest

- Johnnie:
    - What aspect of this is reusable?

- Nick:
    - We could share some pieces, but mostly it's Wasmtime specific.
    - All the tooling around coredumps is not Wasmtime specific though.

- Andrew:
    - Why we went mostly down the guest route initially?

- Nick:
    - Offering a guest debugging experience is the most common use-case,
      particularly taking into account the code of propietary platforms.
    - The end-to-end debugging experience is a pretty advanced feature.
    - The RFC will provide some more color here.

- Andrew:
    - Who will be working on this?

- Nick:
    - From Fastly's side, mostly Rainy, but certainly having one or two people
      helping in different ways might be good.
    - Starting is the most difficult phase most of the time.

## Action Items

* [ ] TODO
