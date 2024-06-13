## SIG Registry in Valencia

Central facility to store multiple signatures
    - A signature communicates that I vouch for something.
    - What are they vouching for when they create what they are vouching for?
        - Builder voucher
        - Owner's signature
            - I have the legal rights to distribute this software
            - I approve the use of this software in a regulated context

### Signed attestation

- log of statements of abilities with a signature attached to those statements of abilities or do you have a core set of "profile-y" things

  - Crypto stuff fits all of their data formats

  - The term used in both JWS and SAML is assertions
    Making assertions about an package
    You can publish some software, I've done QE process on this thing and then provide a stamp of approval
    There are cross-organization
    Intoto - project in CNCF
    Intoto insertion is a link

    They use attestations
    It is an assertion
    In intoto it's a link and then signatures go over them

    With a stress of a signature about the events

    Cross-divisions and businesses
    That makes sense

    Profian publishes a workload or some component of some kind that handles cryptographic key material
    Goes through FIPS
    FIPS could sign an event in our registry
    Now everyone in our world, is this FIPS validated

    Provenance

    The latest signature in the log
    Roll back but can't fork

    Ability at any later stage, add links that refer to some previous links
    You might inherit an artifact at some stage
    That is there and I don't care where
    What set that you want to sign

    Worried about complexity problem that is a set of assertions that are not comprehensive. How to do a union of those sets?

    Some users will be interested in a subset of signatures
    They can see it and go back and look at it
    These things must have happened and probably not in that order

    We can create a query language for this as a timeline. There's always a head of this append-only log
    Read at any point in the log.

    ON read, what are you extracting for your work.

    Ability to attach assertions and signatures and identities

Don't want two sets of signatures to query on, want one set of signatures to query on.

Whatever we do for signatures, single place for query, find one or embed one
Closely relationship with the registry

Query different things with the API
I want to have the particular interface, I want to satisfy

TUFF

We've just redesigned the publish API

Publishing is the start of this append only log

If we support the CNAB spec, then we've essentially made an OCI registry. Is this bad?

Sarah: How does this all fit into all of the security foundation open source

One of the things we're trying to do is design two things at the same time.

One is the data model that we are all going to require over time
As well as all of the operations that we need to do given this data.

We haven't made any decisions here, just creating a straw person.

We've talked about having multiple registries, now we're talking about append logs
Whether we like it or not, we have to deal with this sooner or later.

Evolving legal regulations, whether or not its encrypted.
On append only things, if it's mutable its there its there.
Breaks GDPR if we have anything that can go back to an individual person.

Legislation that is not as articulate

If your name goes into a ledger for someone that committed something, how do we fix that?

If you are going to make this assertion, you are agreeing to this assertion.

Free to publish and not those properties.

Whether doing it with a bot mechanism, does that mean it will be bounced via EULA.

One of the critical failures of OCI registries is that it was built by a central model.

That's failing the ecosystem right now and has monetary consequences.

Create a policy that we pursue and test out in the early stages, see if it gives us what we want. Aware of the problem early.
We this SIG group.

Move on to defining what our particular concerns are for a core case of creating a registries

Before we move on to all of the stages for what we do before we standardize

I've always been thinking about the more we formalize the API and semantics, if we've done our job right at this level of assertions, attestations, receipts, etc.

We might be able to think coherently about grabbing a tree and move it.

OK we think this is sort of working and running, now what does that mean?

Tyler: Has anyone thought about this problem as a bounded-lattice problem.  Many different things, writing and reading, we had a movable merge system.

Syndication rather than reconnection.

Decentralized consensus

We don't have loop around and get merge back again
They are merkle trees
DAGs not complete decentralized where trees

Do we have a use-case for this. Doing a merge of some kind.

Action item: DAG-y-ness. You can do these joins when doing two distinct things. Restricted to avoid the hard problems.

### Failure Modes

Solve failure cases when they come up. We have a link to a particular artifact for a particular release,
contents no longer match the hash and how do we prevent pollution of the registry.

Known bad data that we can't remove.

Publish an entry into a registry, contains to a link to an artifact that I control. Then all of a sudden I delete it from my server.

ACL => the problem exists, there are failure modes that will result from that, that may be possible to recover from.
There is one case where we can detect a failure. We get one file, but the bits are changed, and can no longer construct the file.

How do we keep the registry clean?

In current publishing proposal, link to that object.

The object cannot be obtained and is therefore in an unknown state.

If we have a registry that allows querying addresses, that allows publishing across a million downloads.

Everything in the registry is potentially correct, potentially verifiable. The problem is that its incomplete
If we upload the metadata with the data.

That way the server can always validate whether the publish is correct.

Package has a particular state, its published, yanked which means it's no longer supplied in queries.

The person with privileges of this kind, took an action to say they don't want this anymore.

Deleted. Fetching would also be possible.

Tombstoning, we detected some sort of quality problem, no action taken by the registry owner, in some invalid state, should therefore not anymore.

It must always be perfect, even if we encounter physical problems with the registry, something might still fail. We need this escape hatch.

Might need a plan for managing these tombstones.
Monitoring.

Let's say the registry, even in the failure state, tombstones might still be valid for legal take-down case.

Matrix: we have two states, two actors. We have the actor and the package owner, and the registry.

Another use-case is that I might actually have a prolonged test environment, more complex formulations of my entire solution, I'm not going to actually say this is part of the registry. This is my publish state, in that env I can see people wanting to working against an unvalidated state. The binary is in an S3 bucket. Potentially validatable but only by a user.  What they are trying to do is some developmental implementation.

Packages are immutable. Doing queries against the previous version.

In crates.io, you can publish a release, everything is immutable, if you try to test as a release of the software.

Initial state is published, the

Get it as a part of a manual fetch operation, it will never

Banish, stronger than deleted.
Or it can move into the published state.

Caches on the client that this could make this wrong.

Some other designator, I only want this published thing. You have to switch to this published thing.
Roman: are people going to abuse this to have latest. Now everyone is suddenly going to have latest.

Clearly communicating to everyone that this is not the thing you want.
