# September 24 project call

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

### Attendees
- abrown
- bjorn3
- amanieu
- tshepang

### Notes

Status updates:
- tshepang: just checking things out
- bjorn3:
  + working on stack load/stores
  + Chris fixed a pre-existing issue that helps (thanks!)
  + also fixing some older issues
- abrown:
  + working on extending the number of registers available from 16 to 32 (e.g., for AVX512, upcoming APX)
  + added a new `Limit` constraint to regalloc2 to this end
  + added a new `cargo bench compile` tool for dropping in `*.wat`/`*.wasm` files to measure compilation time
  + also, planning on being on sabbatical for the next month
- amanieu:
  + noticed the activity on regalloc2--any interest in revisiting the regalloc3 decision?

Discussion on regalloc3:
- amanieu: regalloc3 actually supports limiting the register set in a slightly different way
- amanieu: also, regalloc3 showed compilation and execution time benefits, e.g., https://bytecodealliance.zulipchat.com/#narrow/channel/217117-cranelift/topic/regalloc3.20benchmarks/near/527201621
- abrown: I am open to revisiting the topic but after I come back; it would be nice to have more regalloc options. I think the blockers are (a) not enough reviewer brain power for new 15k LoC and (b) not enough trust yet to replace a critical component (who will fix bugs? how fast? how to handle disagreements? who else can maintain regalloc3? etc.). Maybe these can change...
- bjorn: some small-ish dependencies could be inlined?
- abrown: there are many opinion issues like this that would need to be sorted out and compromised on...
- amanieu: I'm willing to work through that

