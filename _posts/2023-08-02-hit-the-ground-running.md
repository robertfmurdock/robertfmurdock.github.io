---
layout: post
title: Hit the Ground Running
subtitle: Impress Your New Team by Tidying Builds
image: assets/images/build-flex.jpeg
tags: 
  - build
  - gradle
  - versioning
  - semver
  - git
---

Over the last five years or so, I’ve noticed that when joining new projects, I’ve naturally adopted a pattern. There are techniques I instinctively apply over the course of a few months. Having recognized this, I figured these might be of interest to people.

So! Without further ado, let us talk about the...

# TOP FIVE ACTIONS A 10x DEVELOPER CAN TAKE WHEN JOINING A TEAM.

Hold up. Are there even five? What the hell is a 10x developer? (clickbait, duh!) If not for those meddling kids, would we have gotten away with it? We won't find out! But we might have some fun along the way.

Anyway, here we go.

![Get on with it!](https://tenor.com/view/monty-python-wizard-get-on-with-it-do-it-quick-gif-5103506.gif)

## 1: Study the Build

Every software development team these days already has some kind of "build" process. Some are better than others, assuredly, but its there.

When joining the team, to learn about how things work, you'll always want to talk to people. And of course, they're likely to hand you onboarding documentation.

And you should listen, and read it. But. But! If you *really* want to know how the system actually works, *read the build itself*.

Yep. *Read the build.*

Memory fades, documentation lags, but the build code? Oh baby, that's the stuff. Even when poorly maintained, it has the virtue of *being run*, and so has a mandatory relationship with the truth.

If you're lucky, and the team uses a build process on transient-agents, that build will have to install everything you need to run the application properly. It may even include the tooling used for deployments! All its secrets revealed. Ok hopefully not *secret* secrets, but if you notice any of those, its your opportunity to inform the team of the security risk 😅. 

The build process has to show all the dirty laundry: all must be revealed. It's ok. Once we can see it, we can make it better.

In my experience of build-studying, I've poured over Jenkinsfiles, flipped through old-school Jenkins configs, strolled along github actions jobs, investigated custom plugins, found and interrogated shells scripts, and scanned for subtle-but-mandatory installations. All of it useful! Much of it resulted in suggesting solutions to pain-points that the rest of the team had been ignoring for a while.

Each time I've done this, I've become better prepared for adding more features to the product. I also learned a lot about potential pipeline improvements. Speaking of which...

## 2: Unify the Build

Have you ever seen a software Rube Goldberg machine? You'd be surprised at how often they develop in the wild. You may have seen the signs - a build process that requires manual intervention at multiple points before being deployable, and then further intervention(s) for actual deployments. Devs who've been on a project for a long time may have gotten so used to that process that they don't see it anymore.

By actually reading the build, you may have just become the team's build expert (whoops!). Using this knowledge and permission from your team, it's time to give that build some love. For many projects, that means it's time to simplify and streamline the build process. 

So here's an easy test: write down the steps that the team has to go through in order to build the team's software. And by software, I mean, *ALL* of your team's software. Don't worry about being super accurate - the important stuff will jump to mind. How many times does a human have to do *something*? If it's more than one, then great! That's where we'll start.

The objective? Make your build, or more likely *builds* - a one trigger process. Someone hits the button, and *everything* is built. No mess, no fuss, and certainly no brain-power required.

This *could* mean merging git repositories into a mono-build: every build that has to be manually managed has psychic overhead. For some other teams, it could mean standardizing multiple projects such that its impossible to "build them in the wrong order". Almost certainly, it would start by chaining scripts together. After that, simplifying and consolidating.

I like to make each step of a build runnable locally, so it's easy to dry-run parts of the process without having to push things into a cloud build environment. Not only is it faster, but it also starts building up a suite of tools, which can come in handy for other things.

But, whatever you do, the *most* important thing is that builds are predictable, and require zero thought in order to do correctly.

I've done this sort of thing a few times.

For example, I've taken a project that had 7 independent builds in distinct git repositories, some of which were dependent on each other, and merging them all into one git mono-repo with a Gradle multi-project. Now it's *impossible* to forget that you made a change in the "common" lib, and *shudder*. It helped the team focus on features *tremendously*.

Another real-Rob-life-example: one project improved, not by making *everything* into a multi-project, but by consolidating directly dependent layers: each system that was of the form A requires B requires C becomes much easier for one team to manage if A B & C build and release together. This simplified the ecosystem from nine deeply coupled builds into three independent builds.

Why do teams end up this way? Who knows. Perhaps some combination of Conway's Law and collective dissociative identity disorder. But more likely a misapplied "best practice" related to microservices. Sometimes splitting into multiple services can accelerate your workflow! Other times, it will dramatically damage it. A lot of this boils down to *the costs of safe contract-management*, but that's a chat for another day. Most people experience it as "it takes forever to add this new customer facing feature".

Now that you've gotten your hands dirty with the build, you'll probably have noticed some other points of friction. And for many of the team's I've worked with recently, the next missing piece of the pie is...

## 3: Automate Versioning

Versions are not precious.

Say it with me now: *versions aren't precious*! One of the oldest lessons in software development has been the principle of:

- Build once
- Deploy anywhere

And us old farts learned this the hard way - even if you build the same code twice, you may not get the same result!

But a shocking number of teams never get this memo properly. Teams that have to build their source code again every time they deploy happen more often than one might expect in this modern world. And part of the reason that this happens is that they don't have their versioning process under control.

Every time a build succeeds, it should be associated with a unique and legible version number. All of its output artifacts should be tagged with this number as well. That's a hard and fast rule, and when you follow it, it solves a lot of other things (like, speeding up deployments that would otherwise require rebuilding, for one). 

But too many teams get *stuck* on thinking about version numbers, and having to write each version number manually. I sympathize. We live in a world where *products* use version numbers that have great weight attached to them. New versions of Windows. New versions of iOS.

You'll find, of course, that the *marketing* version numbers for these *massive* software deliverables aren't how anybody delivers work day-to-day. They use version numbers for libraries, apps, and other tools that are disconnected from these efforts, so engineers can live their lives and use versions for their real purpose: to distinguish builds from each other. 

Versioning is such a common problem that you'd think that by now, there would be more of a cookbook for handling versions successfully. And, while there are plenty of cookbooks, there are reasons that none of them have won - *Rob waves his hands toward a field of slowly-integrating software developers*.

But enough meditating on the problems. How do we fix it?

Well one thing the industry has broadly adopted well is "semver" - [semantic versioning](https://semver.org/). If you're not already, get familiar. It is good.

Are you a library developer? Then *automate* your semantic versioning. That means, your engineer team will need to consider their API impact on every commit, and label it as such. But how could this work on a project where you can't predict which order commits will be merged in? If you have three streams of work going, how could they resolve this?

Well, you *could* have a social agreement about who gets which specific version number, and then each commits that into their change. But this is brittle and fussy. Luckily, alternatives exist!

Get the team to start annotating their commits with the nature of the change: major, minor, or patch. Add some build logic that will calculate the new version number based on new commits, and voilà! All set. For extra credit you can even add git protections to guarantee that your team always includes those notes.

If you're *not* a library developer, then automatically generating the next version number is quite straightforward - just +1. Every build gets a new one. Move on.

These aren't complicated things to automate-from-scratch, but as a passionate advocate, I've thrown together a Gradle plugin to make this easier: [Tagger](https://github.com/robertfmurdock/ze-great-tools). Better instructions for use pending, but it'll let you generate version numbers, tag the associated commit, and even publish a Github Release.

That said, the *hard* part of setting up a simple automatic versioning process is political and not technical. You may have to wrestle in meetings to pull control of the versioning process back into your engineering team. You may need to help your project management group stop using the build version as a name for feature milestones.

And remember! You're allowed to have *other* kinds of version numbers for things other than builds! File formats can be versioned (and your code can support as many or as few of them as you like). Service APIs can be versioned - typically by using a new endpoint path. Clear distinctions make this all much easier to manage.

### 4: Streamline Deploy

In order to deploy *any* version of your system, you should be able to select that version, and then hit the button.

Then go on with your life.

Seems simple, right? Why would anyone ever do anything else?

It's a good question, but people certainly *do*. So simplify and aggregate all of whatever-is-happening down to one step. This way you can confidently deploy stuff, both new and old (in case rollback is needed).

For me, I've run into multiple situations where organizing build artifacts by the version number was required. If you're using docker containers exclusively to deploy, this is cake: just tag the image, push it, trigger respawn, sip delicious tea. But, if you're dealing with deploys that aren't quite so easy, it might involve some file organization - uploading and downloading all required artifacts by version number before triggering deploy scripts.

I mentioned pulling multiple projects into one before: in the deploy space, this meant allowing the "deploy job" to handle multiple simultaneous deploys. For me, the simplest thing was to allow the Gradle process to handle that. Others may require some more build-agent splitting (which is more cumbersome, but can overcome build agents with low CPU core allocations).

The rule of thumb I try to live by is "ease of administration". If we make 300 changes to seven systems during a week, it's a waste of my time to try to figure out which thing needs to be deployed when. Deploy them all, and if optimization needs to be done, automate that optimization. Blessed be the Gradle cache.

The more continuous your integration and deployment, the less you'll need to make these distinctions anyway.

### 5: Accelerate!

All of the stuff listed so far might make your build slower. That's to be expected - "ease of administration" was the top priority. But after you have the correct form factor for delivering your software, now you can ~~make it more better-er~~ improve it.

What could this include? Well my favorites include:

- Caching! If a component didn't change, don't rebuild it! Gradle has fantastic tools for this, and I'm sure other build systems have good options.
- Parallelism! Some aspects of builds parallelize very well. Gradle also has excellent tools for this.
- Reduce heavy hand-offs! Sometimes setting up a cloud-agent can be time-consuming, so try to consolidate work that involves time-consuming setup onto the same agent.
- Pre-build! Finding components that can be pre-built can save some time. Docker base-images are particularly good options for this strategy, as the top layers of the image can be the most time-consuming to build, but are unlikely to need to change at great speed.

But technical improvements can't be all. The *best* outcome is also to improve your team's culture. So start watching:

 - number of versions created per week
 - number of releases to the highest environment
 - when things are slow, why is that?

Once your team has a shared objective of making your system *better* and easier to change, they're *empowered*. Have conversations about how you could improve things that are painful. This, of course, will never be exclusive to the build process: sometimes the *hot spots* you find during build & release may lead you to substantial core architectural improvements. In my world, this has usually meant re-thinking the modularization, adjusting service boundaries, and simplifying the testing structure.

But all of those bigger changes feel *impossible* when the build and release processes are split.

### Summary

Applying these strategies will do a few important things: you'll get very familiar with how the software works, and you'll have eliminated "drudge-work" that is slowing down the teams regular operations. Once you get these fussy pipeline problems solved, next you can focus on the next frontier:

⚡️Taking advantage of faster builds, with faster integration.⚡️


---

Thanks to [Ryan Taylor](https://www.linkedin.com/in/ryantaylor3/) for helping out some of the thinking on this piece. He's excellent.

---

Image generated by Bing Image Creator, from the prompt 'Splash image for a medium article about impressing your team by learning how their build works better than they know it'.

---
