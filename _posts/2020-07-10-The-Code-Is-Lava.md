---
layout: post
title: The Code Is Lava
subtitle: Concerning Solutions Bigger Than One Team  
tags: review
---

It feels like my job in life has become akin to the Scooby-Doo gang. I keep finding problems wearing technical masks, and yanking them off, revealing "the real problem was human-nature the whole time!" So bear with me - things are about to get a little dense, a little galaxy-brain, and probably has some banalities sprinkled throughout. But hopefully when you get through this text-miasma, you will have a clearer understanding of what the real villain is, and maybe a thought or two about how to manage it. Deep breath... let's go.

Today, the masked problem is: "How should I organize large-scale architecture?" In the wild, this frequently degrades into "how do I get valuable output from these 200 developers?"

Now, the astute among you may have noticed that this is actually a problem created by assuming a solution to a *different* problem - both architecture specifications and mass-development are *components of solutions*. Even so, this is frequently the version of the problem that people encounter - "we have all of this build-capacity... how do we make use of it?"

The "how do we make use of it" question comes with baggage. Often there's a history of wasted effort surrounding enterprise software at scale. Organizations dealing with these unwieldy masses look at software *other* companies are producing with tremendous jealousy and FOMO.

"They seem to be able to do so much more with so little... why are we having so many problems?" Yes, that is not a rational response. Yes, the reaction isn't compensating for survivor bias (the software you *see* in the market doesn't illustrate how many competitors died, or how expensive it was to get there). Despite all this, it is a very *natural* reaction. We all want to be successful, after all.

Perhaps related to this response, many organizations have fallen back on "system architecture superstructure" design in order to *solve* their organization problem. This tends to come in this form:

- Take architectural ideas from other, successful companies, and create a massive architectural spec based on those ideas.
- For each system in the spec, assign a team to manage it.
- Revise massive spec periodically, merging or splitting modules and teams to match the new systemic goal.

Building large systems is *hard*. But the difficulty usually doesn't come from the *systemic* parts of the design. No, the difficulty comes from the mismatch between what the system *does* and what it *should do*, and trying to bridge that gap.

Which brings us to the human problem underneath the technical problem - how to organize work to make the solution the best it could be? How can we maximize the chances that the solution will meet the need, even as the customer, the market and the technology evolve? Specifically, how do we organize the workforce for this?

"When all you have is a hammer, everything looks like a nail" moves from cliché to *prophecy* when you have 200 careers on the line. The sheer weight of that staff makes it very difficult to disconnect from the solution in order to see the problem clearly. This produces a bias toward solving the *staffing* problem, but at the expense of considering the *product* problem. Unfortunately, this can lead to making your product problem much more gnarly.

Problem solving, especially the ongoing problem solving that software demands, isn't something you can solve in a short design phase, followed by an ongoing long-tail implementation phase. Everything is in motion - what your users need, what the market expects, and what is technically possible... these surfaces are constantly shifting, and with it shifts the cost of change.

Now imagine two distinct teams co-ordinating changes. Now imagine an interface they have to come to a consensus on. Widen it. Wait for things to settle - for both teams to build something that works. Now introduce a change that affects *every part* of that interface.

The noise you just heard was the sound of 100 readers groaning at the prospect of that change. It may seem like a small thing, but, generally, change can be rapid *within* a team's domain. At the border between teams, however, the cost of change *skyrockets*. Teams create seams - the place where two contexts touch.

It's important to note that this is a property of *teams* and not *systems* or *services* per se. System is an astonishingly flexible word. When a single team is managing multiple services, making a change that only affects how their services talk to each other is *easy*, and the changes can be *bolder*... the team's internal change management process can handle the whole thing. And the more fluid the team, the more change they can handle.

Change within a team's borders can *flow like hot lava*. It can move, adapt, be rerouted, and be molded. As soon as that lava comes up against a hard barrier, it starts to pool. Change slows. It cools, thickens, and the fluid layer becomes thinner and thinner, until it flows no more.

Thus every team creates its own "zone of maximum fluidity". Again, dividing work between teams creates rigid seams. But let us remember: a healthy product needs to *iterate aggressively towards true, ongoing solutions to problems*. Where you introduce a seam, you are selecting for *rigidity*. Which parts of your system will be rapidly iterable? Which will not? There is tension here.

This is not an argument that *there should never be seams* or that *teams should stretch to be massive*. Rather, it is a recognition that *laying down a seam* creates profound barriers to change, and so should be done strategically, and with as much information as possible.

To capture this kind of thinking for myself, I've started to emphasize *full-stack values*. What are full-stack values?

- Making a solution more useful is *the first priority*, on day 1, day 100, and day 1000. For everyone on the solution team, no matter the role.

That's it. Why full stack? Well, keeping this fluidity front-and-center means all of your disciplines need to be on that solution team. Also, all the architectural layers of your solution need to be maintained by the team. No, not everyone has to be able to do *everything*. But they *do* need to be able to aggressively collaborate in order to keep the valuable changes *flowing*.

Wait! That's too much work for one team! Well, it *could* be, but that depends *how you draw seams*.

Luckily, modern software development has some ready-made examples of well drawn seams. Consider authentication - there are many software services that *provide* authentication now - Google, Facebook, Amazon, Apple, and of course, Auth0, all will provide this service to your application.

Now, *none* of these integrations require knowledge of *how your solution works* in order to provide you that value. They are agnostic - they each provide you with knowledge of an authenticated user, and it is up to your solution team to decide how that maps to the world they provide. Your solution discretion is defended! And yet... these integrations are *profoundly* part of your solution. They even have their own user interface, that they can update *without your permission!*

This may be a bit frightening, but it is profoundly *good*. Not only is your solution unburdened from the messy authentication problem, but the *authentication* solution can update user-facing parts of their product without waiting for *your team* to update.

Another lesson can be found here: the seam where the authentication integration happens is remarkably *thin* and *rigid*. Because the surface area of the authentication product is limited and simple, it allows them to make profound changes to the systems *behind* that interface without getting their customers involved.

Therefore, as much as possible, when offloading work from your solution team, make your seams as narrow and *product-agnostic* as possible. If you're building a video game, and your concept of "hero" has to change, *only the main game team* should have to make changes - all of the 'core solution concepts' should be *fully encapsulated* in that core solution, and *not leak* into other systems. An 'authenticated user' may *relate* to a 'player', but they are not the same concept. A 'hero costume' may *relate* to a 'digital product', but the digital purchase system shouldn't *care* whether you're selling 'hero costume' or 'win-buttons'... they're all just products.

In short, things that may begin life as part of your primary solution may become *solutions in their own right*, and once that concept is clear, that is the time to draw the seam and spin off a new team. Teams rally around clear visions, after all. If a team lacks a core solution-concept, or only has a vaguely defined vision, you are quite likely to find a seam where development, change, and success (if it ever comes) is slow.

Finding the right seams isn't easy. It may take multiple attempts. When you see how teams interact, watch for hot-spots - places where making product changes ripple through many teams, and thus getting the feature to the product *drags*. The hot-spots won't reveal the correct answer themselves, but they'll give you a clear problem to solve, and that's the first step.

*Make no mistake*, system architecture is relevant, important, and can dramatically affect the long-term health of your system. But, even with all that, it is ultimately *in service of the product*. Making the product the best it can be is a work of *human collaboration*.

And so, to make the best product possible, center everything on a unified, collaborative product team. A place where code flows like lava, adapting to a changing landscape at maximum speed. And a unified array of product teams, all flowing fast to serve the customer, is an astonishing, inspiring force. 
