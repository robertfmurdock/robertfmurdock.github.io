---
layout: post
title: Do the (Side) Hussle
tags: 
---

I've been an open-source library developer for a while now - ever since I started writing "testmints" in February 2019. So I thought I'd take this opportunity to share some quick thoughts about that process. Here comes a list!

## 1. Staying Up-to-Date

If you've read any of my other pieces, you'll know I care a lot about continuous integration - the process of pulling in new work as soon as it is available and trustworthy. When you're the solo maintainer of a library, it would seem that there's no one else to integrate with, and so many of these principles aren't super relevant.

... Or are they?

The software universe is more interconnected than ever before - your library probably has its own set of dependencies. You can think of these dependencies as other members of your library's development team. And if they're doing good work, you should want to take advantage of it as early as possible.

So I'm saying, hey, update your dependencies! Do it! DO IT!

Which isn't tremendously helpful advice. Everyone *knows* they should be updating their dependencies, but making the time to do it and validate the results isn't easy... especially if you're developing an open-source library on your own time, without pay.

## 2. Validation

When working on a library as a side-hussle, there's a lot of stop-and-go. You might have an hour to work on it, then go for a month with nothing, then do 3 hours, and then go a week. You simply can't rely on your own memory to confirm that everything is working as intended.

So with that in mind, it's extremely helpful to work in small pieces. And for each of these small pieces, add a few automated tests. This will ensure that when you inevitably forget what you were doing during these gaps, the important stuff is defended and described automatically.

Do this *enough* and you'll end up with confidence that if it passes your validations, then your library is safe. Especially since you're strapped for time, and can't double-check every feature you build everytime you make a change.

## 3. Automation

Once you have strong validation, staying up-to-date can become much easier, using automation.

Which is to say, when you trust your test to defend functionality, you can use automation to make the annoying work of updating libraries dissolve away. I've used a number of different techniques for this over the years, but here's my current recipe:

1. Schedule a daily job that will run a program that automatically updates your dependencies to their latest version.
2. If anything updates, open a pull request with the changes. Set this pull request to automatically merge to your trunk branch if all checks, tests and validations pass.

This way you hold your library authors accountable the same way you hold yourself accountable - by using validation to confirm that nothing breaks. If you still find yourself worried about this, ask yourself - do I have any way of automatically knowing if this library works - and then see if you can write one.

After this runs for a while, you'll have clear indication about which of your dependencies tend to logjam the system (for example, the AWS v3 js library was notorious for a time). The trail of failing PRs will let you catch up on your own time... or hold back that particular version if fixing it is untenable.

## 4. Keep it Simple

All of the previous points should highlight something - yes, you can use dependencies to provide features, but you probably should choose carefully, and wisely. Every dependency that your library depends on both adds a maintenance burden to you, but also asks consumers of your library to include it as well (be it implicit, or explicit).

Use this pressure to keep your library focused on doing one thing well, so as many people as possible have the option of using it without the extra overhead.

## 5. Use It!

I can't emphasize enough... the best way to ensure that your library is useful is to actually use it. If you can work it into your actual work somehow, it'll give you real, meaningful feedback that will both help you evolve your tools and give you the energy to check back in with it.

## Last Thoughts

If you're looking for tools that can help with automatically updating, I've had good experiences with:

https://github.com/raineorshine/npm-check-updates
https://github.com/ben-manes/gradle-versions-plugin
https://github.com/littlerobots/version-catalog-update-plugin

And I'm sure there are more good tools out there.

So what do you think? Got any other questions? Let me know - this is just the stuff that's floating on the surface of my brain on the subject.

- Rob