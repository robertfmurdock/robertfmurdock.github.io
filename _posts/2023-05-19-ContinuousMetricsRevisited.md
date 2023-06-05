---
layout: post
title: Looking Back, Part 2
subtitle: Continuous Integration Metrics
image: assets/images/metrics-revisited.jpeg
tags: 
  - essay
  - metrics
  - continuous integration
  - software
  - delivery
---

In the foggy past of 2017, I wrote a short paper called "Metrics for Continuous Integration". It consists of a semiformal model of how software teams produce work, and how to use that model to measure things that have meaning.

That paper is available [here.](https://github.com/robertfmurdock/TeamCoaching/blob/master/ContinuousIntegrationMetrics.md)

So let's read it again and see how it tracks with my current thinking!

This is a paper aggressively targeting integration, with very little ceremony. It intentionally did not attempt to make the case that continuous integration is *good*. And flipping through my back-catalog of published pieces now, it doesn't look like I've even tried! This is a big gap. I won't address it right now, but I will recognize it.

That said, between you and me right now, let us stipulate that desiring  code integration to be more continuous is *good* and continue.

## Terms

I like the term section. I find myself using the term *Greening* in conversation, because its just very useful to have a term that means "all required validations". I do have *some* regret about using the term *Commit* here... it is so very similar to how the term is used casually by software developers, but *juuuust sliiiightly* different - the difference being that a *Commit* is *Green*. The trick is, it's pretty hard to come up with an alternative term that feels right! Yes, I think it would be a better world if every git-commit was *Green* (meeting all the organizational requirements for integration), but sadly, this is not the world we live in.

## Universal Software Development Workflow

The "Universal Software Development Workflow" section is shockingly tight and strong. No regrets here.

## Metrics

On this recap, I immediately noticed that the "Commit Time":

    This is the Real Time that passes between the initial code change and the creation of a Commit.

is trying to be a little cute and exclude the thinking time before initial coding. It jumped out at me because thinking time is clearly noted in the model. I can understand why - thinking and consideration about work happens all over the place, and can be particularly squirrely to measure. But by the same token, why is an initial code change suddenly the trip-wire? If I revert the initial change, should the clock restart? Probably not.

I'll think about it a little more, but I think I'd like to revise this to be closer to "starting a task that results in a Commit" rather than tie it so closely to code. It makes sense to me that it would include immediate planning time, so that you could start measuring when a story-card is picked up, and restart the clock each time a *commit* is *merged* related to the story (or when people stop work for the day).

That also highlights one of the challenges in trying to automate the collection of these metrics -> how to accurately deal with time-not-working.

The rest of these metrics are pretty solid. I particularly like the highlighting of the "Absolute Minimums" and "Practical Minimums". These are great for illustrating the current biggest challenges in the pipeline.

## Targets

The Targets section is where the paper begins a subtle transition from dry analysis to gentle, opinionated nudging. The use of Emoji to illustrate "good" values from "not-good" is a subtle hint. 

For anyone reading the paper that was not willing to grant that "more continuous integration is good", they're going to chip a tooth on this section. *How dare* you suggest that a longer integration time decays value! Of course, this is also the section that helps lay-bare just how much overhead slow-integration produces, and these numbers help people understand why systems that have poor integration rates tend to decline instead of improve.

My experience from the years following this section have reinforced my take here. The projects that have been the most consistently productive all have *commit* times under 4 hours, and integration times under 20 minutes (the recommended under 2 hours is *extremely generous*). You can watch incoming changes throughout the day and see everyone's progress, all merged together successfully, steadily streaming in.

## Measurement

Sadly the measurement section doesn't have many great answers or suggestions. I'd like to develop this further in the future - along those lines I recently read Doc Norton's "Escape Velocity" and I'm in the process of folding some of his thoughts into my own perspective. 

While the paper has no great answers for easily measuring the metrics of the model provided, I have had success using a different strategy: measure how many successful *merges* occur over the course of a week, and use that as a proxy for integration rate. You can estimate some of the smaller numbers by doing some basic math. For example, if people are working for 40 hours in a week, and we're running 3 pairs, then we can see each pair averaged a 2.5 hour *merge* time.

Keep that as a running trend, and you have a useful data point for framing conversations related to your software delivery pipeline. The number went down - does that make sense for the work we were doing? Did something else slow the process? Useful stuff.

## Understanding and Improving

And this last section fully transitions into team coaching and recommendations. I can imagine even building a metric framework that makes suggestions like these when numbers fall into the related category.

Most of these recommendations stand up very well. In particular, the "Improving CT.2: Green Real Time" section is something that many teams would benefit from internalizing and practicing. Getting good at that stuff makes large monorepos not just possible, but a joy to work with.

I'd also like to highlight "Improving IT.4: Blocking Formal Code Review (BFCR)". This is where I coined the term "Non-blocking Formal Code Review", which... has not caught on. But I love it! There are a lot of useful ways to do non-blocking code reviews, and I hope people experiment with different form factors.

Implicit in a non-blocking formal code review process of course, is that your team has enough safety practices in place to ensure that rapid integration can happen safely. In places where production risk is low (nobody loses substantively if the service is down for an hour), its possible you don't need anything. Most paid service products would be better served by using pairing to reinforce a culture of testing and making-safe-choices. If you have compliance requirements, ensuring that pair-review is accurately tracked in a system-of-record will be important. Non-blocking review is intended to take the pressure to get *every* editorial comment out in the blocking-process, and more the more aesthetic and long-term conversations into a format that doesn't slow down value delivery.

## Wrap-up

I'm actually relieved that this paper still reflects my perspective well - there was real risk that my thinking had drifted! There's definitely more work to be done in this area, in particular improving the ease-of-use for these concepts, especially regarding automation. When I put together a write-up of "Escape Velocity: Better Metrics for Agile Teams", I'll attempt more synthesis. More to come! Thanks for reading.

---

Image generated by Bing Image Creator, from the prompt 'Medium header image for "Continuous Integration Metrics, Revisited", which is an essay about measuring teamwork effectiveness'.

---
