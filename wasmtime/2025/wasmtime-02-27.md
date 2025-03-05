# February 27 | Wasmtime Project Bi-Weekly

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
   1. Post-mortem of https://github.com/bytecodealliance/wasmtime/issues/10184 (alexcrichton)
   1. Explore options for contracted work (Bailey, [table])
   1. Please review [LTS RFC](https://github.com/bytecodealliance/rfcs/pull/42) at your leisure (alexcrichton)
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2024-12-19)

[table]: https://docs.google.com/spreadsheets/d/1k-eiCIzwpXrPFtlIYI66jr1AZphyI7tmKsjFuYTCWCI/edit

## Attendees

*this is alex's best recollection of the attendees, it may be inaccurate*

* Alex 
* Chris 
* Till 
* Bailey 
* Pat 
* Paul 
* Nick 
* Roman
* Victor
* Oscar
* Erik
* Andrew
* James

## Notes

#### post-morteam

* Alex: debrief of issue where rust nightly changed subtly about how it called
  `PartialOrd` and `Ord` and how a bug in `wit-parser` where the two traits
  disagreed got exposed and ended up triggering compile-time failures in
  Wasmtime through a series of unfortunate events. Everything's fixed now though
  so was a heads-up.

#### contracting work

* Bailey: raised in last board/tsc meeting. Open the floor for what are (a) high
  priority things we could contract out to e.g. igalia on behalf the BA. Wanted
  to toss out basic rubric criteria but ok changing anything. Most important
  thing for mission is that we're building something aligning around safety for
  wasm. First crop is around fuzzing. Probably a great place to start. Wanted to
  get more context here to scope work well. Can run through items but maybe
  better to open up with questions.

* Chris: appreciate the effort! Wanted to relay a story. 4 years ago we looked
  to switch to new backend at Cranelift so paid a security consultancy firm to
  do a risk assessment and validate that everything was reasonable. In March
  2021 everything came back good (e.g. good code, fuzzers, no issues). Switched
  to the new backend. Next month we had a CVE which was a big deal. Compiler
  safety I think is the highest priority but isn't here. Hard to put stock on
  external folks coming in and writing fuzzers. Needs to be a lot of domain
  knowledge and we're doing the due diligence of differential fuzzing in core
  areas already. Main thing is we need to push the state of the art. I wanted to
  push more funding to academics and curious what folks think of that.

* Till: hesitant to put too much stock into that experience as precedence. For
  that security assessment it was an overall assessment with a not very large
  budget. Meant we had to tell them what to focus on and the focus was scoped to
  the integration of the new backend and of Wasmtime into the server
  infrastructure that Fastly had. Much more specific and in that scope there was
  no realistic opportunity to catch the subtle bug in code generation. Was
  subtle enough that we had it twice in two different code bases. I don't think
  this is necessarily a precedent against a company with the right kind of
  expertise focused on specific kinds of fuzzing. Not saying it's a small order,
  the "easy" here is probably a bit off and is quite difficult.

* Chris: lesson is that maybe we didn't spend enough? The biggest risk to me is
  compiler correctness because it's part of the TCB. Paying folks to fuzz it and
  find issues is a bit demeaning?

* Till: oh no very much not the context. Conversations came from the context of
  having more to do with p3 and WASI. Context is very much not "hey can someone
  throw money and make p3 ship faster" nor "throw some money and make things
  more secure". One example from the past is to have someone start working on a
  compliance test suite for WASI. Maybe p3 and onwards? No test suite today at
  all. Making that available to the ecosystem and seeding it to be easily
  contributed to and upstreamed is similar to WPT and test262. All current big
  vendors have streamlined ways for writing tests in their environments and
  being synchronized upstream. That kind of infrastructure for compliance with
  WASI would be extremely compelling. Know there are companies with such
  expertise. Hard to resource in day-to-day work. Would prefer to look into
  that. If we go with fuzzing then should focus on component model or on
  operational semantic for specific APIs. No operational semantics defined today.

* Chris: To finish -- demeaning in the sense that we already have a lot of
  expertise. Need to respect that we can't just have folks come in and start
  from basics. Would need to find the right people and this is almost
  research-grade. Open question of how do we explore interleavings and get good
  coverage. Need right people with the right level of understanding, not a
  commodity-level consultant.

* Nick: to add on to that, it's not just the right people, they'd need to work
  with us. Last time folks were hired and we didn't hear from them or talk with
  them. It wouldn't work out well if it's like that. If partners though and
  picking up work we'd like to do but don't have cycles for that'd be a better
  relationship.

* Andrew: don't want to invest money in "parachute in and leave" type of
  operation. Should invest in expanding the community and long-term
  contributions not just for the period of the contract. Growing the developer
  base of this project. Scariest thing for me is all the concurrency bits with
  p3 bringing in async and with what I'm trying to do with threads. If we were
  to invest in anything the scariest bit is safe concurrency.

* Nick: tried to get TSAN working awhile back and it does not work at all. Would
  be really nice if we had some kind of answer for something like this.

* Till: Question to andrew: not sure how realistic it is that we go into
  contract with the expectation that they'd continue contributing.

* Andrew: understand that yeah, not thinking of any funny clauses. One story is
  that the researchers at ediburugh worked on stack switching. If we had funded
  them then all the work and research they put in has given them the expertise
  to know about wasmtime. If they do more wasm research in the future they might
  reach for wasmtime. Feel like there are some examples where if you invest in
  the right way (the BA didn't here) it can be possible.

* Nick (chat): once igalia (for example) gets devs that are experts in an engine,
  they can actively search for more contracts working on that engine and tout
  their expertise and all that

* Till: what Nick is writing in chat makes sense to me. Three incentives for OSS
  projects (alex lost notes here). We won't convert contractors into those kind
  of folks (alex lost what it's referred to). If you're talking about how now
  they have expertise and then after that "come to us with an itch to scratch"
  I'm fully on board with that. That'd be fantastic. We might even try to help
  them.

* Nick: Let's snip this a bit and maybe continue making progress.

* Chris: My general point is that the thing Andrew is talking about,
  sustainability, I feel strongly about as well. OSS often deals with
  transactional nature of "come in and go away". I would add another plug for
  academics have a strong interest in formal verification. It's a good long-term
  answer but not a great short-term badge. If we can fund groups that are
  looking for that the characteristic is that they become familiar with the
  ecosystem and when they have more ideas they'd reach for Wasmtime.

* Andrew: One more thing -- another approach here is instead of investing in the
  hardest problems would be to focus on the "trivial" problems. Come in and fix
  random things no one wants to do to free up others to work on other
  high-priority items.

* Alex: *alex did not take notes so this is his best summary of his own comments*
  I think it'd be best to view this contracting work as a long-term investment
  which is unlikely to have immediate short-term gains. Agreed lots of this here
  may require a lot of context but something like exception-handling might be
  focused enough to delegate.

* Till: My view is that apart from some contained specific things like
  exception-handling ... To make clear have worked with igalia in the past and
  know what to expect of them. They have deep domain expertise. Specifically
  igalia things look somewhat different here. While they might not have deep
  component-model expertise they know how to do many things we're talking about
  and have an excellent team. If we wanted to do this kind of thing then what
  would make sense if talking about doing this more than a one-off thing and
  more of an exercise of building up a muscle of can we bring contracting into
  the mix? If it's not necessarily the BA paying things then maybe a direct fund
  in the BA to help funnel. Or BA helps igalia or consulting entities to become
  established community members and see what we can do about getting them
  contracts. E.g. exception-handling is important but we don't have the right
  person the next best thing is contracting. Otherwise if we do this as a
  one-off then software development isn't what we should look into, instead
  automation tasks that are hard to resource but might make our lives easier
  afterwards. For example test infrastructure I mentioned. Seeding the test
  harness and making it easier to grow. Different way to get ongoing benefit
  without ongoing maintenance and costs that we shouldn't already hav.

* Chris: very much agree, can think of using contracting dollars for the
  boring/grungy stuff that has high activation energy.

* Bailey: Pat!

* Pat: I want wasi-libc to have native wasip2 support to pave the way for wasip3
  to get rid of the adapter. It's grungy and no one's had time.

* Bailey: heard there were ideas, so this is just a strawman! Had this
  conversation a few times each year but never did anything about it. Hoping
  this format might work! These items are what I heard, not necessarily
  advocating. Pat want to speak to this?

* Pat: Lots of ways to skin this cat, don't want to make this a design session.
  Right now `wast` is a superset of `wat` which is wasm + assertions. To pass
  conformance tests you write a wast driver for your runtime. Would be
  significant value (and undertaking) to do the same for WIT. Unsure if it's
  appropriate to contract out it may require a lot of context. If you could
  write contracts about WIT interfaces then you could tell whether a runtime is
  conformant. Never tried to do this. All tests for WASI are written in Rust as
  guest modules.

* Till: maybe not wast? WPT? as the model? Harder parts are defining scenarios
  in terms of available external resources. E.g. external server goes down while
  sending headers.

* Pat: right! lots of things where tests are testing things that are internal to
  the component - few about testing things about the outside. Outside testing is
  one-off tests. Goes back to testing automation perhaps, would be great to make
  this easier to write. Want to move down the list here too.

* Victor: going down in order? can make suggestions? yes? ok! Ran into Yuta who
  works on ruby/wasm support. He spends a lot of time on those projects and it's
  quite hard. Would like the idea of helping with ruby wit-bindgen support. That
  was something he needed was necessary to move ruby support from basic to
  "anyone can use it" with component model support. Other thing on Swift was
  support needed there. Other than pointing this out I wanted to ask if the idea
  of delegation of this ... we could fund Yuta ... but might be better to have
  him take part in distributing or finding someone to do the work. He could do
  the back-and-forth of getting them up to speed. E.g. empowering an existing
  maintainer to get up-to-speed. He's just a graduate student right now.
  Existing community member doing a lot of work in 2 languages. Probably would
  be able to distribute funds in a good way. Can tack on "please write about
  your work" and can uptick ecosystem <alex missed this> and awareness.

* Till: if we were to look at this specific item as can we help a student make
  their contributions more sustainable in a low-key way that makes sense to
  me. I'd worry less about the specifics as long as it's not a lot of money. If
  it's specific projects I'd fear focusing on wit-bindgen-for-ruby would get us
  into ... harder to feel there's a clear objective case. Much more driven by
  some people with more interest in ruby than other languages. Then BA has a
  role in facilitating but funding should maybe then come from specific
  companies with aligned interest.

* Bailey: the ruby team. <alex missed this> About aligning with the ruby
  upstream team. Falling out of something I heard. If we refactor wit-bindgen in
  a way that supported plugins. Would make it easier for folks to self-maintain
  and support their own uplift. Can help someone who's highly motivated.

* Chris: talking about a whole bunch of different topics. We probably don't have
  an infinite money fountain here. What's the prioritization heuristics here?
  Going wide with languages? Getting more assurances with what we have? Anything
  we can throw money at?

* Bailey: Challenge for me as i'm downstream from maybe 3 conversations trying
  to bring this together. Understand the initial conversation started as how can
  we confidently ship WASIp3. For me that's top. If something is a small thing
  we can get it done cheaply and frees us up to do other things. That also helps
  it get prioritized. I could maybe come up with 30 more items and could be here
  all day! If we put our heads together for confidently shipping WASIp3 (not
  faster, confidence). Can feed into us shipping software faster eventually
  <alex is losing a lot of notes here>. There are definitely things here that
  are worth talking about. Specifically could talk to MSFT friends. Not
  necessarily saying we need to use igalia but we need to solve some problems
  this year. Feels like many of these should happen this year. Getting feedback
  on what we can feasibly do if we inject funds. Specifically talking about
  igalia as we have a track record with them.

* Alex: *this is alex's own best recollection of his comments*
  I'd propose we take shipping p3 sooner off the table here in terms of
  contracting motivations. Too unlikely that contractor can get up to speed on
  context necessary to do anything critical for p3, best we can hope for is
  freeing up time for other folks.

* Till: I do disagree a bit and I think we can ship more confidently. Why I keep
  talking about the test suite. Very closely related if someone took on project
  to start hashing out some of the most important APIs how do we describe the
  semantics of the API. For wasi-http it's going to be some language around
  here's how to map these things to underlying specifications. For wasi-keyvalue
  that's not an option. How do we define the scope. Doesn't include entire
  implementation of key/value store. Similar to creating a test harness and make
  setting it up in a way that allows it to be maintainable instead of having
  individual separate over time harnesses. This'd be something where the focus
  would be building robust foundations. What should the framework even be?

* Bailey: I'm hearing general agreement that investing in compliance testing
  would be a good place to put dollars? WASI semantics and expanding that? Think
  igalia could do it? If we commit we'd need partners to spend time and make
  sure we designed it correctly. Timing that would be something. We've been able
  to rise to the occasion if we can commit engineering resources. Does sound
  like something that needs to be designed. Anyone with suggestions of who to
  reach out with first? igalia? Take a step back and someone like Conrad? Unsure
  which way to go.

* Chris: my take is we need both. Agree building out tests would be huge boon.
  Short/long term thing. Also need investment with Conrad to convince ourselves
  this big async thing actually works correctly.

* Alex: *this is alex's own best recollection of his comments*
  I'm still wary of even testing requiring a lot of context for example we can't
  expect a specification of key/value + filesystems + http + components all at
  the same time when we ourselves can't boot up on all of that.

* Till: would contend the infrastructure parts align well with what igalia has
  done. Would make sense to focus on that. Would make it easier to make us think
  about how we do specific things. Not saying this is free! One thing we haven't
  talked about is that there's no free lunch. No matter what we do someone's
  gotta spend time on defining the project and scope. Need to work with
  contractors on getting it in and reviewed and such. There's a resource cost.
  If anything even if we're talking about shipping more confidently for WASIp3
  it'll time at a time cost almost certainly.

* Bailey: can't come up with a complete cohesive plan. I'll think about the
  compliance effort and try to put together a draft. Maybe for alex
  understanding some types of projects that would reduce burden on you. I wrote
  down wit-bindgen but unsure. If we could take on those types of things would
  be important to know.

* Alex: *this is alex's own best recollection of his comments*
  I'm definitely attempting to be more vigilant at asking others to take on
  tasks.

* Bailey: item after this one?

* Alex: There's an LTS RFC!

* Pat: I support! Thanks!

* Pat: Announcement: I'm told to do more OSS BA stuff. One thing that's been
  lacking is cargo-component after Peter left (we miss Peter!). Gonna spend more
  time and pick up maintenance about that. Talk to me if interested! Thanks to
  F5 too!

* Andrew: Back to contracting: Bailey hopefully this gave more insight into what
  we talked about earlier. What are next steps for if we're doing the compliance
  approach or other things? If people have more comments when would they need to
  have those in by?

* Bailey: Before next tuesday? By next Tuesday I'll share with Andrew/Oscar
  samples of what work could be. See if contractors would be interested. Would
  be most interesting one to take on right now I think? Will follow-up with
  board member raising this issue. Will level-set with my understanding of what
  contributors feel.

* Nick: Triage time!

<no notes for triage>
