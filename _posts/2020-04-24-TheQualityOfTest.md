---
layout: post
title: The Quality of Test
tags: testing, TDD, test design, priorities, coding
---

So testing. What's up with that?

I've been working with Test-Driven Development and its fellow traveler concepts for essentially the whole of my career, thanks to being introduced to the concept at a scrappy startup called Menlo Innovations in Ann Arbor, Michigan. Parallel to my personal development, the industry as a whole seemingly discovered this strange "testing" thing, and underwent its own journey of change, fashion, and discovery.

So in theory, I *should* have learned a thing or two about testing along the way. I hope I did. Here are some quick observations.

Firstly, useful tests tend to be *long-lived*. Think of writing a test as an investment: yes, it will force you to better understand what you're testing *today*. But also, that test will be able to demonstrate that the test-target works (or does *not* work) as intended *for the life of that feature*.

That's neat! But it means that the test bears a tremendous burden of communication. *Even more than the test-target itself*, the test is obligated to communicate its purpose to the user.

But wait, there's more! As any software system changes over time, a test may have to be updated to work within a new paradigm. That's not to say that the *feature* is meaningfully changing, but the *structures that produce*, or the *structures that test* that feature need to be able to evolve. That means that its *even more important* that the test communicate successfully its intention; because the programmer of the future (perhaps *far* future) that will update the test needs to be able to mutate it without losing that original purpose.

So I propose these ordered priorities for writing tests:

1. A test must *actually* validate that the feature works, or that the feature does not work.
2. A test must communicate accurately to the reader the feature under test.
3. A test should be self-contained - it should minimize knowledge required to understand it.
4. A test should be a realistic example of how to use the feature being tested.
5. A test should be brief and focused.
6. A test should restrict itself to *one scenario* (properly understood, additional scenarios are additional tests).

Ok, *of course* making sure the test *actually fails* and *actually succeeds* is priority #1. That's the cost of entry to *being a test*, after all. That said, you'd be surprised at how often software developers write tests but never actually confirm *that they can fail*. That's crazy important, and you'd do well to get extremely disciplined about confirming failure.

But why is "accurate communication" (aka readability) #2? Surely being brief, or self-contained, or one-scenario is more important?

Well, I've shown my hand already. If brevity compromises accurate readability, then *you must throw the brevity out*. I've seen many tests rendered ungrokkable by aggressive adherence to the Don't Repeat Yourself (DRY) principle... and then, when the next developer stops by, those ungrokkable tests become incoherent... because a small change makes them no longer verify the intended feature. But who would know?

In testing, *readability* trumps the DRY principle. Yes, refactoring and brevity are important. But when it comes into conflict with readability, it loses. Properly used, the DRY principle *serves* readability, not the other way round. This is not an excuse to *never care about brevity*, but instead think of it this way: repetition *must* have a purpose to be justified. Reader clarity is a valid purpose.

Of course, readability is also *the most difficult to quantify* priority. I suggest using other members of your team to "test" your test for readability. Can they get the gist quickly? Where do they get hung up? The test will last a long time, so taking a few minutes to get a readability pass from someone is usually worth it.

On to the concept of a *self-contained* test! This one is more subtle for people, but it can make a big difference. The goal is to make sure that *everything relevant to the test's goals is clearly visible from the test itself*. Why do we want this? Well, even if the test *content* is accurate and readable, that does not mean that it is *easily discoverable*. And when important details about how a test works aren't immediately obvious, then the reader may naturally make assumptions that turn out to be incorrect.

This naturally *puts more pressure* on showing important stuff *in the test itself*. This is some hotly-contested real estate! So you'll need to make a lot of prioritization decisions: should I show this as-is? Can I summarize it by calling a well-named helper function? Is it safe to summarize it by making it an *implicitly-called* helper function... also known as a stealth-setup? I will never suggest that *nothing* should be done implicitly, but it has a real cost in maintainability... so *count that cost*.

Implicit knowledge isn't just restricted to the test itself, of course. The fewer things you have to 'just remember' in order to understand the purpose of the feature, the more maintainable the feature will be in the long run. As with many of these rules, pressure to write an easy-to-use test translates directly to pressure to create easy-to-use features. That is to say, a test will reflect its feature, and a feature will reflect its test. A poorly conceived feature will spread its mess to its test. An indecipherable test will result in misunderstood features.

Realistic examples! Thinking of your tests as *examples of how to use the feature* is a very powerful technique. And this is not just a trick of the mind - tests *by their very nature* are examples. Keeping this in mind, it becomes more important to avoid *hiding details of how the feature is being used*... after all, this is the *center* of the test itself. If you look at a test, and you can't immediately *see the feature under test being used directly*, something is wrong... you've smuggled the feature out of the test, somehow! And that indirection has a comprehension cost. *Of course*, all of the prior rules are more important than this one - do not sacrifice them in the name of a better example! - but when you succeed at making your tests realistic examples you're really starting to *live the automated-testing dream*[^1].

[^1]: What's the automated-testing dream, you ask? It is the idea that we can truly create *self-documenting code*. The hope that you can write automated tests *so clear* that the need for additional prose is negligible. The idea that you could generate a README file *from* your test suite, so that adding a new feature with its test will automatically show the user *how it is intended to work*.

We touched on it before, but this really is important - keeping your tests brief makes a huge difference in the long run. The less the eye has to parse to understand the intent of the test, the easier it will be to make informed decisions. Spend time refactoring to make sure your test really *illustrates concisely* what it is all about. The goal shouldn't be to simply reduce raw line count, or raw duplication, but instead to *get the message across* in fewer statements. That means that helper functions *need* to be well-named, well-parameterized, and non-disruptive. Any repetition must be *justified*, and communicate something that other phrasings cannot.

I find it helps to do a lot of "what do you think is better... A?... or B?" readability testing when trying to improve brevity. Also, keep in mind that breakthroughs in brevity tend to come later in the maturity of a test suite... after you've tested related features a few times, you see patterns that aren't obvious at the outset. It is ok to let a test be born verbose (and clear), and whittle down its verbosity over time.

Finally, restricting a test to *one scenario* helps you sidestep a number of pitfalls, principally a lack of clarity about *what is the feature being tested*. When you write a function, call it a test, and then it proceeds to use one feature three times, interspersed with assertions, you are sending muddled messages to your reader about what your priorities are.

Let a test just be a test: a simple function that proves a feature works correctly.

And I don't mean "feature" in a "back of the box, bullet point" way. By feature I mean *an intended property of the system*. For example: "when logging is disabled, no output is sent to the out stream" is a feature, as is its brother feature "when logging is enabled, log output is forwarded to the out stream". Two features, two tests. In this way, a feature only exists at the level at which it is being tested: a test *for a class* tests features *of that class*, not features of *the system as a whole*, and the language of the test should reflect that. This of course does not prohibit references to the systemic features at a low level, but it does make them lower priority... a test needs to *describe its own context first*.

So those are the principles I recommend, and try to apply myself. I will add though: none of these features *only apply to a specific kind of test*: they all should apply to the most micro-targeted unit test *just as much* as to the most externalized system integration test. The same rules and pitfalls apply *at every level of a system.*

I've tried to convert these principles to a library in the last year, and if you want to read about that, wait for [The Flavor of Mint](/2020/04/28/TheFlavorOfMint), a follow-up essay.
