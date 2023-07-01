---
title: "📝 Software Engineering at Google notes"
date: 2023-07-01
draft: false

tags: ["Software Engineering"]
---

Notes about the [book](https://abseil.io/resources/swe-book).

<!--more-->


### What Is Software Engineering?
> Software engineering is programming integrated over time.

> With a sufficient numbers of users of an API,
it does not matter what you promise in the contract: all observable behaviors of your system
will be depended on by somebody. - Hyrum's Law

> It's programming if 'clever' is a compliment, but it's software engineering if 'clever' is an accusation.

> If a product experiences outages [...] but the issue wasn't surfaced by tests in CI, it is not the fault of the 
infrastructure change. [...] "If you liked it, you should have put a CI test on it."

> The more frequently you change your infrastructure, the easier it becomes to do so.

> Finding problems earlier in the developer worklow usually reduces costs.

Costs: **Financial** (money), **Resource** (CPU), **Personnel** (effort), **Transaction** (to take action),
**Opportunity** (cost to not take action), **Societal** (impact on society)

> Jevons Paradox: consumption of a resource may increase as a response to greater
efficiency in its use.

> Software is sustainable when, for the expected life span of the code,
we are capable of responding to changes in dependencies,
technology, or product requirements.
We may choose to not change things, but we need to be capable.

> Most decisions are based on a mix of data, assumption, precedent,
and argument. It’s best when objective data makes up the majority of
those inputs, but it can rarely be all of them.
Being data driven over time implies the need to change directions when
the data changes

### How to Work Well on Teams
> You're probably not a genius.

> The chances of an early misstep are high.
The more feedback you solicit early on, the more you lower this risk.
[...] “Fail early, fail fast, fail often.”

> Bus factor: the number of people that need to get hit by a bus before your project is completely doomed.

> Programmers work best in tight feedback loops: write a new function, compile. Add a test, compile. Refactor some code, compile.

> Software engineering is a team endeavor.

> Pillar 1: Humility, 
> You are not the center of the universe (nor is your code!). You’re neither omniscient nor infallible. You’re open to self-improvement.
> 
> Pillar 2: Respect, 
> You genuinely care about others you work with. You treat them kindly and appreciate their abilities and accomplishments.
> 
> Pillar 3: Trust,
> You believe others are competent and will do the right thing, and you’re OK with letting them drive when appropriate.

> Do not underestimate the power of playing the social game. It’s not about tricking or manipulating people;
it’s about creating relationships to get things done.

> “Hey, I’m confused by the control flow in this section here.
I wonder if the xyzzy code pattern might make this clearer
and easier to maintain?”

> A good postmortem should include the following:
> - A brief summary of the event
> - A timeline of the event, from discovery through investigation to resolution
> - The primary cause of the event Impact and damage assessment
> - A set of action items (with owners) to fix the problem immediately
> - A set of action items to prevent the event from happening again
> - Lessons learned

> Googleyness:
> - Thrives in ambiguity: can deal with conflicting messages or directions
> - Values feedback: has humility to both receive and give feedback gracefully
> - Challenges status quo: Is able to set ambitious goals and pursue them
> - Puts users first: Has empathy and respect for users
> - Cares about the team: Has empathy and respect for coworkers
> - Does the right thing: Has strong sense of ethics about everything they do

> If you want to work effectively with a team or a large organization, be aware of your preferred working style and that of others.

### Knowledge Sharing
> Challenges
> - Lack of psychological safety
> - Information islands
> - Information fragmentation
> - Information duplication
> - Information skew

> Software engineering can be defined as the multiperson development of multiversion programs.

| Cooperative | Adversarial |
| --- | --- |
| Basic questions or mistakes are guided in the proper direction | Basic questions or mistakes are picked on, and the person asking the question is chastised |
| Explanations are given with the intent of helping the person asking the question learn | Explanations are given with the intent of showing off one’s own knowledge |
| Responses are kind, patient, and helpful | Responses are condescending, snarky, and unconstructive |
| Interactions are shared discussions for finding solutions | Interactions are arguments with “winners” and “losers”

> - No feigning surprise
> - No well-actually’s
> - No backseat driving
> - No subtle -isms

> Always be learning; always be asking questions.

> Seek out and understand context, especially for decisions that seem unusual.

> The first time you learn something is the best time to see ways that the existing documentation and training materials can be improved.

### Engineering for Equity
...

### How to Lead Team
...

### Leading at Scale
...

### Measuring Engineering Productivity
...

### Processes
...

### Style Guides and Rules
...

### Code Review
...

### Documentation
...

### Testing Overview
...

### Unit Testing
...

### Test Doubles
...

### Larger Testing
...

### Deprecation
...

### Tools
...

### Version Control and Branch Management
...

### Code Search
...

### Build Systems and Build Philosophy
...

### Critique: Google's Code Review Tool
...

### Static Analysis
...

### Dependency Management
...

### Large-Scale Changes
...

### Continuous Integration
...

### Continuous Delivery
...

### Compute as a Service
...
