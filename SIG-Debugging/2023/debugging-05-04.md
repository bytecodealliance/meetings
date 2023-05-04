# May 04 SIG-Debugging Meeting

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. _Submit a PR to add your item here_

## Attendees

* Rainy Sinclair
* Jeff Charles
* Joel Dice
* Saúl Cabrera
* Thomas Pelletier
* Julien Fabre
* <some folks joined towards the end, so if I missed you feel free to add yourself here>

## Notes

* Rainy
    * Discussion on how to handle coredumps with multiple wasm modules started in the [tool-conventions repo](https://github.com/WebAssembly/tool-conventions/issues/204). Weigh in if you have thoughts or ideas there!
* Joel
    * Circulating an RFC for shared-everything linking: https://hackmd.io/IlY4lICRRNy9wQbNLdb2Wg
    * Modeled somewhat on emscripten’s way of doing things
    * Initial implementation takes in a set of shared library module and produce as output also a module
        * End goal is to produce a component as the output
            * Difficulties with that in the short term
                * One of the consequences of is that we would drop DWARF debugging because of the transformations involved
        * Once we move to components we’ll be able to maintain some of that dwarf support

## Action Items

* [ ] TODO
