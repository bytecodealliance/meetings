## September 19, 2023 - SIG Guest Languages meeting

|          |      | 
| -------- | -------- |
| Attending  | Brett Cannon, Guy Bedford, Robin Brown, Timmy Silesmo, Saúl Cabrera, Kevin Smith, Achille Roussel, Aaron R Robinson, Scott Waye
| Note Taker | Robin Brown

## Updates

* Joel: Working on resources for Python

## Agenda

* [Subgroup C#/.net](https://github.com/bytecodealliance/SIG-Guest-Languages/pull/7) Proposal
    * Voted in favor of creating the new Subgroup
* Meeting Frequency
    * Agreed to move the meeting to a once a month cadence.
    * With a slower frequency of the main group, we'll aim for more detailed updates from the subgroups.
* [Joel] - Module De-Duplication
    * Discussed how compositions of components conaining runtime modules contain multiple copies of the same runtime and how to avoid that.
    * Using module imports ideally with registry identifiers in combination with registry-aware composition tooling can help.
    * Tooling may want to factor their components into a more structured module graph so that doing this is easier and may want to use the dynamic linking approach used by Python if applicable.
* [Robin] - Unifying tooling arguments
    * Different Componentizing tools in the BA have different arguments/names for the same concepts (e.g. wit path, world name) and we should unify them to avoid confusing users.
* [Joel] - Java Update
    * Joel spoke with Sébastien Deleuze about Java component work.
    * General takeaway is that we'll have to wait and see if Oracle becomes interested in Wasm or views it as a competitor and what happens in the next few months.

## Action Item

* Robin: Merge subgroup vote & reach out to organizers
* Robin: Poll for meeting date
* Robin: Start command line args discussion on Zulip
