---
date: 2025-03-24
---
<meta property="og:published_time" content="2025-03-24">

# Automated Tests or Documentation? Why Not Both.

I write [Elixir](https://elixir-lang.org)/[Ash](https://ash-hq.org) apps at [Alembic](http://alembic.com.au). Here is my approach to automated testing, very much influenced by BDD (behavior driven development) and focused on heuristics like “does this make me feel more confident implementing this?” rather than metrics like “have we achieved X% test coverage?”. Also we aim to have improved documentation and accessibility as by-products, using these as lenses to guide writing our tests.

## Ask: what am I wanting to gain by writing this test?

- To ensure this functionality won’t break in the future (regressions)?
- To document how the implementation works for my team to learn (behaviour)?
- To automate a bunch of manual testing I’d otherwise have to do (efficiency)?
- To gain confidence that the system actually works as I imagine it works (behaviour)?
- To increase some sort of coverage metric (conformance)?

Before writing the test decide what you and your team gains from having the test. Having more tests isn’t necessarily better. The “wrong” test can be expensive if it has to be rewritten all the time, often fails in CI, or verifies things of little value.

## Ask: what is the interface to what I’m implementing?

- A user interface that runs in the browser?
- A network API for clients to call?
- A SMS or Email sent or received
- A programatic interface that is called by myself or other coders?

Imagine how what you are implementing is then used. Who or what uses it? This then tells you how to test it. Your test puts your implementation through its paces through its interface and checks it behaves how you expect.

- A browser UI should work in a browser with a URL, render semantic accessible HTML, and respond to user events.
- A network API should speak a protocol (Restful HTTP/GraphQL/etc) with the expected responses for particular requests.
- A programatic API should be practical (i.e. not require hundreds of lines of boilerplate) and demonstrably work for its intended use cases.

Types help check the interface has a certain shape, automated tests check the interface behaves a certain way.

## Tests should educate a reader on the system and the world around the system

- The domain the system lives in with its ubiquitous language.
- The wider experience the UI is a part of. For example, if this is part of a flow of emails and web page forms, document that in a test.
- The business and market context.
- The complexities inherent in these types of systems, and the decisions this application has made.
- The database records and other pieces of state necessary for my code to function.

## Tests should be without branching or clever abstraction and be simpler than the implementation under test

- If a test has conditionals then how do you know which conditions are triggered when? How do you know the test does what you think it does? Do you then need to test the test?
- If a test has multiple levels of abstraction, again how do you know it resolves to what you think it resolves to?
- Tests aren’t a place for clever code or heavy abstraction, as they those can then become liabilities when the system doesn’t work like people assumed it did, or it becomes unmaintainable spaghetti.
- Elixir tip: Use `@tag`, `@describetag` and `@moduletag` to change inputs to a test. If your inputs require inserting in the database or mocking, then use a `setup` function instead. Prefer to have variations with discrete `setup` functions over conditional logic that changes depending on other inputs.
- Rather than worrying about “do I want more of a unit-test mocked approach or integration test” and looking at the testing pyramid, write the test that gives you the most confidence for what you are testing for, and then spend effort on making all the tests fast.
- “Unit testing” of an internal system can bind you to that internal system’s workings. Identify the interface you intend others to use, and commit to that interface by writing a test against it.

## Tests are like a scientist running an experiment and then making a bunch of observations and verifying those match expectations

- Tests should declare given this scenario or this state is in place, when I do this, then I’d expect to observe this. Use `describe` for each “given”.
- Ask “if I were to observe this it’d be nearly impossible for to get to this end state any other way”. e.g. If I have seven orange and three white muffins and I decide to eat all the orange ones then it‘d be really hard to observe there are three muffins total left over in any other way than removing the seven orange ones.
- If a test is confident that the implementation should have results in a particular way, then put that in the test. The test is a contract, and it‘s desirable that if the implementation’s contract changes, then the test should too. Equally, if something trivial changes or the implementation is refactored, then it’s desirable that tests remain unaffected by such a change.
- Tests should pin an implementation down to acting a particular way and then make refactoring that implementation frictionless and pleasant because you are assured via the tests passing that it still all works the same as it did before. Tests’ role is to allow making changes to existing code quick & painless rather than brittle & time consuming.
- Tests should be interested in the output (we filtered the list of pets to dogs and don‘t see any cats) rather than how it got there (e.g. it should _not_ be interested in: it filtered using the database vs in memory, it used 3 modules vs 5, it requires these particular actions to be run in this order).
- Chaos monkey — disable a bit of logic in the implementation and see if the test still passes. Purposefully make a part faulty and see how it affects the end result. Automated tests help you run little experiments to see how the system reacts in certain scenarios. You don’t have to commit these experiments into your test suite, just use them as playgrounds for “what if” scenarios.

## Look for ways to avoid tests

- Avoid tests altogether by writing declarative code that is either so obvious it can’t break (e.g. lookup tables or the simplest pattern matching) or is backed by a reusable imperative system that has been tested itself throughly (e.g. a declarative framework).
- Use types to get the compiler to verify ahead of time that certain states and code paths aren‘t even possible.
- Use assertions/guards to confidently declare “the system can’t ever even get into an unexpected state” and use CI/monitoring to get developers to fix surprises immediately.
- It’s perfectly ok to retire tests if they are getting in the way and you have confidence the system works through other means (e.g. types, redundant tests, manual testing).
