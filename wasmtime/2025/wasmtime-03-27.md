# March 27 | Wasmtime Project Bi-Weekly

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

* TODO

## Notes

- Alex: Regarding the CVE process; Opened a PR to RustSec; they're a bit understaffed
  - Should be able to import their advisories
  - Alex will do that and set example for future imports
  - Recent issue: found a compile-time panic
     - Policy is that we don't consider it a security vuln
     - But others might consider it to be!
     - Is there another process that's lighter than CVE to let people know as a courtesy
     - Nick: any bug could be considered a security bug; maybe just backport and do point release
     - Alex: currently anyone can do such a backport, but we haven't committed to do it ourselves
     - Nick: we can do it, but no guarantee
     - Alex: should we guarantee it (for certain classes of issues, e.g. panics)?
     - Chris: yeah, makes sense for panics; not a huge burden
     - Nick: makes sense to strive for (as best effort), but concerned about a hard guarantee
     - Alex: Could we automate point releases on e.g. monthly basis if backport branch has changes?
     - Nick: can't argue against automation
     - Andrew: could use GitHub issue tags and give people a URL to search for those tags
        - Need discipline during triage to remember to tag
     - Nick: who would use this versus just watching the repo in general
     - Chris: most people will only care about CVE in terms of knowing what needs to be acted on
     - Andrew: hard to decide for others what's worth a backport and what isn't
     - Alex: we need to decide what we care about, and others can contribute with what they care about on top of that, if anything
     - Nick: best effort on our side; others who know they are affected can do backport
     - Alex: tiers: CVE, then backports, then normal fixes to `main`
        - Will document that
     - Chris: could also distinguish between LTS backports vs. non-LTS backports
     - Alex: user might backport to LTS (because that's what they're using) but not latest stable release
     - Nick: require backports to LTS to also go to newer branches?
     - Alex: could be prohibitive effort
     - Nick: not comfortable with promises given our small team
     - Chris: do think we'll need to make sure backports need to land in all newer branches to avoid confusion
        - Might discourage contributions, but still need to do it
     - Paul: Linux kernel has similar situation; GTH has enforced consistency
     - Alex: conclusion: if backport to be made, must apply to all active, affected branches (newer or older)
- Issue triage
  - Alex: regarding tagging, we only triage stuff that hasn't had enough activity, so would need to change that to ensure all issues tagged appropriately
