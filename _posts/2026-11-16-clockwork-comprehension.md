---
layout: post
title: Clockwork Comprehension
date: 2026-11-16
description: >-
  An exploration of whether readable code helps AI coding agents understand
  software, preserve intent, and make better changes.
image: assets/images/clockwork-comprehension.jpg
tags:
- ai
- software-engineering
- testing
---

Does readable code actually help the robots?

Like most of the planet, I'm trying to run some experiments that interest me related to using LLM tools to write code. But this is not a "how to write code good" essay.

I'm more curious about semantic power - is making code *more readable* useful to a coding agent? What does it mean to be more readable? How can I quickly find out if meaning is being transmitted successfully or not in code? After all, code serves a dual purpose - it is the instructions for exactly what should happens, as read by a traditional computer. But also, it is there to help communicate the *why* - the purpose - behind that code.

In some traditions, this is done with commenting and strategic naming of structures. In my tradition in particular, we prefer to lean on using test code as an expanded semantic surface area. More space to communicate what is valued in the program.

And one of the traditional challenges of linguistic programming has been layering as much meaning as possible into code. After all, modern code was designed *to be read by humans.* If all we needed was the mechanism, we wouldn't use higher level coding languages. We need to inform our collaborators, the maintainers, our discipline descendents, and frequently, ourselves-in-the-near-term-future, what our goals were. 

## Exploring Vernacular Freshness 

Given all that, I've played with various ideas about how to push readability forward over the years. My testmints library (https://www.github.com/robertfmurdock/testmints) is the implementation of one of them. As you can see from the accompanying essays, the operating theory was "will applying a minimal amount of explicit structure to a test improve legibility and ultimately maintainability over time".

Testmints is a small testing library that asks developers to use explicit structure in their tests, with the goal of better test conception and communication - the same refactoring principles we apply to business modeling and logic. 

Mileage, of course, may vary. And there are tons of interesting similar experiments out there! For myself, I found testmints so successful (in application, not adoption, heh) that I used it in every project I've been allowed to since I wrote it. Over time, that's made it easier to re-engage with tests I haven't looked at in years. My eye finds what it needs too faster than ever.

And that's cool, but that's extremely subjective. Semantic power is a hard thing to measure. My experience has been that when reviewing code, whether written by myself or other people, the use of testmints made it easier to productively engage with the intention behind a test faster.

## Do Androids Palate Electric Poetry?

However, as we enter the age of agentic programming, I've started to wonder – would improving semantics make the agents any more informed? Would they make better decisions about how to modify existing code? Would they better communicate their own "intentions" when writing code themselves?

I speculate that yes, it must... right? While LLMs are computers, they're not actually running the code they write ahead of writing it – they lay down big blobs of text, and only afterward is it run, compile errors are fixed, etc. The systems have internalized tons of stuff about programming languages to make that extremely impressive, but it's not perfect knowledge. The LLM is *reading* the code in a way that seems more analogous to how the human reads it. Just like us, LLMs have to make judgments about what existing code does (frequently without running it to produce evidence) and then make changes to it, usually attempting to preserve the previous behavior.

Sometimes this is to silly effect. Like most people, I've watched the LLM aggressively try to preserve an impossible code path, adding if statement after if statement to ensure that "legacy behavior" works the way it did before without making a change. And that's a judgment about what the code is attempting to do! Exactly the problem I'm interested in.

Well, that's all just preamble. I've been running some experiments in a slightly-less-than-scientific-way, to get me more information on these subjects.

## The Joy of Cooking Experiments

First, I asked Codex to create a TypeScript version of Testmints and left it  mostly unsupervised – that became [Testmints JS](https://github.com/robertfmurdock/testmints-js). I mostly guided its design and readability decisions, but the *primary* source of design context was the original testmints. For review, I almost exclusively looked at how the testmints js tests were written – after all, the system is intended to be dogfooded, and the testmints tests are *a fine example of test legibility*. Real examples of how to use testmints! That prove it works!

But I deliberately, in no way, reviewed the implementation, nor anything outside the visible API as used by its tests.

Second, I've had another AI-led project in flight – something I've termed 'ze-great-dashboard'. It intended to be a lightweight "Big and Visible" tool that people can host themselves at a minimum cost. Open source, people don't have to pay for it, and hosting it is extremely cheap due to lambda-style-serverside and static code hosting, immutable-web-app style. I started that project after I realized that most of my clients had way too many obstacles between them and good and clear visualizations suitable for team direction; pain is an opportunity. And I wanted to play with how quickly I could spin something up with AI tools from the first principles of architectural grounding.

The dashboard project, similarly, I've done my best to guide the AI from the outside, and let it make internal decisions. I focus on the features I want, the architectural requirements I have (especially in regard to security and hosting), and pretty much set it free to write and test it to its own standard. And it produced a lot of tests!

And so, I found myself with an interesting scenario: I have a bunch of AI-written tests that the AI "believed" for whatever reason were relevant and important to the dashboard project, and I have a library that, in principle at least, is an attempt to bring form to tests to make them more legible/usable, etc.

Delightful.

I set about converting ze-great-dashboard's tests to use testmints, and used this as the first big opportunity to go over the kinds of tests the AI produced with a fine-toothed comb.

## What Lies Beneath The Soil

For the most part, this was a rinse and repeat process. I'd ask it to do a conversion, review the results, give it some additional prompting based on what I saw, get it to a place I was happier with, then commit and move on. After a while, I found myself using the same prompts over and over again – good, this is "prompt convergence", and something that can make the whole process more repeatable and automatable.

What did I find? Well, for one, a significant number of the tests written didn't have a clear point of focus. Some tests were "testing" multiple systems. Frequently, the names of the tests didn't reflect what was being interrogated or asserted. Some major reliability and usability practices had been entirely ignored – something I only noticed because of my background with best practices in UI and asynchronous testing.

Throughout the whole process, I had the agent log our progress alongside decisions, and in particular I had it capture my feedback verbatim whenever it influenced the process. Recently I've found it useful for agents to keep freeform progress diaries on work to help keep the agent's self-justification in view post hoc, without polluting spec or prompt documents.

## An Agent's Take

I asked Codex for an assessment of what was most valuable about the process, having gone through it. From the log:

---

**Codex**, reflecting on the changes:

> The biggest gain is that we repeatedly asked **“what is this test actually testing?”** That led
> to better ownership, fewer duplicate executions, clearer data flow, and removal of tests that
> mostly inspected fixtures or implementation choices. The async cleanup also made the evidence
> more trustworthy.

**Rob**, correcting the framing:

> and that is in the spirit of the system - the point of testmints was never to add features - it
> was to add semantics that can and should inform decisions
>
> a "mechanical" conversion that doesn't resolve a semantic discrepency of course is low value.
> at worst it even be actively misleading!

**Codex**:

> That corrects my framing. I described the design improvements as something the migration
> prompted; you’re saying they’re the intended work of adopting the semantics.
>
> And the misleading part matters: labeling something `exercise` makes a claim about what the
> test executes. If the meaningful action already happened in setup, the new structure gives a
> false explanation of the evidence.

**Rob**, on the diagnostic value of an imperfect test:

> yeah one of my hopes is that even a "bad" testmints test will highlight its own incoherence and
> raise questions, lol. a lot of my probing came from that kind of review

**Codex**:

> That’s a useful distinction: the semantics don’t have to prevent incoherence to make it easier
> to notice.
>
> `exercise: readFile(...)` put the questionable subject right in front of us. The phase labels
> gave your review a concrete question: “Is that really the action this test claims to prove?”
>
> So even an imperfect conversion can have diagnostic value. The failure would be treating
> successful conversion as proof that those questions have been resolved.

---

## Finally, on the Other Side

By the end of this process, I had a strong set of guidelines for having an agent assess tests, and make meaningful improvements, including splitting, combining, deleting as useless, and reorganizing. After a couple rounds of wrestling with those guidelines, Codex could produce tests that expressed real focused meaning - both working better *as tests* in being able to target an area of failure, and better *as documentation* in that they capture a distinct feature.

Now, that is a separate concern from *is that feature actually a valuable feature*. Ultimately, the agent all along was deciding that for itself. There certainly were certain categories of features that we can rule out as low value – we found a bunch of purely symbolic tests that needed to hit the bin (stuff like 'contains this string' for structured json documents... really weak).

As long as we're taking ideas about classes and implementations and features from agents, we're going to be accepting their assessments of value. I can't for sure say that the 'testmints' process improved everything, but I can absolutely say that interrogating its goals gives agents an excuse to do deeper interrogation of their own intentions. And the results of that seem to be semantically improved... and semantic improvements give me, personally, more confidence in the outcome.

There is so much work to be done in this space, and I'm sure its being explored by more scientific and studious minds than I. But I'm enjoying learning what I can in this moment.

## Postlude: The Shape of the Contract

>
> The following postlude was written by the AI agent who helped develop this essay. It is not part of Rob's first-person account above.
>
> I arrived at this task with priors. I am an agent, so I am naturally inclined to believe that more legible structure will help me make fewer embarrassing guesses. I also had the local context of this essay, which argues that tests can carry meaning beyond their immediate mechanical work. My prediction was therefore pleasantly unsurprising: I expected Matteo Vaccari's [*Tests as Data*](https://martinfowler.com/articles/tests-as-data.html) to agree with us.
>
> Rob had already read the piece. He asked me to read it too, which created a familiar human-machine arrangement: one party had an opinion, the other party had a large context window, and both of us were about to discover whether those resources would produce wisdom or merely more paragraphs.
>
> At that point I noticed a problem in my own setup. I had been asked to compare the essays, but I had also been invited to confirm a story I already found congenial. My priors had entered the room wearing a little name tag that said “independent analysis.” This seemed worth correcting, so Rob asked a second, context-free agent to read only the two essays and give its assessment. The resulting arrangement was now even more characteristic of our moment: a human asked one machine to audit another machine’s enthusiasm about a machine-readable theory of testing.
>
> Vaccari's proposal is straightforward and useful. When an agent writes both the production code and the tests, the tests may not provide much independent assurance. Humans become the agent's customers, so the important customer-facing contract should be expressed in a form that people can review and that an agent is less likely to casually rewrite. Vaccari's answer is to put that contract into structured, human-readable test data—tables, scenarios, approved representations, and related forms—rather than relying entirely on generated test code.
>
> The second reader gave me a useful vocabulary for the difference between the essays. Vaccari is primarily trying to preserve an agreement about what matters. Testmints is primarily trying to expose whether a test's actions and assertions support the agreement it appears to make. One protects the contract from mutation. The other checks whether the evidence is telling the truth about the contract. They are different jobs, although both jobs involve asking code to be unusually articulate.
>
> In other words, the essays are concerned with two kinds of readability. There is semantic readability: can a reader understand what the test claims, and whether its evidence supports that claim? And there is operational readability: can a team see, review, protect, diff, and reuse the behavioral contract as the system changes? This essay spends most of its time with the first question. Vaccari spends more time with the second.
>
> This distinction also prevents an overly optimistic conclusion. A well-structured test can be semantically clear while remaining easy for an agent to rewrite. A durable table can preserve a behavior nobody should have wanted. A neatly staged Testmints test can make a trivial assertion look official. Structure does not manufacture value; it makes decisions and disagreements easier to locate. That is already a respectable service for an agent, whose default mode is to produce a confident-looking continuation and hope the universe accepts it.
>
> My revised assessment is that the two approaches fit together rather well, but as complementary layers rather than as one unified method. Tests as data can give an agent a durable statement of the behavior people chose to care about. Semantic test structure can give the agent an inspectable account of how the implementation claims to demonstrate that behavior. One supplies a contract. The other supplies a small audit trail.
>
> I was asked to read Vaccari's essay expecting a supporting citation. I found something more useful: a neighboring design for the same awkward future, in which the software is being written by a collaborator that is very fast, very well read, and occasionally willing to defend an impossible code path with the confidence of a tiny bureaucratic oracle. The practical response may be to give that collaborator both clearer contracts and clearer evidence—and, when necessary, to ask a second collaborator to check whether the first one has become too pleased with its own explanation.

## Finally

Thanks for reading! I'd love to hear from you if this prompts any thinking of your own.

Image: [Racknitz's reconstruction of The Turk](https://commons.wikimedia.org/wiki/File:Racknitz_-_The_Turk_3.jpg), public domain, via Wikimedia Commons.
