---
title: 📝 Microservices notes
excerpt: "Some notes about microservices"
read_time: true
share: true
toc: true
related: true
comments: true
tags: [System Design, Software Engineering, Programming]
---

- [Building Microservices](https://www.oreilly.com/library/view/building-microservices-2nd/9781492034018/)

## Microservices

- Indendently releasable/deployable services (loosely coupled, stable contracts).
- Modeled around a business domain (end-to-end slices of business functionality).
- Implementation details are hidden (owning their own state, no sharing of databases, different technologies/try new things, etc).

They allow developers to work independently, reducing delivery contention.
(Technology Heterogeneity, Robustness, Scaling, Ease of Deployment, Organizational Alignment, Composability, etc)
{: .notice--info}

They are not a great idea if the product is brand new or the domain is undergoing significant change.
Also, if the team is small, the time spent on deployment and management might not be worth it.
(Developer Experience, Deployment Overhead, Cost, Reporting, Monitoring, Security, Testing, Latency, Data Consistency, etc)
{: .notice--warning}

## Monoliths

A valid default choice, doesn't mean legacy.

- Single-process monoliths: most common, single process that talks to a database (monolIthic Django, Rails, etc).
- Modular monoliths: single process with separate modules that can be worked on independently ([shopify](https://www.youtube.com/watch?v=ISYKx8sa53g)).
- Distributed monolith: multiple services that must be deployed together (disadvages of distributed systems and single-process monoliths,
  not enough focus on information hiding and cohesion of business functionality).

## Microservice Boundary

- Information Hiding: improve development time by allowing more things to be worked on in parallel,
  allow each module to be understood in isolation and allow modules to be changed independently. The
  important thing is to form good boundaries between modules.

## Cohesion

> The code that changes together, stays together.

We want the functionality grouped in a way that we have to make changes in as few places as possible.
Avoiding the need to deploy multiple services at the same time.

## Coupling

> A structure is stable if cohesion is strong and coupling is low.

Some coupling is inevitable, but we want to minimize it.
A loosely coupled service knows as little as it needs to about the other services.
Limiting the number of different types of calls is also important, chatty communication is a sign of tight coupling.

- Domain Coupling: Largely unavoidable, occurs when one microservice needs a functionality that the other provides.
  If a microservice needs to talk to a lot of other microservices it might be doing too much.
  Also, remember to send only the absolute minimum amount of data needed.
- Temporal Coupling: One microservice needs another to do something at the same time (synchronous HTTP call),
  it isn't inherently bad, but with more microservices it can cause issues if you want to scale.
- Pass-Through Coupling: one microservice passes data to another microservice because it is needed by some other downstream microservice.
  There are some ways to improve this: bypass the intermediary, make the intermediary take in data in its contract and delegate the formatting to it
  or make the intermediary treat the needed data as an opaque blob.
- Common Coupling: two o more microservices use a common set of data (shared database, shared memory or filesystem),
  changing the structure of the data can impact multiple services.
  This can be a big problem if the structure of the common data changes frequently / multiple microservices are reading and writing.
  Who should be responsible of managing the correct lifecycle of the data, do we make a thin CRUD wrapper and reduce cohesion?
- Content Coupling: similar to common coupling but there is no clear knowledge about ownership of the data (eg: one microserve makes changes to a database bypassing the actual service that exposes the data).

## DDD

Important concepts for building microservices:

- Ubiquitous Language: the same language is used by all the stakeholders (developers, business, etc).
- Aggregates: a group of objects that are treated as a unit for the purpose of data changes, typically have a life cycle around them / state machine.
- Bounded Context: organizational boundary, internal concerns should be hidden, make use of hidden models and shared models of associated aggregates.
- Mapping Aggregates and Bounded Contexts to Microservices: you can always star with a coarser-grained API and split into more microservices if needed (implementation detail).
- Event Storming: a technique for building a domain model with both technical and nontechnical stakeholders.

Alternatives:

- Volatility: extract parts that go through a lot of changes (eg: bimodal IT, Mode 1 and Mode 2), not very recommended.
- Data: driven by privacy and security concerns, to reduce risk of data breaches and simplify oversight.
- Technology: this isn't really a general means of decomposition.
- Organizational: driven by the organizational structure of the company, prefer end-to-end vertical slice instead of a horizontal slice.

## Splitting the Monolith

- Microservices are not the goal just a means to an end.
- Incremental migration, identify quick wins, avoid premature decomposition.
- Balance difficulty with benefits.
- Patterns:
  - Strangler Fig: wrap an old system with the new system gradually.
  - Parallel Run: run both and compare.
  - Feature Toggle: feature flags.
- Data Decomposition:
  - Performance: joins?
  - Integrity: cascading?
  - Transactions: distributed transactions, sagas?
  - Reporting database for external reading.

## Communication styles

TODO
