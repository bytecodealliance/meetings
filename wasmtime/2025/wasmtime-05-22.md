# May 22 | Wasmtime Project Bi-Weekly

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
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)

## Attendees

* Chris Fallin
* Paul Osborne
* Erik Rose
* Andrew Brown
* Alex Chrichton
* Arjun Ramesh
* Dan Gohman
* Khagan Karimov
* Roman Volosatovs

## Notes

Eric:
* VM Tricks to make epochs faster
* Approach:
	* Inject instructions to poke (made) illegal address to trap
	* Move on eventually to invalidating whole-stack
* Looking forward to numbers
* May be worth looking at async signals again carefully one more time
* Chris: you can mmap an existing memory region again, might be a way to use this in order to avoid problems (maybe).  Boundaries need to remain the same (possibly pad out to keep the same with noops).  Could replace the code at runtime essentially.

Chris:
* Arjun/Khagan interns at F5 working on fuzzing and record/replay respectively

Paul:
* Taking on getting the stack switching work from the wasmfx folks across the finish line.

