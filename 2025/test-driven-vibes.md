---
date: 2025-03-30
---
<meta property="og:published_time" content="2025-03-30">

# Test Driven Vibes

Test-driven development (TDD) is the perfect match for vibe coding.

Here’s how it works:

1. Write out a test (don’t generate this part).
2. Check that it fails.
3. Generate the solution.
4. Check if it passes. If not, repeat by generating another solution.
5. Refactor by hand while ensuring tests still pass.

I call it test-driven vibes.

## Why not generate your tests?

Your test is one of the most important parts of your software. It’s like the pre-flight checklist an airplane pilot uses before taking-off. If the checklist is wrong then everything downwind of that may end up wrong.

Your rudder could be stuck. You could be in bad weather. You could be heading to the wrong location. You could be misreading the wind direction.

Wrong checklist and your plane could crash.

It’s the same with your tests. If you don’t have confidence that they are accurate, then you don’t have confidence that anything else downwind of them is accurate.

That’s why you should be mindful of everything that goes into your tests. You should know exactly what they do. You shouldn’t be confused by what’s happening within them, and they should be [free of over-abstraction and conditionals](https://royalicing.com/2025/automated-tests-accessible-documentation) that obscure your understanding.

You can still use AI-assisted code autocomplete to accelerate your typing. But avoid blindly generating your tests because then they are useless. It’s like receiving a contract and signing it without knowing what you’ve just committed yourself to.

## Why do I have to check they fail?

It’s critical after making a test that you run it. Against the existing code. Without having changed the code. It’s gonna fail. You want the test to be red.

We want to avoid false positives. What happens if you run the test and it passes? Just like that! Without having implemented anything.

That’s a sign your test is wrong. The test you currently have can’t be trusted. Perhaps it‘s mocked out something that is actually affecting behaviour. Or your assumptions about how the system works are wrong — do we have a feature like this already? Or the setup your test is doing is unintentionally achieving the stated goal already. Whoops!

So if the new test passes then that’s a sign you don’t understand the system. Your assumptions are wrong. So dive into your system and understand why they were wrong. Change your assumptions. Learn more about the system — run the code in a debugger, use other existing tests to run experiments, disable parts of the code and see what happens. Then change your mission (“turns out we have a feature like this already”) or change your test with your updated assumptions.

## Generate the solution

Do whatever makes sense here for you. Perhaps you want to one-shot a solution using AI. Perhaps you want to start writing the skeleton first and then get LLMs to “draw the rest of the owl”. Perhaps for this particular piece you want to write the code by hand.

The aim is to:

- Get you making robust features faster.
- Get you to have a deeper understanding of the system, that it works how you think it works.
- Get you confident that you aren’t breaking anything.

I think there’s a spectrum from writing code by hand, to using code symbol autocomplete, to autocompleting lines with AI, to generating whole files with LLMs.

Each day move wherever you want in that spectrum. Feel like you can one-shot an entire feature today? Try it out. Feel faster writing stuff by hand today? Go for it. Speed is about measuring quality & quantity of what you ship not what you write.

## Check it passes

Ok so you should have run the test before generating, and now after. The before was your null hypothesis — no code change, nothing happens. The after is your experiment — does this code change fulfill the test?

LLMs are notoriously erratic. Maybe your experiment failed. Run a new one! You have the test, which a checklist of desired behavior you wish to observe, and also constrains what the attempted solutions should do and importantly not do.

It’s like a contract you give to a freelancer. If you open a café and hire someone to design you a logo, are you going to appreciate when they write a detailed menu? And demand that you keep it? And pay them for this unasked labor? You want the contract to state exactly what you do and do not want from this person, and for how much. Otherwise you are wasting time and money.

If you have a high failure rate, then you need to change the recipe. Make your test smaller by testing less. Find a better LLM model. Take the scenic route and write more by hand. Write a design document in Markdown that will guide the LLM.

You are manufacturing code, and so if you have a low yield then something needs to change about your process. You also want to think about how else you can measure the quality of what you are producing — how do you know your code isn’t raising unexpected errors all the time? How do you know it’s not full of security holes? How do you know it’s not causing a slow or clunky or brittle user experience? How do you know you aren’t losing or corrupting or leaking user data?

This is what software engineering is about. In my experience writing the code is the easy part. That’s why it’s critical you and other people have an understand of how the code works otherwise the asset you imagine you have is actually a liability.

## Mission accomplished

Great, you got to the other end. Not only do you have some working code and a test that proves it works, but you understand what’s been made. You have an intuition about the system you are editing. You have clarity which lets you plan and make the next feature. (Or feature removal. Principles like “less is more” still apply in this AI world. The customers are still humans.)

Consider what else you can do. You could dive into your latest implementation and fiddle with it. See what feedback you get from the tests — do the ones you expect to break from your rewiring actually breaking?

Your code is an asset and that asset isn’t just valued intrinsicly. It’s valued by how much the people working on it understand it. Otherwise there’s a danger it becomes a liability. That’s why the term “tech debt” is relevant — it’s as much a function of the maintainers _and_ the material being maintained, and if the maintainers are clueless your organization is at risk.

Vibes are about what people feel, not machines. So to keep the vibes going it’s critical you think in the long term.
