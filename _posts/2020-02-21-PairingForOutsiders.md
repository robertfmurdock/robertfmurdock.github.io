---
layout: post
title: Pairing for Outsiders
tags: pair programming, programming, pair, collaborate
---

I've been working as a software developer using "pair programming" for most of my career. Over the years, I've discovered that it can be quite difficult to explain, well, what on earth "pair programming" actually means. People who know programming but not *pairing* have one level of understandable confusion. People who *don't* know programming can have a particularly difficult time understanding what I'm talking about, how something like that could work, or even get a picture of what it is.

This essay is for the latter. This is an attempt at describing what "pair programming" is for the outsider audience. Hopefully it works out.

When you encounter a team that uses pair programming to complete work, you will notice a few things:

- Rather than having one person at a workstation, there are two, likely sitting side by side and looking at a shared screen.
- These two appear to be constantly communicating: chatting, pointing, singing, or scribbling furiously on nearby scraps of paper.
- Each of these pairs is sitting relatively close to the other pairs. Occasionally small missiles(maybe a nerf dart, a stuffed animal, a paper crane) fly from one pair to another, causing short term disruptions.

What's happening here? Well, each of these pairs is working on *one* task. Two people, one task. Each task is a change that has to be made to a system the whole team is responsible for maintaining.

You see, software by its nature is always a *custom machine*. As a machine, it has many parts that need to be perfectly fit in order to operate correctly. Software is highly reproducible; there's no need for the same software to be written twice. Thus all of the software a team may work on is essentially custom and unique. And as such, only team members know the details of how the pieces fit together, and equally importantly, the "why" behind those choices.

Accordingly, pair programming has a few desirable features:

- All changes are personally owned by both team members; they are both responsible for the change.
- Both team members now have knowledge of the change, and will carry it forward throughout the project.
- If members of the pair have competing visions for how the system should work, they *must* resolve and compromise in order to compete the task.

The latter point is worth expanding on slightly. As we mentioned before, software is a machine that requires perfect fit in order to operate correctly. But in order to maintain perfect fit, all of the changes made to a system must be concordant: they must work well together. When people work alone, differences in understanding and vision for "how the system *should* work" grow - sometimes without being noticed until it causes a catastrophic problem. By asking team members to pair-up to complete tasks, a team is trying to highlight the differences in vision *as soon as possible* so they can be resolved to all parties satisfaction.

After all, a conflict unnoticed will fester.

Now, those features are *fantastic*, and they're the biggest part of why we choose to do pairing at all. But there are a few ancillary features that are also quite powerful:

- Team members are constantly cross-training. They observe each other's techniques, teach them to each other, and improve.
- Team members have to practice communicating complex ideas to each other. Over time, this will result in a powerful team language that can share detailed information quickly.
- Regular, day-to-day complaining about obstacles or challenges is more easily converted to *action*. This is because each *pair* has a warrant for improving the system: if both members of a pair want to do something, *they can*.
- Regular reassignment of pairs will further distribute the benefits of pairing in a multiplicative way.
- Teams that rotate pairs regularly become extraordinarily good at on-boarding new team members, because the skills used when starting a new pair are the same, no matter the origin of that new partner.

So this is *why* we would actually do something so *apparently* sub-optimal. And thinking pairing is inefficient is not irrational at all! Pairing is a form of incurring up-front costs in order to produce long-term efficiencies. Software is a distance race, and pairing is how a team sets a sustainable pace.

But what does it feel like? What are you actually *doing* in a pair? Here's how it might go.

Natalie arrives at work and sees Steve chatting with someone else at the office. Natalie and Steve know that they'll be paired today - Steve sees her, waves "Good morning!" and gives Natalie a minute to get situated at the office. She mixes some flavored water, sits at the workstation where Steve is set up, checks her laptop for any work-emails that need attention (thankfully, there aren't any). Steve walks over.
"All set?" He asks. Natalie responds, "Yep. Tired today. So, did you look up what we're working on?"

Steve directs Natalie to the task description - today they'll be adding a new feature: it'll require a new user configuration setting, and logic that handles the two cases that setting drives. Now, Steve admits that he hasn't worked with the user configuration systems yet, so Natalie takes the opportunity to run him through the high level concept, drawing a picture on a nearby whiteboard of how the system responds to user config changes, including details and terms that connect the concepts to their code.

Steve holds up his hand, and Natalie tosses him a marker. "So... for our task we could..." and Steve adds a couple notes and smaller pictures to the big picture. "Is that right?..." Steve says, uncertainly. Natalie tilts her head, then nods. "Well. Yeah. Yeah, that works except for..." Natalie grabs a red marker and draws a big circle around part of Steve's notes. "That'll be trickier than you think. It's hard to describe here, but you'll see when we get there". The two decamp and return to their workstation.

Steve takes the help, and Natalie directs him - showing him the parts of the code that relate to the picture she had previously drawn. After this orientation, they decide to start writing components that will add the feature. For the first component, Natalie plans and directs, while Steve types the code. For the second, Steve directs, exercising some of his new-found knowledge, and Natalie types, adding in little flourishes that Steve ends up liking (after a moment of confusion). The third component seems more complicated, so they spend more time planning out how it should work before doing any typing at all. Satisfied with this, they switch the keyboard back and forth while coding. The person not currently on the keyboard isn't directing so much (as they already have a clear plan), but more providing editorial commentary and proof-reading.

The day goes by quickly, and the feature is soon usable, but not quite complete. Natalie turns to Steve. "So, we're going to have new pair partners tomorrow... are you comfortable keeping this task and completing it with your new partner?" She asked because the area had been new to Steve. Steve agrees. "Yeah, I pretty much get it now. But you'll be in tomorrow right? We may have a few questions..." Natalie laughs, and says "Yeah I'll be around."  They wrap up for the day, and head home.

Now, there are many, many variations on this story. But some things should always be the same: the pair agrees and *collaborates* on a plan for how to complete their task. They share responsibility for *all aspects* of the task, even when they have a skill or experience differential. And they put in the effort to make sure that both of them improve by *teaching* constantly. This means consistently investing in the *only team* that are experts on the *custom software*.

That's pair programming!

### Postlude

If reading this has intrigued you and you want to learn more, I recommend [this content](https://martinfowler.com/articles/on-pair-programming.html), which is a more *insider* view of what the practice is for, why, and when to consider adopting it. As a professional, of course I have my own recommendations and "rules of thumb" on such subjects, but that will have to wait for another day. On a little research, you will also find many articles on pair programming, with explicitly contradictory recommendations and "best practices". Like all loose social arrangements, what you get this technique  depends on *what your team wants from it* and *a willingness to engage wholesale*. If you want to learn more, please reach out!

I almost forgot - I also built a program with the goal of making it easier for interested teams to try and continue to pair! It is called [Coupling](https://coupling.zegreatrob.com), and the story of how it came to exist is fairly interesting... but that is a story for another day.