# August 27 | Wasmtime Project Bi-Weekly

**See the [instructions](../README.md) for details on how to attend**

## Agenda

1. Opening, welcome and roll call
   1. Find volunteer note taker. Thanks!
1. Announcements
   1. _Submit a PR to add your announcement here_
1. Other agenda items
   1. [Till] What should we do about cap-std?
   1. _Submit a PR to add your item here_
1. Issue Triage
   * [New, Untriaged Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aopen+comments%3A%3C2+created%3A%3E%3D2026-08-13)
   * [Old, Backlog Issues](https://github.com/bytecodealliance/wasmtime/issues?q=is%3Aissue%20state%3Aopen%20sort%3Acreated-asc)

## Attendees

* Alex Crichton
* Dan Gohman
* Pat Hickey
* Nick Fitzgerald
* Roman Volosatovs
* Chris Fallin
* Adam Bratschi*Kaye

## Notes

### Cap-std?

* Alex: we should move it into wasmtime-wasi and then eventually deprecate cap-std
  * Not as a public API, import the tests and import that we use, but delete everything else
* Dan: It's possible that someone would step up to maintain it, but if we want to archive it
  * Maintainers that depend on this do know
  * Alex: Last call for adoption likely going to be posted on Zulip
  * Dan: For the record, I agree with all this and it makes sense
* Dan: what about `wasi:filesystem`?
  * Alex: right now just moving the code without changing impl
  * Dan: Wanted to do this at the same time, but it looks like we should handle them separately
    * We *should* be able to migrate in place? We should be able to merge preopens and come up
* Alex: cap-std removal & backports?
  * cap-std is not a dependency of wasmtime*wasi, in theory we need to support it for one more year
  * changing a public dep *will* be a semver, and cap-std is currently likely exposed (e.g. wasmtime 24 we took a cap-std dir)
    * Pat: looking through docs, there are deps on cap*fs (`SystemTimeSpec`) and cap*rand
  * Nick: there seems to be two options: accept minor breaking change or maintain/keep ownership of `cap-std` until we don't have to (effectively freezing it)
  * Dan: Seems viable to keep it ** people who want to change it can fork it
  * Pat: 48.01 has dep on the `SystemTimeSpec` (shaped like `std`, but *not* `std`)
  * Alex: So we could:
    * vendor & land on main
    * backport the vendored cap-std to 48 & 36
    * land another change on main to actualy delete
* Nick: We should avoid all 3rd party crates in the API
  * Alex: People might use cargo*semver*checks to do this


### Filesystem

* Alex: redoing it means 3 things:
  * Give the guest a VFS (downside: no guest will do this correctly, e.g. preopen to VFS root and then give a path to another VFS root case)
    * Easy for host to implement, hard for guest, and host code (cap-std/wasi-libc) is complicated
    * Dan: code in wasi-libc that does this
      * Alex: it will find the preopen, do the operation, and you can't go across
  * Dan: If you do a .. to get out of a propen, then you resolve to a VFS that has the preopens in it
    * Thinking multi-step:
      * Host changes is no longer an error
      * Host gives you only one preopen (root), guest is wasi-libc is unchanged
      * Update wasi:filesystem spec to note the new required behavior, rip out old behavior and open goes to root
    * Alex: End state would ideally be Guest only getting one preopen
      * Implementing would be difficult, wasmtime has to keep track of 
      * Transition could be
        * broken behaior in wasi-libc goest to wasmtime
        * wasmtime coalesces multiple preopens, and throws error if you go across 
        * it's unlikely that any other runtime to try and sandbox this properly... VFS is hard
        * Dan: full VFS likely looks a lot like Deno
          * Maybe don't do the first step which is putting the broken stuff in wasmtime
          * Skip cap-std, when `open()` happens with a preopen we go straight to VFS lookup?
          * Implementing VFS is still hard, but it's intuitive and likely needs to be done
          * Alex: cap-std being inside of wasmtime does create the possibility of dealing with path resolution in one place
            * trivially we could paths as names and O_NOFOLLOW on everything, would make VFS impl much simpler
            * Guests still perceive as simple root
            * Linux is the only different platform that could support the special case of streamlined single preopen and sandbox access
            * Ideally the goal is to get it so the guests *can* be correct
    * Victor: this won't change much the wasi-virt virtualizability, as far as I can tell?
      * Alex: Yes, this shouldn't change that much if at all, except at the last step if WIT changes
      * Dan: wasi-virt should get simpler without `..` and symlink resolution
      * Alex: this still assumes that you cannot open directories above you (it's kind of implicit in `wasi:filesystem` today)
        * Dan: There is a document in the repo covering this... Sandboxing of .. not going abovey your handle is probably a fine change to make since no one uses that sandbox feature

## Component model

* If a component never lowers anything that modifies thread state, then we don't need to materialize? 
  * When something is "thread transparent" (doesn't use threads), and that uses threads (e.g. which asks for async call stack), but we don't have a materialized thread... Is that OK?
  * Alex: We recently discussed this, and it's actually kind of broken currently -- the intention of the callstack was to give you the host call (at the bottom) 

## Last Old, Backlog Issue

*This tells us where to begin the backlog triage next time.*

TODO


