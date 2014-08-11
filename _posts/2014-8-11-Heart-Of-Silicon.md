---
layout: post
title: Heart of Silicon
tags: ethics programming
---

Periodically, I meditate on a favorite question of mine: what does it mean to be a software developer who acts ethically? That is to say, what challenges that developers face should be considered ethical first, rather then simply mechanical or strategic. I have a few answers, but I'd love to know what the community thinks, so please add ideas in the comments below so we can all be enriched by your meditation!

One of the examples I come to the most is the problem of error handling. Knowing the kind of errors that can occur in a computer program is principally the domain of engineers and programmers - since the ancient of days, programming languages have been trying to tell us in no uncertain terms "Oh hi! This can totally fail. Be ready." The ethical challenge I see in error handling has multiple components:

- Transparency. When a programmer discovers that it is possible for an error to occur, their ethical responsibility is to communicate that information to the parties involved in designing the software. Even if those parties are disinterested in handling the error, knowing that it is a possibility can have an effect on the design.
- Logging. The absolute bare minimum error handling that a programmer should tolerate is logging that it happened: when a designer tells you to ignore the problem, *it is still the ethical responsibility of the programmer to make sure that it gets logged*. Finding try-catch blocks with no implementation in the catch is pretty solid evidence that there are some bad habits on your team.
- Design facilitation. In most cases, logging is clearly not enough error handling, and a more explicit strategy must be derived in order to handle the problem. Frequently, the designers of the major use-cases won't be well equipped to think of all the implications of each error. It is the responsibility of the programmer to ensure that they're getting all the information they need; it is not the responsibility of the designers to come up with the solution in isolation.
- Budget-sensitivity. All of these responsibilities do not mean that it is just and right for a programmer to unilaterally decide to spend an extra $xxxx for appropriate error handing. In well designed systems that thought of error handling early on, costs tend to be low... but in some cases the cost of a real error handling strategy can be high. It is the responsibility of the programmer to make sure that the team or individuals in charge of the budget have a voice in the conversation about the level of error handling appropriate to that project at that moment in time.

Frequently, error handing gets swept under the rug because its not part of the primary design - no one wants to design for failure, after all. But, as programmers, it is our responsibility to advocate for the reliability of the software. No one else has enough information to do that job well. And when we don't do it well, it won't matter how well designed the application is: poor error handling betrays the trust of your users. That's difficult to win back.

I pose the question again: what ethical responsibilities do you think programmers have?
