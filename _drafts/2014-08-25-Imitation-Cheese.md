---
layout: post
title: Imitation Cheese
tags: swift testing mocks spies
---

As I've been diving into Swift again these last few weeks, the issue of [mocking](http://en.wikipedia.org/wiki/Mock_object) has crept up a few times. I've had a few thoughts over the years on the subject, and recently I've observed a lot of testing-related pushback, so... a few thoughts. For the purpose of this article, I'm going to use the term 'mock' to refer to any test-oriented substitute implementation of a programmable interface.

Mocking is an important tool in the unit testing toolkit - it is the natural solution for targetted testing of how a system uses the mocked system. And that *is* an important thing to test... for certain systems. But once understood, it is very easy to overuse... and because of the repetitive nature of most mocks, a variety of frameworks have cropped up in many languages to make it even easier. Some languages, like Javascript, barely need a framework at all (though Jasmine spies make it even easier).

Here's what overuse might sound like:
- Every model object in your codebase is mocked.
- 
