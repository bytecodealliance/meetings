# April 11 project call

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
    1. fitzgen: `cargo vet` and separate PRs for updating lock files given recent attacks?
    1. fitzgen: move caching out of `wasmtime` crate, into a `wasmtime-cache` crate?
    1. telliott: switch the pr description for squashed commits on wasmtime?
    1. _Submit a PR to add your item here_

## Attendees

alexcrichton
elliottt
fitzgen
jameysharp
jlb6740
ricochet
saulecabrera
sunfishcode
tschneidereit

## Notes

* should we switch back to the old workflow for `cargo vet` after the xz attack?
  * alexcrichton - everything about that attack screams nation-state, and
    we can't fully seal ourselves against an attack of that level of effort
  * fitzgen - could we re-require reviews to subsets of files?
  * sunfishcode - can we make the `cargo vet` updates more async?
  * fitzgen - that's what the process was up until now
  * alexcrichton - we also have prs languish that only need a rebase to fix
    issues with `cargo vet`
  * ricochet - can we take over pull requests that are depending on `cargo vet`
    issues?
  * alexcrichton - we could continue landing the vet changes on main, and then
    take the pr over after a week if the contributor disappears?
  * elliottt - could we merge into a separate branch where a contributor lands
    the `cargo vet` changes as well?
  * alexcrichton - this would require some automation to be usable
  * elliottt - can we comment on PRs when there's a `cargo vet` failure from CI,
    with recommended next steps for contributors?
  * alexcrichton - yes, but again it's a question of automationj: we don't want
    a comment for every failure, just the first one
  * jameysharp - if we could enter the merge queue with a red cargo vet, the
    merge would succeed, and would avoid needing the PR to be rebased
  * tschneidereit - we could configure the `cargo vet` task to fail without
    blocking the PR
    * <https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstepscontinue-on-error>
    * tschneidereit - I think it can be combined with conditionals to only
      continue when not running a merge workflow
    * alexcrichton - A problem with that though is the whole job is still green
      when it fails so there’s no great way for a reviewer to be notified that a
      vet is necessary
  * fitzgen - i'll take on the configuration changes to get the messaging about
    `cargo vet` failures
* move caching out of `wasmtime` crate, into a `wasmtime-cache` crate?
  * fitzgen: have the `wasmtime` crate enable the cache by a feature, and
    `wasmtime-cli` use `wasmtime-cache` by default
  * tschneidereit - would this make performance worse for embeddings of wasmtime
    by default?
  * fitzgen - yes, but this is already the case as the cache is already disabled
    when you use `wasmtime` directly
  * sunfishcode - how would this differ from not enabling caching now?
  * jameysharp - making caching a default off feature would avoid the dependency
    in the lockfile
  * fitzgen - it seems like where we're landing is turning off the feature by
    default, and possibly switching to the `deflate` library
  * jameysharp - we should also consider if we want the cache compressed at all
    -- we can't mmap the compressed cache
* switch the pr description for squashed commits on wasmtime?
  * sunfishcode - this feels like it would be a surprising default for folks
    that are used to how github works by default, and it would be nice to not
    make wasmtime too special
  * alexcrichton - ultimately it would be great to choose when putting the PR
    into the merge queue what the description would be, but github doesn't alow
    for that
* alexcrichton - if there's anything anyone wants backported, we'll do releases
  of 17, 18, and 19 today
