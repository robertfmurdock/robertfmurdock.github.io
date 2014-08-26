---
layout: post
title: A Synthetic Creature
tags: swift testing mocks spies
---

As I've been diving into Swift again these last few weeks, the issue of [mocking](http://en.wikipedia.org/wiki/Mock_object) has crept up a few times. I've had a few thoughts over the years on the subject, and recently I've observed a lot of testing-related pushback, so... a few thoughts. For the purpose of this article, I'm going to use the term 'mock' to refer to any test-oriented substitute implementation of a programmable interface.

Mocking is an important tool in the unit testing toolkit - it is the natural solution for targetted testing of how a system uses the mocked system. And that *is* an important thing to test... for certain systems. But once understood, it is very easy to overuse... and because of the repetitive nature of most mocks, a variety of frameworks have cropped up in many languages to make it even easier. Some languages, like Javascript, barely need a framework at all (though Jasmine spies make it even easier).

Here's what overuse might sound like:
* Every model object in your codebase is mocked.
* Your tests frequently have state modeled in mocks that would be impossible with the sole production implementation.
* Setting up your mocks often obscures the actual component being tested from even quick readers.
* You change the requirements of your models, and unit tests that consume those models pass! Even though they should fail. And then the problem isn't found until the application isn't run.

First, its important to recognize that yes, these are real problems. Furthermore, quality test-driven-development does not make them inevitable. When code smells like these crop up, they're trying to tell you something about the way you've designed your system. Here are a few things that these smells tell me:

Generally speaking, the reason behavior is mocked is to avoid an unseemly side effect. Things like spammed log files, high cost calculations, or communication with a remote system. I believe that in the vast majority of cases, these should be considered part of your application's control systems. You'll save yourself a lot of grief (and mocking) if you extract functions like these from your models and group them together by purpose. Your models should provide one major function: guarenteed valid state. So simplify. Boil your models and gather the control logic that boils to the surface. Group that logic into a coherent control layer. If you do this, you'll elimate smells #1 and 2, and you'll make headway on 3.

The other smell that #3 might indicate is that your units tend to do too much - if you have to mock four or more control functions, its time to get suspicious about your unit and start to apply the [single responsibility principle](http://en.wikipedia.org/wiki/Single_responsibility_principle) more aggressively. All of software design is organization, and there are plenty of design patterns available to help with this sort of problem.

The final smell is trickier - with languages that detect interface complience at compile time, this problem will exist, but is comparitively rare as the compiler acts as the 'first test' and will nudge you in the right direction when your mocks drift from the intended implementation of a function... though of course these languages are still vulnurable to problems not made apparent in a defined interface. In languages such as Javascript however, this problem can be quite pronounced and exist for long periods of time before being detected. And problems like this, rightly, make test-drivers look foolish... why are we running all of these tests if we still miss the most basic problems!

The solution of course, includes the ones we've already covered: only mock when you want to avoid a side effect. Keep your models as simple as possible in order to maximize your ability to use their state-defending powers. And, especially in dynamic languages, harness the power of integration tests. Simple, well designed integration tests are the surest defense against the problems that arise from the mockward-drift.
