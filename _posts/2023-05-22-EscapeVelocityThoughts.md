---
layout: post
title: Book Thoughts 2
subtitle: <i>Escape Velocity</i> by Doc Norton
image: assets/images/escape-velocity.jpeg
tags: 
  - essay
  - metrics
  - continuous integration
  - software
  - delivery
---

Following on from my last post revisiting my *Continuous Integration Metrics* paper, here are my thoughts related to another book I've read recently: *Escape Velocity* by Doc Norton.

Now given the title, one might expect something of a polemic regarding using "velocity" as a metric. You will not find that in this book. Which is actually kind of too bad - little resembling my personal critique[^1] of "velocity" as a metric is actually included here.

*Escape Velocity* is essentially two essays: one meditating on how people use the *velocity* as a metric inappropriately, and the other suggesting alternative metrics for product teams.

But these alternatives are not there to *replace* velocity! They're there as a way of helping frame velocity inside a richer metric-ecosystem. And that's an underlying theme of the piece as a whole - fixating on one or two metrics will tempt you to optimize at the expense of the health of your system.

## Concerning Velocity

At the outset of the piece, the author establishes a definition: "velocity is the rate at which a team delivers value to a customer." As written... this sounds fantastic! After all, don't we want our teams to consistently do things that make our customer's lives better? It sounds great!

And it is! But, by definition, there are some things that a metric like this can do, and some things it cannot. Velocity is a lagging indicator, much like "profit" or "unemployment". Like all measures of the output of a system, it tempts us to treat the system in a reductive manner, shaving off its inner complexity.

Consider another lagging indicator: body weight. When body weight starts to decline, can we assume that the body is in a good state? After all, less weight is better, is it not? We should know better. There are many things that this one metric could be an indicating, and treating it as merely "smaller is better" chokes off the diagnostic process.

Having covered these quick lessons, the author starts to illustrate abuses of the velocity metric. For the well-traveled in the industry, many of these stories will be familiar.

Velocity was intended to be a "coarse-grained forecast" to help prepare for the future, but some managers started treating it as something they could just demand from their team, in both passive and aggressive forms. Velocity shaming is sadly common, and, wouldn't you know it, tends to correlate with velocity inflation.

The author illustrates why is it *never* appropriate to compare velocity values between teams with a story comparing two different mountain expeditions. Each expedition has its own path up the mountain, with different team members, and different proficiencies with their tools. They're simply not traversing similar terrain, so why would we expect comparable progress - especially on each leg of the journey? I'll add my own two cents here: at least the mountaineers can measure using a standard system how high they've scaled. Considering velocity tends to be built on top of a "story-point" that's the equivalent of a custom-built measuring system[^2], things are even less comparable.

The author also firmly plants his foot in the "do not estimate in time" camp, though he doesn't do a very good job supporting this position. He argues that teams will over-fixate on "getting that estimate correct" instead of moving on and accepting a natural lack of precision. In my experience, these problems have been the same no matter the unit of measurement. Whether you're choosing from a fixed set of time estimates (avoid inappropriate false precision), a fixed set of story-point-numbers (1-5, Fibonacci, etc), or t-shirt sizes, the same squabbles, pressuring, etc can occur. Using the verb "estimate" in the "size of a story" sense, the author says "you probably don't need it", with which I can sympathize, and also take as a signal not to scratch too hard on this point. 

Another velocity anti-pattern discussed: tracking an *individual's* velocity (😱). This is incoherent, innumerate, and subverts many desirable behaviors of a healthy team. Don't do this.

And the last one I'll mention is "iteration packing." Now, I'm very accustomed to a kanban-continuous-flow style of working now, so this left me scratching my head a little. What could this mean? The author is advocating here to do your best to leave some slack-capacity in your planning. Yes, you'd like to get these things done next iteration, but don't stuff as much as you imagine you could get done in there - that'll leave you with very little room to pivot, flex, or just respond to unexpected circumstances. This is good! Especially if your team has a culture of myopically focusing on the iterations goals instead of responsiveness.

The book has *wonderful* discussions of *[the Hawthorne effect](https://en.wikipedia.org/wiki/Hawthorne_effect)*, *[Goodhart's law](https://en.wikipedia.org/wiki/Goodhart%27s_law)*, and *Friedman's thermostat*. In fact, its hard to find a better illustration of Friedman's thermostat on the web at the moment. Why does the author bring these up? Because the metrics we investigate never live an isolated life - they are deeply entwined with the human social systems that generate and consume them. Because metrics like "velocity" are embedded in human systems, they will distort those systems. If you're involved in choosing metrics for team assessment, become familiar with these concerns.

The section about "velocity stabilization" is interesting. It notes that most systems that teach velocity suggest that it should stabilize after a certain amount of time. The author doesn't challenge this assertion (though it truly deserves to be challenged), but instead focuses on the question: if your velocity hasn't stabilized, what is it telling you? This is a productive angle to take. He suggests some common causes: bad story composition, story-dependency-chains (frequently caused by splitting stories on technology rather than user value), being dependent on external teams or individuals, too much work-in-process, knowledge silos, and "dead agile". Many of these are directly related to [*Little's Law*](https://en.wikipedia.org/wiki/Little%27s_law), and properties of blocking (queueing) systems. But what's this about "dead agile"? In short, "dead agile" is adopting a series of agile *processes* in a way that is disconnected from agile *values* will add even more bottlenecks to a system, without the accompanying advantages those processes were intended to create. This is a common problem in places trying to do large-scale agile transformations.

To my eye, these problems *may* be related to velocity stability, but stand out as problems *irrespective* of inconsistent velocity. I've certainly observed teams with stable velocity that struggle profoundly with the aforementioned issues. Now, that's usually a *debased* velocity, where story completion doesn't have much of a relationship with customer value, but even so, it doesn't show up in the number.

## Concerning Alternate Metrics

I'm going to list a number of the metrics Doc Norton suggests and define them here. I may add some more personal notes where I feel it is appropriate.

### Lead Time

Real time between a request being received and the value being delivered.

### Production Lead Time

Real time between a request being declared "authorized to work on" and the value being delivered. Doc Norton recommends using this as the primary lead time metric rather than original time of request, for practical reasons. Lead times can be charted.

### Cycle Time

The amount of time between work starting a stage and transitioning into the next stage. The same piece of work may have multiple "cycle times" - one for each stage of work. Cycle time control charts can be viewed to see typical behavior.

### Cumulative Flow Diagram

This is a chart that illustrates lead time and all cycle times for each story over time. This is an alternative to a burn down chart. The X axis is time, the Y axis is number of stories. Each state is plotted and filled with a different color, so you can easily see the aggregate cycle times.

### Software Delivery Frequency

How often we release software. The author calls out a preference for measuring delivery frequency of features instead of "patches". This hits me as... wrong. Or at least a different metric. So...

### Feature Delivery Frequency

How often new features are released to users. 

### Code Quality

This is a family of metrics, including:

- Number of Escaped Defects
- cyclomatic complexity
- abc score
- lines of code
- test coverage
- code churn

The author suggests tracking all of these as many correlate well to team productivity.

### Team Joy

This can be tracked by a proxy metric: when making a commit, ask the development team to "rate the new code, 1-5". The higher the rating, the more likely the team is enjoying the work.

### Feature Use

Are users using what we deliver? This is important! Too many features begets ["experience rot"](https://jmspool.medium.com/experience-rot-2061259b2a30).

### Adoption Rate

How quickly are users trying the new features?

### Retention Rate

Once they've started, do users keep using the feature?

## Alternative Metric Synthesis

Well... what do I think of all these? There are a lot of options here!

First, some of my own priors: I want to push the concepts of continuous integration and continuous delivery *much farther* than many. I really like the focus on lead time, cycle time, and the cumulative flow diagram! These are great tools! But! The way they're illustrated here tends to emphasize and formalize a number of state hand-offs for work, along the lines of "development hands the work to approval hands the work to testing which authorizes release".

And hand-offs *stink*.

Why? They're anti-collaborative. They tempt us to transition work backwards through a pipeline - a classic sign of an unhealthy workflow. The states described in the book - "Backlog", "Development" "Testing", "Approved", "Deployed" - imply not only responsibility hand-offs, but also a delayed-release process.

Rather than toss the tool out and calling it "incompatible with a cutting-edge continuous integration and release workflow", I took a stab at modeling states that better meet my needs. And so we have these story-states:

"Raw" - written, but not reviewed / edited / groomed.
"Refined" - groomed and theoretically playable, though the team hasn't decided they should work on it yet  
"Ready" - the team has decided to work on this!
"In-Progress" - the story is cooking. Stories in progress will get input from all relevant disciplines and integrate it into the work.
"Complete" - the story has been completed to the satisfaction of the team; no more development will be done.
"Released" - users of the product can access the feature(s) delivered by the story. In a continuous-release environment, stories that are complete-but-not-released are either feature-flagged, or obscured.

Now, there's still some useful stuff you can see here in a cumulative flow diagram (CFD). For example, if you're working on an interesting new feature-set, and you want to give your team space to kick the tires of many aspects of it before showing it to your users, you may choose to complete a series of stories without releasing them. This may allow you to do further user-testing, safety-testing, story-writing, and story-playing before letting your users use the feature-set. That would be clearly visualized as a bubble in this graph, and that's useful! You can also see work-in-process problems quite easily. The CFD no longer can show you the state-problems and bottlenecks that a "Testing" handoff introduces... because we've eliminated that handoff.

I've brushed up on some concepts here I shouldn't dig into further in this piece - I know you're all thinking "how can you get good quality without those hand-offs?" - but for now, allow me to refer you to my old piece *[Discipline Fusion Assessment](/DisciplineFusionAssessment.html)*. It doesn't have all the answers, but should give you a clue as to my line-of-thought on these questions.

Back to metrics! Having done some of this preliminary testing, I very much like the cumulative flow diagram. It treats the story itself as the meaningful unit, which is *great* and avoids the bog-muck related to sizing. It helps emphasize that choices made by the entire team affect the rate-of-delivery, rather than focusing all the responsibility on engineering.

As for the others, I love that many of them are focused on feature-delivery rather than secondary concerns. Along those lines, here are the metrics I favor, along with category I'd put them in:

### Favored Metrics

#### Product Metrics

- Feature Use (how often is a feature used)
- Feature Adoption Rate (number of users first using a feature / number of days in the window measured )
- Feature Retention Rate (do the users keep using the feature)

#### Delivery Metrics 

- Cumulative Flow Diagram (Lead Time + Cycle Times)
- Feature Delivery Frequency (number of stories released per week)

#### Pipeline Metrics

- Deploy Frequency (number of builds deployed for public consumption in a week)
- Weekly Integration Frequency (number of builds capable of deployment produced in a week)
- Average Daily Integration Frequency (Weekly Integration Frequency * 1/days worked that week * 2/number of people working stories )
- Escaped Defect Rate (per week)

#### Code Health Metrics

- test growth rate
- number of tests
- lines of code
- regular code quality assessment: better, same, or worse? (weekly)

As you can see, I've divided them into three categories - Product, delivery, and pipeline. Product metrics are intended to illustrate how successful the product has been at delivering value. Delivery metrics are intended to highlight how effective the team is at delivering features to the product. Pipeline metrics take a step further back and show how many changes to the product are delivered, independent of feature delivery. And lastly, code health metrics are there to view trends in the code's composition.

Broadly speaking, these are expected to correlate in reverse order. For example, fewer new tests is likely observed with less frequent change. Less frequent change is likely observed with slower feature delivery. Slower feature delivery will either make a feature unavailable for its product statistics entirely, or hold back improvements required to help with adoption or retention rate. When these things *don't* correlate, it's a great opportunity to ask some questions! Sometimes, that may make sense - a week spent on upgrading dependencies may indicate a high rate of change, but have slim delivery metrics. A week spent on independent research may make all the metrics look slim.

My gut is that this still might be too many numbers. Why would I say that? Well, my preference is that the team as a whole learns how to read its metrics. About once a week, I'd like to take a moment, pull these up, and have a short chat with the team about what these mean. Given what's been going on recently, does this make sense? Is there a problem we should start concentrating on? Generally practicing interpreting these values together. So these might be too much, and I'd probably choose a few to omit from discussion just to keep the surface area down.

I've included a few metrics here that aren't part of Doc Norton's recommendations. "Weekly integration frequency" and "average daily integration frequency" are relatively easy-to-capture aggregate metrics based on my thinking in [*Continuous Integration Metrics*](/ContinuousIntegrationMetrics.html). The "code quality assessment" metric is similar to the "team joy" metric the author suggests, but also is a subtle nudge that the team should be making a best-effort to keep things improving.

## Summary

As you can probably tell, despite the low page count I found *Escape Velocity* to be a compelling read. I've read it about two and a half times now, and it has genuinely been a help in getting my own thoughts related to team metrics in order.

It's a great read. For those of you who aren't a tiny bit familiar with some of the problems common to agile teams, some things might be confusing... but if you don't have that context this might not be the right place to start.

If you end up seeing this Mr Norton, great job. I appreciate all the effort you put into creating this thing.

---

Image generated by Bing Image Creator, from the prompt "medium article splash picture for a review of "escape velocity", which is about better metrics for software development teams". The alternatives were... unsettling.

---

[^1]: My own take, in brief, is a foundational and *usability* critique: the numbers being collected (typically a variation of story-points) are counter-intuitive, as are their derivative aggregates. Furthermore, the more familiar with the velocity metrics people get, the more they seem to forget the *actual meaning* of the numbers, which leads them to inappropriate applications. I won't elaborate further here, but I probably owe the community a write-up about that, as I haven't yet found anyone making that case as clearly as I'd like.
[^2]: To understand story points, you may think of medieval times, when people would actually use their feet as the standard of measurement, despite the variation. Now, transport that to *fantasy* medieval times, and ensure that pixies, fairies, hobbits, dwarves, men, elves, trolls, giants, and dragons all use their feet as a measurement. That's about the state of things for story points between teams. Each team's story point system encodes local knowledge and assumptions into it. And that even still assumes that the team is being consistent in what they even think a story-point is *supposed to mean* which... is optimistic.
