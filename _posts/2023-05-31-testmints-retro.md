---
layout: post
title: Testmints Retrospective (2022)
image: assets/images/testmints-retro.jpeg
tags: 
  - testmints
  - retro
  - metrics
---

Jan 16, 2019: "Created new sugar functions to help describe different components of a test in kotlin." Coupling, [Git Hash: 0da861e6](https://github.com/robertfmurdock/Coupling/commit/0da861e6963bbe45c2c183b049d16628640b702b)

Back in the before times of 2019, I wrote a library called "testmints". It was born of a single thought: can I improve the readability of basic tests? And now, looking back, I think it succeeded (to some degree)!

The original "testmints" was a 13 line file, called TestStyle.kt. It included a comment indicating the language was based on "http://xunitpatterns.com/Four%20Phase%20Test.html" - a hyperlink that still works!

The library was extracted from Coupling on Feb 22, 2019 and it had already grown. Testmints was born of a migration: I was translating Coupling's Javascript test code into Kotlin. Even then, it had a few properties we'd recognize now: the standard module, the async module, and a file full of multiplatform assertions. The tests? Few existed - after all, these functions had slowly been refactored out of existing test code as helpers.

And shockingly, looking back... the semantics from a *usage* perspective haven't really changed much!

It's been over four years since then. I can personally confirm that over 2000 testmints tests are currently live. There are no analytics built in, so I can't tell if that's all the usage, or if others have picked it up without my knowing. Nexus Repository Manager claims that the com.zegreatrob.testmints group has seen "1,395,432" downloads in the last year. I have no idea if that's terrible or good or completely misleading. I assume usage is still not widespread, as I would have assumed I'd have heard more chatter about the project.

So it's time to sit down and consider how the library has gone! What's been successful? What hasn't worked? What confounds people?

Let's go.

First, lets enumerate some of the core features that the *testmints* suite provides.

- low-impact organizational test structure
- robust support for kotlin coroutines
- a reporter system that allows hooking into the testmint lifecycle
- test-templates, that allow inclusion of shared setup and teardown functionality
- multiple exception capture, for situations where both the exercise and the teardown produce errors.
- a gradle plugin for automatically enabling lifecycle logging on two platforms (jvm and js)
- compatible with commonly used testing libraries, including junit, mocha, jasmine, kotest, mockito, and more.
- simple assertion library
- simple spy function library
- runs on all kotlin multiplatform platforms

Make sense? Alright, I compiled some numbers from the projects I have access to, and utilization comes out in this order:

- minassert
- async testmints
- standard testmints
- (big gap in usage)
- minspy
- test templates
- reporting / testmint lifecycle logging

Now it's time for some informed guesswork about why things came out this way:

That tiny assertion library actually edges out everything! I... did not expect that. To be fair though, usage of the assertion library is about 1:1 with use of a testmint test. This could make sense - while most tests will be complete with one assertion, a substantial minority will have a couple. In fact, that the ratio is almost 1:1 is suggestive that the testmints suite is successfully nudging people toward keeping their tests simple, asserting on a single output.

There's a substantial gulf after standard testmints and "minspy". The spy module is one I probably spent the least time refining: it's oriented toward building a function with collection information, and nothing more. It works well for what it does, but now that its had some time to cook it could probably use some love to help with rough edges in use. 

The rest - test templates, test reporting, and the test lifecycle logging plugin, may as well be zero. Almost entirely unused.

For test templates, this is partly by design - testmints is nudging you to keep setup local, so you can see it easily and better understand your test. Using a test template to inherit setup makes it invisible, and so as a code designer I'd want people to reach for that tool more sparingly. If so, why write the feature at all? Well, it was originally written to solve a specific problem related to end-to-end testing: requiring shared allocation and de-allocation of resources that the test depends on, but are not central to the semantics of the test. Examples include setting up a drivable web-browser, generating a jwt token, and ensuring a file is pre-generated.

Test reporting and lifecycle logging are both features that are completely unadvertised. The lifecycle logging plugin only was created a month ago and is still in proof-of-concept stage, so it's appropriate for its use to be limited. That plugin is intended to be the "easy" in to understanding the power of the lifecycle hooks. This pairs well with things like structured logging, which can unlock even more power.

## Good Behaviors Observed

Something about the design of testmints puts pressure on keeping things simple. When things grow, usually the growth is in the setup becoming more and more complicated. Some devs responded to this by introducing more variations on builder functions: if multiple tests need an object with similar configuration, just make builder function for the shared content and use it. If you're using kotlin data objects, you can then tailor the object for the specific test scenario using the "copy" function, and you're off to the races.

Another thing I consider a positive is, even for relatively un-groomed large-setup tests, everything relevant to the test was in eyeshot (possibly with a little scrolling). Large setups like that suggest that the function-under-test has too many collaborators, and the framework helps illustrate that. In my estimation, it does it without making it punitive (even the unwieldy are still relatively easy to read), but your team's mileage may vary.

Philosophically, pushing as many assertions as possible into a simple "assertEquals" check also has had some good rewards. This puts more pressure on passing around simple data-models that capture all the information you need to know at a given stage. Again, kotlin data classes are ideal for this, as they come with great natural string output. The 'minassert' library provides a "result.assertIsEqualTo(expectation)" function to make the verify sections flow nicely.

## Curious Behaviors Observed

I've seen some engineers get in the habit of putting an "expected" data structure in the setup, even if it's only used in the verify section. To my eye, this can be a bit misleading: when I see data in the "setup", I assume it's going to be consumed in the "exercise". When that data is not consumed, I have to make a mental correction. I assume this comes from a pseudo-old-school approach: the idea of doing all allocations at the beginning, then operating on that provided data. I don't think this is exactly *wrong*, but myself I'd guide people to only allocating data in the section that needs that data. That way the setup maps more 1:1 with the concept of "input".

## Bad Behaviors / Framework Failures

A few things came up that could definitely use work. First, for large object comparisons, the output of the "assertIsEqualTo" function is challenging to scan, especially in an IDE. Occasionally the IntelliJ compare viewer would work correctly, but it wasn't consistent, and we never did research to figure out why it would work in some situations and not others.

The template system has an advantage that is also a disadvantage: if you're using a custom template, you can clearly see in the first line of the test which template you're using. What could be wrong with this? Well... if your template is designed to *always* be used, it's very easy to accidentally forget to include it and fall back to the normal testmints system. And then you might find out *the hard way* later that your system wasn't actually tearing down everything like you had hoped.

Seems like a solvable problem? Not solved at the moment.

## New Ideas

Thinking about *testmints* the last few days, it's occurred to me that I should declare an aspirational goal that the library is merely a (current) means to an end.

That goal? Current working name: *Structured Testing*.[^1]

The idea of *structured testing* is similar to the concept of [*structured logging*](https://duckduckgo.com/?q=structured+logging). If structured logging is producing log data in an indexable format (JSON), structured testing is producing *test* data in a structured format. But Rob, I hear you say, don't most test results already produce structured output? Isn't that what the xunit XML is?

Well, yes and no. I envision *structured testing* as being about a *live stream of marked, structured test data*. Every event that happens during a test should produce a JSON message, sent to a stream, about what that event was.

With *testmints*, you can see how this could be enriched. You'd see a message stream like:

- { "test": "Rob's Test", "state": "start", timestamp: "now"}
- { "test": "Rob's Test", "step": "setup", "state": "start", timestamp: "now"}
- { "test": "Rob's Test", "step": "setup", "state": "end", timestamp: "now"}
- { "test": "Rob's Test", "step": "exercise", "state": "start", timestamp: "now"}
- { "test": "Rob's Test", "step": "output", "message": "Remove this printline before you commit, Rob", timestamp: "now"}
- { "test": "Rob's Test", "step": "exercise", "state": "end", timestamp: "now"}

You get the idea. To do this *right*, you'd need to tap into the test-runner's lifecycle hooks, while allowing test code to push messages itself - like the testmints lifecycle messages for "setup", "teardown", etc. Or maybe internal stuff would log to a "jsonMessage" node, inside of a wrapper JSON. A clever assertion framework could also provide richer information. 

I don't know: this is still cooking. It feels like one of those ideas that *should* be solved already, but while there are a lot of custom solutions (like how IntelliJ receives test events from runners it can hook into), I haven't seen a standard. But my internet-search-fu isn't always the greatest - let me know if you've seen anything interesting along these lines.

## Summary

I am extremely grateful for everyone who's played with the library and given feedback on it. I'm especially grateful for people who've integrated it into their projects and are getting value from it. It started as a lark and a way of exercising Kotlin, and has evolved into something precious.

I don't know what's next, or whether the library will die and be replaced by something better, but I think its been worth it so far.

Thanks all!

[^1]: This is different from McCabe Software's 1996 proposal for Structured Testing, which is about finding and proving that code paths are sufficiently covered. Given that version of the term is not in common use, I don't expect a lot of pushback at the moment. But if you're curious, [their white-paper is here](http://mccabe.com/pdf/mccabe-nist235r.pdf), and it is interesting.