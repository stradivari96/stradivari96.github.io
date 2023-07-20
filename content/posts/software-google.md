---
title: "📝 Software Engineering at Google notes"
date: 2023-07-01
draft: false

tags: ["Software Engineering"]
---

Notes about the [book](https://abseil.io/resources/swe-book).

<!--more-->


### What Is Software Engineering?
> Programming integrated over time.

> With a sufficient numbers of users of an API [...] all observable behaviors of your system
will be depended on by somebody. - Hyrum's Law

> It's programming if 'clever' is a compliment, but it's software engineering if 'clever' is an accusation.

> If a product experiences outages [...] but the issue wasn't surfaced by tests in CI, it is not the fault of the 
infrastructure change.

> "If you liked it, you should have put a CI test on it."

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
> Bias is the default.

> Diversity is necessary to design properly for a comprehensive user base.

> Inclusivity is critical not just to improving the hiring pipeline for underrepresented groups, but to providing a truly supportive work environment for all people.

> Product velocity must be evaluated against providing a product that is truly useful to all users. It’s better to slow down than to release a product that might cause harm to some users.

> Don't build for everyone. Build with everyone.

> Design for the user who will have the most difficulty using your product.

### How to Lead Team
> An engineering manager is responsible for the performance, productivity, and happiness of every person on their team— including their tech lead— while still making sure that the needs of the business are met

> The tech lead (TL) of a team— who will often report to the manager of that team— is responsible for the technical aspects of the product,

> If an individual succeeds, praise them in front of the team. If an individual fails, give constructive criticism in private.

> Antipattern: Hire Pushovers
> 
> You should strive to hire people who are smarter than you and can replace you.

> Antipattern: Ignore Low Performers
> 
> If you immediately deal with a low performer, you’ll often find that they merely need some encouragement or direction to slip into a higher state of productivity.
If you wait too long to deal with a low performer, their relationship with the team is going to be so sour and you’re going to be so frustrated that you’re not going to be able to help them.

> Antipattern: Ignore Human Issues

> Antipattern: Be Everyone’s Friend
> 
> It can be tricky to move into a management role over someone who has been a good friend and a peer. [...]
> We recommend that you avoid getting into this situation whenever possible, but if you can’t, pay extra attention to your relationship with those folks.

> Antipattern: Compromise the Hiring Bar
>
> A team needs to hire 5 engineers, so it sifts through a pile of applications, interviews 40 or 50 people, and picks the best 5 candidates regardless of whether they meet the hiring bar. This is one of the fastest ways to build a mediocre team.

> Antipattern: Treat Your Team Like Children
> 
> If you hire people worthy of trust and show these people you trust them, they’ll usually rise to the occasion.

> Lose the Ego
> 
> Part of “losing the ego” is trust: you need to trust your team.

> Be a Zen Master
> 
> Mediating your reactions and maintaining your calm is more important as you lead more people.
> Zen management trick: asking questions.

> Be a Catalyst, Remove Roadblocks, Be a Teacher and a Mentor, Set Clear Goals, Be Honest, Track Happiness

> Delegate, but get your hands dirty. Seek to replace yourself. Know when to make waves. Shield your team from chaos.
> Give your team air cover. Let your team know when they're doing well.

> Delegate where possible; don't DIY.

### Leading at Scale
> Always be Deciding, Always be Leaving, Always be Scaling

> Reevaluate and rebalance the trade-offs again; it's an iterative process.
This is what we mean when we say *Always Be Deciding*.

> Your Mission: Build a "Self-Driving" Team

> Ask yourself: *What can I do that nobody else on my team can do?*

> It's also important to "manage up", making sure your management chain understand what your group
is doing and staying connected to the company at large.

> 95% observation and listening, and 5% making critical adjustments in just
the right place.

> Anchoring a team identity to a specific solution can lead to all sorts of angst over time.
(should own the problem instead)

> Your organization is scaling by tackling new problems and then figuring out how to compress them so that
it can take on new, parallel struggles.

> If you let yourself slip into pure reactive mode (which happens almost automatically), you spend every moment of your life on urgent things, but almost none of those things are important in the big picture.

> First, even if you don’t delegate that middle 60% of tasks, your subleaders often notice and pick them up automatically. Second, if something in that middle bucket is truly critical, it ends up coming back to you anyway, eventually migrating up into the top 20%.

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
> - They tend to be small according to Google’s definitions of test size.
> - They tend to be easy to write at the same time as the code they’re testing,
> - They promote high levels of test coverage because they are quick and easy to write.
> - They tend to make it easy to understand what’s wrong when they fail
> - They can serve as documentation and examples.

> we encourage engineers to aim for a mix of about 80% unit tests and 20% broader-scoped tests.

> brittle: they broke in response to a harmless and unrelated change that introduced no real bugs. Second, the tests were unclear: after they were failing, it was difficult to determine what was wrong,

> the ideal test is unchanging: after it’s written, it never needs to change unless the requirements of the system under test change.

> When an engineer refactors the internals of a system without modifying its interface, whether for performance, clarity, or any other reason, the system’s tests shouldn’t need to change.

> As with refactorings, a change to existing tests when adding new features suggest unintended consequences of that feature or inappropriate tests.

> the presence of the bug suggests that a case was missing from the initial test suite,

> Changing a system’s existing behavior is the one case when we expect to have to make updates to the system’s existing tests.

> The takeaway is that after you write a test, you shouldn’t need to touch that test again as you refactor the system, fix bugs, or add new features.

> write tests that invoke the system being tested in the same way its users would; that is, make calls against its public API rather than its implementation details

> Test State, Not Interactions: we tend to prefer the use of real objects in favor of mocked objects, as long as the real objects are fast and deterministic.

> A clear test is one whose purpose for existing and reason for failing is immediately clear to the engineer diagnosing a failure.

> A test is complete when its body contains all of the information a reader needs in order to understand how it arrives at its result. A test is concise when it contains no other distracting or irrelevant information.

> rather than writing a test for each method, write a test for each behavior.

> Behaviors can often be expressed using the words “given,”“when,” and “then”

> When writing such tests, be careful to ensure that you’re not inadvertently testing multiple behaviors at the same time. Each test should cover only a single behavior, and the vast majority of unit tests require only one “when” and one “then” block.

> A test’s name should summarize the behavior it is testing. A good name describes both the actions that are being taken on a system and the expected outcome

> multiplyingTwoPositiveNumbersShouldReturnAPositiveNumber multiply_postiveAndNegative_returnsNegative divide_byZero_throwsException

> in test code, stick to straight-line code over clever logic, and consider tolerating some duplication when it makes the test more descriptive and meaningful.

> A good failure message contains much the same information as the test’s name: it should clearly express the desired outcome, the actual outcome, and any relevant parameters.

> Good tests are designed to be stable, and in fact you usually want them to break when the system being tested changes. So DRY doesn’t have quite as much benefit when it comes to test code.

> Instead of being completely DRY, test code should often strive to be DAMP —that is, to promote “Descriptive And Meaningful Phrases.”

> DAMP is not a replacement for DRY; it is complementary to it. Helper methods and test infrastructure can still help make tests clearer by making them more concise, factoring out repetitive steps whose details aren’t relevant to the particular behavior being tested.

> Using helper methods to construct these values allows each test to create the exact values it needs without having to worry about specifying irrelevant information or conflicting with other tests.

> One risk in using setup methods is that they can lead to unclear tests if those tests begin to depend on the particular values used in setup.

> Tests like these that explicitly care about particular values should state those values directly, overriding the default defined in the setup method if need be.

> The best validation helper methods assert a single conceptual fact about their inputs, in contrast to general-purpose validation methods that cover a range of conditions.

> - Strive for unchanging tests.
> - Test via public APIs.
> - Test state, not interactions.
> - Make your tests complete and concise.
> - Test behaviors, not methods.
> - Structure tests to emphasize behaviors.
> - Name tests after the behavior being tested.
> - Don’t put logic in tests.
> - Write clear failure messages.
> - Follow DAMP over DRY when sharing code for tests.

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
