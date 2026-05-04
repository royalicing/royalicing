---
date: 2026-02-04T06:57:38Z
---
<meta property="og:published_time" content="2026-02-04T06:57:38Z">

# Software is Made of Fickle Loops

You can use the lens of iterative loops to look at software development today. We continuously run tests in CI, we regularly update dependencies, our web apps are loops turning HTTP requests into responses, and user input into UI updates and database writes.

What makes software development hard is that these loops are fickle: they are prone to failure, delays, and uncertainty.

Let’s go through them:

## Code

| Loop                                                                          | Steps                                                | Fickleness                                                                                                                                        |
| ----------------------------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| Code compilation is a fickle loop of source code into executable instructions | `edit → compile → link → run`                        | <ul><li>syntax errors</li><li>type errors</li><li>missing dependencies</li><li>configuration mistakes</li><li>language breaking changes</li></ul> |
| CI is a fickle loop of flaky tests on a remote machine                        | `install → build → test → fail… → fix… → pass`       | <ul><li>flaky tests</li><li>slow feedback</li><li>dependency drift</li></ul>                                                                      |
| Dependencies are a fickle loop of independent code changes                    | `new release → upgrade → fix breakages → git commit` | <ul><li>transitive dependencies</li><li>ecosystem churn</li></ul>                                                                                 |
| Code review is a fickle loop of human opinions                                | `pull request → review… → revise… → merge`           | <ul><li>human delays</li><li>inconsistent standards</li><li>unclear feedback</li></ul>                                                            |
| Tech debt is a fickle loop of labor applied to a running software system      | `add features → fix bugs → meet goals → hire staff`  | <ul><li>changing requirements</li><li>staff turnover</li><li>lack of system understanding</li></ul>                                               |

## Networks & storage

| Loop                                                                                    | Steps                                                           | Fickleness                                                                                                        |
| --------------------------------------------------------------------------------------- | --------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Databases are a loop of writes with durability guarantees and loops of consistent reads | `connect → open → write → commit → replicate`                   | <ul><li>network partitions</li><li>crashes</li><li>conflicts</li></ul>                                            |
| Web servers are a fickle loop of HTTP requests                                          | `request → handle → cache → respond`                            | <ul><li>timeouts</li><li>partial failure</li><li>server load</li><li>poor Wi-Fi or cellular connections</li></ul> |
| RPC is a fickle loop of requests over unreliable networks                               | `request → timeout → retry/backoff → acknowledged success/fail` | <ul><li>timeouts</li><li>network partitions</li><li>server overload</li></ul>                                     |
| TCP is a fickle loop of packets over unreliable networks                                | `send → ack/loss → retransmit/adjust`                           | <ul><li>packet loss</li><li>network congestion</li><li>variable latency</li></ul>                                 |

## Product & users

| Loop                                                                                     | Steps                                                               | Fickleness                                                                                                                                                        |
| ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Lean startup is a fickle loop of validating product-market fit                           | `build → measure → learn`                                           | <ul><li>misleading or lagging proxy metrics</li><li>shifting market conditions</li><li>changing user behavior</li><li>conflicting management incentives</li></ul> |
| Double-diamond is a fickle loop of exploring & narrowing problem/solution space          | `discover → define → develop → deliver`                             | <ul><li>noisy signals from research</li><li>feature creep</li><li>conflicting management incentives</li><li>shifting user behavior</li></ul>                      |
| User sessions are a fickle loop of authentication and engagement                         | `login → interact → timeout/logout`                                 | <ul><li>forgotten passwords</li><li>session expirations</li><li>engagement drop-offs</li></ul>                                                                    |
| Subscriptions are a fickle loop of billing and user retention                            | `bill → renew/cancel → plan changes`                                | <ul><li>payment failures</li><li>churn</li><li>pricing and plan changes</li></ul>                                                                                 |
| Enterprise contracts are a fickle loop of negotiations and renewals                      | `negotiate → sign → deliver → renew`                                | <ul><li>changing requirements</li><li>legal reviews</li><li>budget cycles</li><li>feature overload</li></ul>                                                      |
| Accessibility is a fickle loop of design constraints, legal requirements, and user needs | `design → implement → test with users → verify compliance → revise` | <ul><li>diverse user needs</li><li>evolving standards</li><li>limited QA resources</li><li>conflicting best practices</li></ul>                                   |

## Trust

| Loop                                                          | Steps                                                                                        | Fickleness                                                                                                                     |
| ------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| Security is a fickle loop of attack and defense               | `zero-day/attack → detect → mitigate → patch → monitor`                                      | <ul><li>adversarial inputs</li><li>supply chain risks</li><li>human error</li><li>complex systems</li></ul>                    |
| Compliance is a fickle loop of interpreting laws and auditing | `regulation changes → implement → audit → report`                                            | <ul><li>evolving regulations</li><li>ambiguous laws</li><li>regional differences</li><li>more reporting requirements</li></ul> |
| Privacy is a fickle loop of data collection and user consent  | `collect data → anonymize parts → store/analyze → comply with requests & laws → delete data` | <ul><li>changing regulations</li><li>data breaches</li><li>law enforcement requests</li><li>encryption</li></ul>               |

Note how these loops are interlinked: one loop can cascade its problems into another. Faster CI loops often improve code review loops. Web server loops rely on TCP loops. Congestion over the network can cause TCP to be austere compounding into a sluggish UX. An onerous login flow can lead to less product usage and even cancellation. Tech debt slows almost all other loops.

---

## Our loops are becoming yet more fickle

Due to more complicated management of software projects these loops are becoming more encumbered with fickleness. Here are some examples:

- Browsers are more bloated and elaborate.
- Operating systems are more resource intensive and disrespecting of user control.
- Frameworks are larger in scope and have greater complexity over underlying distributed systems.
- Native apps tools are more fragmented and convoluted in their layering.
- Cloud infrastructure is less transparent and more proprietary.
- E2E testing is more brittle with more permutations to test.
- App Store review is more inconsistent and opaque and greedy.
- Users are less attentive and more price-sensitive.
- Regulations are more onerous and densely ambiguous.
- Key platforms are in loops of enshittification: built for the wants of shareholders not needs of users.

## Lubricants for fickle loops

The industry has tried various ways to fix or streamline these problems.

- DORA metrics: measure time to deploy, time to recovery, and change failure rate.
- SRE and SRO: objectively measure end user experience.
- Lean startup: focus on quickly delivering software that has been learned to meet user needs.
- Feature flags: gradually rollout features.
- Over-the-air code push: update code on users’ devices bypassing app store review.
- Observability platforms: provide visibility into user failure within their loops.
- Jepsen: inject faults to find breaks of durability and consistency guarantees.
- Chaos engineering: proactively break parts to test overall system resilience.

Making successful software is to triumph over these fickle loops and create value in the daily loops of users.

## Agentic coding: lubricant or abrasive?

Where does AI coding fit in? Does it accelerate existing loops or is it a new loop in itself? How can it help and where could it go wrong?

| Loop                                                                     | Steps                                 | Fickleness                                                                                                                                                                                                                                                      |
| ------------------------------------------------------------------------ | ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Agentic coding is a fickle loop of using LLMs to write code of all sorts | `prompt → generate → review → deploy` | <ul><li>syntax errors</li><li>remote code execution</li><li>data breaches</li><li>supply chain risks</li><li>incorrect implementation of regulations</li><li>data loss</li><li>flaky tests</li><li>feature overload</li><li>invisible partial failure</li></ul> |

Just like tech debt, AI can affect all other loops. There’s the potential for faster iteration inside existing loops. Loops like CI, dependencies, review, security, testing, design could all increase in efficiency.

But there are also massive risks that fickleness could beamplified through over-reliance on generated code and loss of human oversight and understanding.

I’d argue that understanding all the loops present and sources of fickleness and their failure modes is as important as ever. That’s what senior engineers are on the hook for. This need hasn’t gone away.

We can improve loops by understanding how they work and what they connect to in the real world, making them faster by removing friction, and make failure loud and unambiguous. Ideally we remove unnecessary loops instead of adding more automated loops.

I suggest focusing on loops where humans are involved with agency, such as user experience, security attackers, legal compliance.

The loops haven’t disappeared, they are becoming more abstracted. It’s our duty to see them, understand them, and be accountable no matter how much AI we adopt.
