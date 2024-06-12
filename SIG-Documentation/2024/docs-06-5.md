# Jun 5 SIG-Documentation Meeting

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
1. Announcements
    1. N/A
1. Other agenda items
    1. N/A

## Attendees

* Ryan Levick
* David Bryant
* Eric Gregory
* Kate Goldenring

## Notes

* Message from community member on wanting WAT in component book
  * Kate: It was an active choice to not include WAT
  * Ryan: Should not include WAT. I deal with components on a low level and still can struggle to deal with WAT. It is a representation of what is in the binary. Don't need to expose people to it necessarily. We could put it in the references section. Instead we could present users with opaque, already built components (say from a registry), and we can show you the tools of how to poke around at a component (with `wasm-tools component wit` to see it's world). Then they don't need to understand how the component was built
  * Eric: Sounds good
  * Kate: to summarize, component book is supposed to be approachable and illustrate how you should build/use components. WAT is neither of those things
  * Ryan: To poke around and use components (which is half of the objective of the book), you don't need to build components. So we can also achieve a simpler demo without language tooling needed.
  * Kate: Do we know where we are with the ability to host a component in the BA registry
  * Ryan: Talked to Lann and things seem *dangerously close*. Maybe we can apply pressure by saying this is a use case.
  * David: SIG registries is going through some refactoring, changing to SIG packaging. There was some preliminary work to stand up a registry that is no longer online. We've orbited around this work for awhile but haven't landed it.
* WASI.dev reception
  * Eric: haven't gotten explicit feedback. Some things are missing that i'd like to see there. I'd like to see more about reading interfaces and using them -- that could also be docs that point over to the component book
  * Kate: Maybe have more of the interfaces that have not reached phase 3 (such as the embedded ones), so that we can spread the word on them and get more eyes and help.
* Discoverability of component model docs
  * David: Are there things we can do on the Bytecode Alliance website? I am in a position to help with getting things on the website.
  * Kate: I'd love to tell people to go to Bytecode Alliance.org to find the documentation
  * David: Site is really outdated. Can we dovetail it into the current layout
  * Eric: A learning tab would be great
  * David: We did simplify the tabs but we could add it back. Maybe discuss in Zulip. We don't have any information on the site about SIGs.
  * Eric: Also the events calendar is not there
  * David: SIG community too -- we want to share events
  * Kate: is the site in the BA repo? Should this be a GH issue? I don't know the technical layout of the site but i think the "About" tab could be expanded upon
  * David: We threw this together in the early days with Jekyll. We may slow things down with a full refactor rather than fitting doc references into what we have.
  * Eric: For clarity, is there buy in from other SIGs that we need to change the site?
  * David: if it is simple (just increasing visibility of docs), probably not. If it is a refactor, people may have other asks that they want incorporated. There is no website team. It is me and Till.
  * Eric: Seems like a good item to have on our agenda
  * David: SIG docs is a great place to facilitate that conversation and have informed opinions around the site refactor
* Referencing component book from other repos
  * Kate: maybe we can create an issue in the component book to list all the repos we want to point out to the component book


## Action Items

* [ ] Kate: Create issue in component-book to track around quickstart with pre-built components
* [ ] Eric: add phase 1 and 2 WASI interfaces to WASI.dev
* [ ] Kate: file an issue on adding books to bytecodealliance.org (link to Ryans issue on component docs)
* [ ] Kate: create an issue on component-book to track which repos we should link to it
