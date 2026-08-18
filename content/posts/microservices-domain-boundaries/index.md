---
date: 2026-08-18
description: "Microservices are often blamed for becoming a maintenance nightmare, and the usual fix is to merge everything back into a monolith. Drawing on a real experiment that split services down to one per endpoint before consolidating around domain-owned boundaries, this post argues the failure was never service size, it was skipping the domain modeling that tells you where boundaries actually belong."
image: "/posts/microservices-domain-boundaries/images/microservices-one-per-endpoint-shared-library-versioning.png"
lastmod: 2026-08-18
showTableOfContents: false
tags: ["software engineering", "architecture", "microservices"]
title: "Microservices Didn't Fail Us. Our Domain Boundaries Did"
type: "post"
---

![Microservices split one per endpoint with a shared library versioned across them](/posts/microservices-domain-boundaries/images/microservices-one-per-endpoint-shared-library-versioning.png)

Anyone who works in IT has heard some variation of "microservices went too far" at least once. And usually the conclusion is that the solution is to go back to a monolith.

That has never sat right with me.

We ran the experiment, hit the wall, and walked back. But what we walked back to wasn't a monolith. It was something smaller than that idea usually suggests, and the reason it worked had very little to do with service count.

## The experiment: one service per endpoint

In a client's project, we ran a deliberate test. Instead of grouping functionality by domain, we split it as far as it would go: something close to one service per endpoint.

Get customer. Update customer. List customer orders. Each one its own deployable unit.

The idea was independence taken to its logical conclusion. If small is good, smaller should be better, and the smallest possible unit should also be the most independent one. It also seemed like a natural fit for serverless hosting, with all the obvious advantages that came with it. Obvious advantages, with some hidden and particularly painful details.

It fell apart fast, and not for the reasons people usually blame. Latency was fine. Infrastructure cost was manageable. The problem was the model underneath all of it.

A "get customer" service and an "update customer" service both need to know what a customer is: its fields, its validation rules, its invariants. So we did what seemed natural in that situation. We pulled the shared logic into shared libraries.

Business models, entities, validation rules, all versioned and imported across a quickly growing number of services.

That decision quietly cancelled out much of the independence we were trying to create. The problem wasn't the existence of a shared library itself; it was that the library carried the Customer model, its rules, and its invariants across multiple independently deployed services.

The moment the Customer model changed, it was no longer a one-service change. It became a shared-library version bump propagated across every service that depended on it, each on its own release cycle and each potentially running a different version. We had more deployable units than ever, and less deployment independence than we started with.

## Small was never the same thing as decoupled

This realization isn't new; in fact, it's a foundational software design principle that we simply forgot. This is the part the standard "microservices went too far" narrative usually skips. It treats the lesson as "we made too many services," and the fix as "make fewer, bigger ones." That is a real pattern, but it is not the mechanism. The mechanism is boundary placement.

An endpoint is not a domain boundary. A database table is not a domain boundary. A team's sprint scope is not a domain boundary.

When you split services, you still need some sort of glue to keep things consistent somewhere. And that "somewhere" often becomes a shared library, a shared schema, or a coordination ritual between teams.

The services get smaller. The coupling doesn't go away. It just moves somewhere less visible, and often harder to manage, than an in-process function call.

This is what Eric Evans' original bounded context concept (_Domain-Driven Design: Tackling Complexity in the Heart of Software_, 2003) is actually for, and it's worth being precise because the term gets used loosely: a **bounded context is the boundary within which a model is consistent and has clear responsibility for how that model evolves**.

Outside that boundary, the model does not apply in the same way, and interactions cross an explicit contract instead of relying on a shared assumption.

Domain-Driven Design was never a competing architecture style to microservices. It's a way of finding where a boundary can actually hold weight. Ignore that, and you can build fine-grained microservices, a modular monolith, or anything in between and still get the same result: pieces that look independent on a deployment diagram but behave like one tightly coupled system in practice. The shape people usually call a distributed monolith.

## What we consolidated to

![Domain-owned miniservice compared to shared-library fragmentation](/posts/microservices-domain-boundaries/images/domain-owned-miniservice-vs-shared-library-fragmentation.png)

After the endpoint-level experiment, we moved to services aligned with coherent domain boundaries: a Customer service, an Order service, a Billing service, and so on.

These were not simply services named after entities. Each represented a cohesive area of responsibility with its own model and rules. No shared library carrying the Customer entity across five deployables.

If you needed customer data, you asked the Customer service through an explicit contract rather than importing its internal representation. The dependency was still there, but it was now visible, intentional, and owned at the service boundary.

I've seen this pattern called a miniservice, and I personally like to call it a microlith (really only to make a bit of fun of monoliths).

Neither term has a settled definition industry-wide, so I want to be specific about what I mean here: a service sized to a single bounded context, not to a table, not to an endpoint, and not to a team's current headcount.

It's smaller in scope than what some people call a macroservice, more independently deployable than a module inside a modular monolith, and closer in spirit to a self-contained system: a unit that owns its data, its logic, and its lifecycle end to end.

The difference this made had very little to do with having fewer moving parts. We still had several services. They were still deployed independently. They still scaled independently where it mattered. What changed was that a model had one home.

Changing how a Customer worked was a Customer service change, full stop. No shared domain library to version. No coordinated redeployment across services just because the internal Customer model evolved.

That's the actual trade being made when you size a service around a domain boundary instead of a technical seam.

You give up some theoretical flexibility as you can no longer redeploy "get customer" without also redeploying "update customer", in exchange for a boundary that holds under change, because it matches where the model's rules, behavior, and consistency requirements actually live.

## Where finer-grained still earns its keep

None of this means smaller services are wrong on principle.

There are real reasons to split below a domain boundary: a read path with wildly different scaling needs from the write path, a component that needs hard tenancy or compliance isolation, or a sub-team that genuinely needs its own release cadence for regulatory reasons. Those are all legitimate justifications.

But notice the test in each case: the split is still justified by something in the domain or in the operating constraints, like a consistency boundary or a scaling profile.

It isn't justified by "smaller services are inherently more modern" or "let's give every team its own repo". The domain analysis comes first. The service boundary is a consequence of it, not a target set in advance.

## The actual lesson

If your microservices architecture has turned into a maintenance burden, the honest question isn't "are our services too small"? It's "did we ever do the domain modeling that was supposed to tell us where the boundaries belong"?

Often, the answer is no. The boundaries were drawn around whatever was easiest to see: the database schema, the REST resource list, the org chart.

Domain-Driven Design is not the alternative to microservices. It's the step that often gets skipped before the services are cut. And consolidating into fewer, domain-owned services is sometimes simply what it looks like when you go back and do that step properly.

The right size for a service was never small. It was always the size of the domain it owns.