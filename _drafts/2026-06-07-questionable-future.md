---
layout: post
title: A Questionable Future
tags: 
  - AI
  - leadership
  - software-engineering
---

## Outline

**1. Opening** (100-150 words)
- The answer-selling problem
- Why questions instead of answers
- Set up self-examination for leaders

**2. Question 1: Systems level** (150-200 words)
- LLMs biased toward generation vs. composition
- Impact on modularity/upgradability  
- Security patching nightmare with custom implementations

**3. Question 2: Individual level** (150-200 words)
- "Never touching code" trend
- Code as human-readable documentation of intent
- Where are we spending human context?
- Reading/writing loop and maintaining comprehension

**4. Question 3: Team/culture level** (150-200 words)
- Pressure from multiple sources (dopamine, professional, tools)
- "Yadda yadda yadda-driven development"
- Code review culture under siege
- How do we resist this?

**5. Close: Call to humility** (100-150 words)
- Resist premature answers
- The discipline to stay with questions
- Posture for leaders in uncertainty

**Target: 600-1000 words**

---

## Draft

### Opening

Man, everyday I get post after post on LinkedIn and Medium and Substack, and *everywhere* about using AI tools. "This Claude-Code technique revolutionized my company!" "Here's my book on AI, and here are my Agent Skills you can download!" "These Techniques Will Solve your LLM-Code Woes!". Its a lot. Everyone is an expert it would seem. In a world where everyone seems to have the pretention to know the right answer, to see where its all going, mahy of these notes hit me falsely (despite me having real care and love for many of the people pitching these things!). I can't pretend to know what the future holds. But I can do my best to help get us asking the most important questions.

[COMMENTARY: Good opening energy. Consider: "hit me falsely" → "ring false" or "strike me as false". Also "mahy" → "many". The thesis pivot at the end works but could land harder - you're positioning questions as the more honest move against the certainty-industrial-complex.]

### Question 1: Systems Level

All of us software people have been diving into these tools recently - especially those of us with company-funded token budgets. And we've been developing practices, techniques, ideas on how to get the best output out of these things. We know these will change - the tools themselves are writhing with constant adjustments - but there are some things properties that seem very insistent about how these tools are affecting our workflows. One that jumps out at me: these tools seem aggressively, wildly biased toward *more content*, and struggle with re-use at every level.

This seems to be their nature: they're generative systems, and all knowledge that isn't baked into their core dataset has to be loaded as just-in-time context. But this bias means that it is easier then ever to *do the wrong thing* (in some cases). Its easier then ever to roll your own version of a standard tool. Its easier then ever to solve problems by overwhelming them with code, rather than adding features while reduce total code - by strengthening a core model concept, for example. And, perhaps ironically, the more raw code there is to traverse, the more expensive it gets to use the AI tools themselves, both in terms of cost but also reliability. Lack of reuse, especially of library solutions, means that it requires more raw labor then ever to find security problems: if everyone uses a few standard libraries and those get security patches, its one thing. But what does the world look like when everyone has their own LLM built implementation?

So my first question for everyone: how do we balance the real value these tools provide, with our need to ensure that systems are built safely and reliably? How do we fight the temptations and pressure to just accept the easy answers the tools give us?

[COMMENTARY: Strong section. The "writhing with constant adjustments" line is vivid. "Some things properties" needs cleanup → "some properties". "then ever" appears 4x - consider varying: "than ever" (correct spelling), or replace some with "easier now" / "simpler now". "LLM built implementation" → "LLM-built implementation" for clarity. The escalation from convenience → cost → security is well-structured. The closing double-question lands the provocation without prescribing. Length: ~245 words, slightly over target but not egregiously - you could tighten if needed, but the ideas breathe well here.]

### Question 2: Individual Level

I've been hearing a lot from people bragging about "never touching the code" and treating LLM agents and prompts as the primary interface to their system. Given that, I wonder if we've lost the purpose of code: in a higher programming language, the code, especially what we call "domain" code, or "business logic", is intended to be a plain, human-readable presentation of what the system does. And its tests are intended to be a plain, human-readable presentation of how it works in a series of scenarios. Not all code fits this objective - much is derivitive of these core decisions, but the *important* code is. So my question is: where are we spending our "human" context? Are we focusing on reading the right things? And how do we maintain our ability to effectively read if people are so tempted to surrender the need to write?

[COMMENTARY: This section hits the core tension cleanly. "derivitive" → "derivative". The progression from observation (never touching code) → purpose reminder (code as human-readable documentation) → the real question (where's our attention going?) works well. The final question about the reading/writing loop is sharp - you're not moralizing about AI use, you're pointing at a genuine skill atrophy risk. Length: ~160 words, perfectly in range. The contrast with Q1 (systems/security) and setup for Q3 (team culture) creates a nice arc from macro → individual → social.]