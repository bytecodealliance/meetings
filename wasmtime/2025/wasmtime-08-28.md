# August 28 | Wasmtime Project Bi-Weekly

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
   1. Issue gardening (@alexcrichton)
   2. WASI test suite [failures](https://github.com/WebAssembly/wasi-testsuite/issues/117#issuecomment-3214258179) (@tschneidereit)
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)

## Attendees

Dan Gohman
Alex Crichton
Pat Hickey
Joel Dice
Roman Volostavos
Till Schneidereit
Andrew Brown
Chris Fallin

## Notes

### Issue gardening

Alex: What should we do about the backlog of issues?

Nick: We agreed we want the state of the tracker to be better, but not about what to do about it.

Alex: Lots of old issues that aren't actionable, or relevant, or other things.

Alex: It's important to traige recent stuff. It's also important to go from the beginning
and clear out the old issues. So I propose we continue to triage new issues coming in,
but also each triage session we go through 10-15 old issues too, starting at the back and
moving forward in time.

Till: We have something, perhaps the labeler or triage automation, which is touching issues
making the "least recently updated" sorting harder.

Chris: Sometimes we hesistate to close things because they represent things we could do some day.
Maybe it's fine to close things; the history is still there.

Nick: I have no problem keeping moonshot issues open. I worry more about potential problems
that are sitting open.

Alex: I lose track of what's in the tracker. There are good ideas out there.

Alex, Nick: One challenge with this is that "Triaged" labels always seem to
get out of date, leading to "Triaged 2" labels, and so on.

Dan: Labels have staying power when they have concrete meaning. The problem with
"triaged" labels is they're about a process, rather than about the issue itself.
But if we stick to concrete labels that describe the issues, like "needs more info",
"bug", "feature request", etc., those feel like they can stand on their own because
they all have concrete meaning. And then we can focus triage on issues that lack
those labels.

All: General agreement.

Alex: Let's it once or twice and see if it works.

### Wasi-testsuite

Till: Andy Wingo has turned up some places where Wasmtime doesn't pass wasi-testsuite tests.
We should figure out our stance here. Some of these relate to wasip1's "rights" system, and
some of them relate to Windows, as Wasmtime is the only engine in the suite that's run on
Windows.

Till: Attempting to carve out subsets of the spec is a dangerous precedent.

Alex: Proposing new text as a "spec" for wasip1 is also a potential precedent.

Till: We need to make our position clear.

Discussion. We should propose changing the tests to allow things like rights to
not be implemented. These tests have only been run on a small number of WASI
implementations and don't reflect broader implementation consensus. And, we
should propose deleting fd_allocate tests because this functionality is not
provided on Windows.

Alex: I'm going to change the rights and fd_allocate tests to propose this.

Dan: I'll take a look at trailing-slash behavior.
