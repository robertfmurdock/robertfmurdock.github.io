---
layout: post
title: A Questionable Future
tags:
  - AI
  - leadership
  - software-engineering
---

Man, every day I get post after post on LinkedIn and Medium and Substack, and *everywhere* about using AI tools. "This
Claude-Code technique revolutionized my company!" "Here's my book on AI, and here are my Agent Skills you can
download!" "These Techniques Will Solve your LLM-Code Woes!" It's a lot. Everyone is an expert, it would seem. In a
world
where everyone seems to have the pretension to know the right answer, to see where this is all going, many of these
notes
hit me falsely (despite me having real care and love for many of the people pitching these things!). I can't pretend to
know what the future holds. But I can do my best to help get us asking the most important questions.

## More, Always More

All of us software people have been diving into these tools recently – especially those of us with company-funded token
budgets. And we've been developing practices, techniques, ideas on how to get the best output out of these things. We
know these will change – the tools themselves are writhing with constant adjustments – but these tools have insistent
properties that are affecting our workflows. One that jumps out at me: these tools seem aggressively, wildly biased
toward *more content*, and struggle with re-use at every level.

This seems to be their nature: they're generative systems, and all knowledge not baked into their core dataset
has to be loaded as just-in-time context. But this bias means that it is easier than ever to *do the wrong thing*. It's
easier than ever to roll your own version of a standard tool. It's easier than ever to solve problems by overwhelming
them with code, rather than adding features while reducing total code – by strengthening a core model concept, for
example. And, perhaps ironically, the more raw code there is to traverse, the more expensive it gets to use the AI tools
themselves, both in terms of cost but also reliability. Lack of reuse, especially of library solutions, means that it
requires more raw labor than ever to find security problems: if everyone uses a few standard libraries and those get
security patches, it's one thing. But what does the world look like when everyone has their own LLM built
implementation?

So my first question for everyone: how do we balance the real value these tools provide with our need to ensure that
systems are built safely and reliably? How do we fight the temptations and pressure to just accept the easy answers the
tools give us?

## Never Touching It

I've been hearing a lot from people bragging about "never touching the code" and treating LLM agents and prompts as the
primary interface to their system. Given that, I wonder if we've lost the purpose of code: in a higher programming
language, the code, especially what we call "domain" code, or "business logic", is intended to be a plain,
human-readable presentation of what the system does. And its tests are intended to be a plain, human-readable
presentation of how it works in a series of scenarios. Not all code fits this objective – much is derivative of these
core decisions, but the *important* code is. So my question is: where are we spending our "human" context? Are we
focusing on reading the right things? And how do we maintain our ability to effectively read if people are so tempted to
surrender the need to write?

## The Flood

I've been a long advocate for continuous integration, and one of the practices that the industry tends to undervalue:
continuous review. Also known by the colloquial term: paired-programming. One of the great things about paired
programming is that every choice is built on a dialog with another human being, who can consider, refine, and challenge
assumptions. Pairing has been a useful counterpoint to what has become the industry-norm for software code reviews:
blocking asynchronous pull requests, where one or more programmers other than the original author have to review and
approve every change. In theory.

A process designed for low-trust open source development, frequently buckled under the weight of a high-throughput
high-trust collaborative team. Pull requests get too big. Reviewers feel the pressure to move forward and get lazy. The
stated goals the process was intended to achieve: safety, consensus, collaboration, education... these all
frequently failed to show up, as cultures reverted to the dreaded LGTM.

Other authors have covered the problems with this process better than I have here. But here's the rub: now with LLM
agentic coding, we're in real danger of it re-emerging aggressively, *even when we've been working with the LLMs the
whole time*.

I feel it everytime I work on anything of substance using the LLM tools.

Yadda-yadda-yadda driven-development.

It is *so easy* to put more trust into these systems than they deserve, even when we constantly see them fail. I see
them fail constantly! And still my eyes glaze over when considering the sheer amount of output these suckers want to
produce. I know I'm not the only one feeling it.

And so my last question for you all is: how do we build people, teams, and most importantly *ourselves* into software
leaders that can productively channel these pressures, rather than get swept away entirely? How do we ensure that our
core responsibilities - working, deliverable, trustworthy software - don't get lost in the flood?

## Who We Become

Have you felt any of this? Do these questions speak to you in any way? If so—and no pressure if they don't—you're not
alone. One maxim I've found perennially useful is "you are what you do." Any behavior that you build into your life,
whether intentionally or under protest, reinforces itself. You internalize it and it becomes part of you. I've always
found it to be a useful reminder of where I need to draw lines; how long I can live in an unstable state.

But it's also a reminder that, ultimately, we control who we become.

I don't know where the industry is going. But I do know that people, real live human beings, need the value that
software has built into their lives. And they need to be able to *trust* it, and will feel pain when they can't. Those
things aren't going anywhere. The world needs people who can shepherd these systems, and keep them safe.

So explore these questions, and more. Maybe we can use them to become the stewards that the future needs.
