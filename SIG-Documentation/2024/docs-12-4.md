# Dec 4 SIG-Documentation Meeting

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announcements
    1. _Submit a PR to add your announcement here_
1. Other agenda items
    1. Project management for component docs - Timo 
    2. WASI HTTP / Rust sample
    3. _Submit a PR to add your item here_

## Attendees

* Kate Goldenring
* Eric Gregory
* Danny Macovei
* Timo Stark

## Notes

* Kate: Timo mentioned that docs for BA could use some PMing. I created this project board: https://github.com/orgs/bytecodealliance/projects/18. Right now it just has one issue. Not tied to a specific repo or docs objective
* Eric: Where is the right spot for a comprehensive and up to date tooling of BA projects?
* Kate: Maybe the Projects page on the BA site. This could be a good objective to revamp the BA site.
* Eric: Built on Jeckyl so adding a page wouldn't be too complicated. Actually redesigning is beyond my scope of abilities
* Kate: Now BA will have "graduated" projects soon so that page will probably want to be updated to delineate what maturity each project is at https://bytecodealliance.org/projects. Do we want a BA landscape
* Danny: Was watching a video about software being so complex and the CNCF landscape was the example
* Kate: when the landscape was first created, was it helpful
* Eric: Still could be useful to have something
* Kate: we would need to come up with categories too: runtime vs composition vs registries
* Kate: what are other foundations doing -- such as Eclipse
* Timo: Eclipse has more of a structured list rather than a landscape: https://projects.eclipse.org/
* Danny: Khronos has a little umbrella of all the things they work on: https://www.khronos.org/
* Kate: CNCF has a repo that you enter an entry into when you become a project. Then projects like the landscape pull from it.
* Timo: who updates this project page? https://bytecodealliance.org/projects
* Kate: I think David but he has express desire for support and revamping of the site
* Kate: Timo and I discussed easing the path to adding blogs 
* Timo: Blogs make it easier to discuss how to solve problems using a combination of tooling and then you can link to the technical documentation. You can pick up the user where they actually have the issue. I think the blogs are under the News tag. This is something about WASI, wkg, and warg -- people think it is cool and go to docs
* Eric: Should we re-label that tab to blog and add instructions to how to add a blog
* Timo: Can even create a blog about how to add a blog
* Kate: How do you add a blog?
* Timo: It's using Jeckl. Basically create a PR into bytecodealliance.org repo
* Eric: Do we want to rename that and add categories for News vs Blog
* Timo: What I would like to do is clone the repo, build it locally and then write a README section and add a CONTRIBUTION guide
* Kate: A blog template would be nice too. Would we also update the News tab in that pr
* Timo: just contribution guide and in subsequent PR update blog and add template
* Eric: This seems like a good focus area for us for the next couple meetings
* Timo: Project board for keeping track of blog posts and ideas etc. It is a main reference for what we are working on and provides transparency for the community. Helps create alignment around project releases. Best to keep the doc bound to the project -- update features and docs at the same time. Board gives us an umbrella across all repos 
* Kate: What do we think about the type of board I created?
* Timo: Sprint type of management makes sense for me -- we have 2 week cycles
* Danny: Having a backlog would be beneficial 
* Timo: I am less familiar with this kind of board and I don't think we will benefit too much using time based rather than bucket based
* Kate adds board view which will be main view

WASI HTTP / Rust sample

* Kate: Yosh is upstreaming a Rust WASI http example. Ideally we have other language implementations in the future too and these are pushed to a registry.
* Timo: This is great for nginx unit, too, having an example of how to create a Wasm component that targets WASI http and that example looks good to me
* Kate: Moved that issue to TODO column of the board :tada:

## Action Items

* [ ] Create issue on BA site repo to add a page for docs references
* [ ] Create issue on BA site repo to update projects list and describe project maturity levels
* [ ] Create issue to discuss repo for all projects to use as a source to pull in for projects page
* [ ] Create issue on BA site to rename news to blog 
* [ ] Update README and add CONTRIBUTION.md in BA repo about adding a blog and testing locally - Timo
