# February 23 SIG-Debugging Meeting

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. _Submit a PR to add your item here_

## Attendees

- Chris Fallin
- Jamey Sharp
- Benjamin Bouvier
- Joel Dice
- Ralph Squillace
- Ulrich Weigand
- Andrew Brown
- Timothy Chen
- Chenyu Jiang
- Jeff Charles
- Saúl Cabrera

## Notes
- Ulrich’s presentation on GDB
- Joel:
    - Going back to the GDB protocol and server, and trying to map the layers that we’ve discussed previously
        - Could GDB be taught to treat Wasm as native architecture?
- Ulrich
    - Might be possible — something similar (or precedent) would be using QEMU.
    - For Wasm: to what extent is Wasm similar enough to a real ISA; it might be possible to bridge the gap, it would require some work.
- Chris Fallin:
    - How flexible GDB is? Is there’s precedence for adaption maybe there’s room for using it for Wasm.
- Ralph:
    - Follow up to Joel’s question.
    - Modelling Wasm as the target to debug, seems to bring a coherent story for the end developer.
- Ulrich:
    - Maybe what we need is different tools for different use-cases.
    - When using GDB, a possibility would be to see the Wasm that generated the JIT code, but also the source code that generated the Wasm. Allowing an inspection of the main layers. 
    - But there’s work to be done, nothing works automatically.
- Andrew:
    - Can GDB inspect between targets while debugging (at a breakpoint).
- Ulrich:
    - Disassembly and source level view what it can do.
    - There’s no notion of 3 layers (Source, Wasm, machine code)
- Andrew:
    - Can this feature be added?
- Ulrich:
    - I’ll need to think about this. 
    - If you treat Wasm as the machine code, you could see Wasm as the disassembly and the source level too, but you no longer have access to the machine code.
- Andrew:
    - Can you decide the things that you want to look at before starting the debugging session?
- Ulrich:
    - Maybe you need to have two tools to do this.
- Joel:
    - Can this be modelled as a “thread context switch”?
- Ulrich:
    - We had something similar in the past. Might be possible, but has to be implemented.
- Andrew:
    - GDB is a wide collection of things — they might be open to new additions?
- Ulrich:
    - I’m one of the maintainers, so happy to accept new contributions.
