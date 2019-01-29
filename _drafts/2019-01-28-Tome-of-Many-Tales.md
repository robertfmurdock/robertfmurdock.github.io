---
layout: post
title: The Tome of Many Tales
tags: 
---

Hello friends! It has been many a moon since I've updated this here rant-box, and its time to collect some thoughts!

First off, I haven't updated in a while because, well, when work is interesting, I work, and don't blog, seeing as no 
one is currently paying me for these proto-essays. This is normal. Everything is going according to plan.

That said, I've had the blessing of being able to do a lot of independent research this January while working on a few 
professional things that will go unmentioned here. Take that, professional things! So here I'm going to take a second to 
enumerate the things I've been learning, and provide links to source code to help other people research, should they so 
desire.

Phew! Enough preamble, lets get to the meat!

The Meat
--------

### Coupling Sign In
First up, I got a scary email from Google indicating that the Coupling application (discussed in previous 
post [here](/A-Gemini-Dilemma)) would have problems in March because Google was disabling the Google Plus authentication
 APIs. This meant that I needed to switch over to the newer, fresher, all-together-better, Google Sign-In APIs.
 
 So I did! Most of the related code is [here](https://github.com/robertfmurdock/Coupling/blob/e615990d688cca8a6c9ffa3b9b47668243e9ea51/client/app/GoogleSignIn.ts).
 I'll say, it wasn't too bad... the Google Sign-In is all client side, so it posed an interesting challenge to keep the
 Coupling APIs secure correctly. Here's the long and short of it:
 
 On the client side, Google asks you to drop in a script tag that will load their SSO client side logic. This will do 
 magic things (share a Google session via a tiny i-frame, if memory serves) that will allow your users to log in once to
 Google, then be logged in in your application.
 
 On the server side, the server doesn't know that the client is authenticated. So in order to create a session, the 
 client will send an oauth token to the server. The server will unpack it and verify that it is a correctly provisioned 
 token before granting access to relevant data. Google even provided a node.js library for validating the token! 
 I had to hand-roll a custom passport middleware, but that's just a few lines of code connecting the Google library. 
 It was shockingly simple. 
 
 But wait, there's more! This made me pretty optimistic about putting in new auth systems, so I've recently added 
 Microsoft SSO to Coupling as well (... and hopefully in the short term future there will be a button providing this!). 
 Why did I investigate this? Well, some of the client's I've worked with in the past have deals with Microsoft, and 
 haven't used Coupling because it required Google login. Now... they can! Microsoft rolled their own server-side passport 
 middleware for this, and you can see how its used in Coupling [here](https://github.com/robertfmurdock/Coupling/blob/17950963b8557ae953ff77504f60a77dffa1a2fa/server/config/express.js#L21).
 
### Amazon Container Server
I had a minor freak out this year when I heard that docker-cloud, formerly Tutum, was shutting down... this was the tool
I was using to orchestrate my personal web apps and services. In a flurry of desperation, I moved Coupling over to be 
managed on the fledgling Amazon Container Service (ACS), and managed to get it running in a fairly quick amount of time.

This was nice! The concepts / terms they use mapped pretty well to what I'm used to, and since it was docker image based, 
moving over from docker cloud required no code changes.

That said, in that transition I lost one of my favorite docker-cloud features, which was "auto deploy on publication of 
new image". Big sad face.

### Circle CI Workflow
However, this January I spent some time trying to replicate the aforementioned feature. Using CircleCI, I was able to 
put together a CircleCI workflow that would tell ACS to redeploy my image after CircleCI built and pushed the new one! 
It took a bunch of research on my part, but it got me the results I wanted. The script is [here](https://github.com/robertfmurdock/Coupling/blob/004f06ad0bb425258b3ca3e4deea8d4a0c495b97/.circleci/config.yml#L42).
My favorite thing about that script is that it uses multiple docker images rather than having to construct a new base 
image with all of the software I need. I didn't have to install the aws command line tool at all! Docker is magic, 
CircleCI is magic. Merry Christmas, everyone.

### (Webpack) Stats for the Pack-Rat in Your Life
If you've been working on the web with webpack, and you've done any optimization at all, you're familiar with the stats 
output. Its so useful! It gives you a picture of "what the heck is taking up so much space in my javascript files?!?" 
and "who on earth decided we needed to use ANOTHER variation of lodash?". But if you're like me, you might run into tiny
 little roadblocks that prevent you from using this tool as much as you like. Mostly, its inconvenient.
Well, because Gradle is great and there were no signs of my webpack system getting any simpler, I decided I needed a
webpack stats task dedicated to getting me that sweet, sticky, stat-sauce. Check it out [here](https://github.com/robertfmurdock/Coupling/blob/cf8d13ec1e95f085034aadc729e020663c4eea16/client/build.gradle.kts#L71). I do recommend making 
this as easy as possible, because the more people on your team paying attention to the health of your bundles, the 
healthier they'll be! 

### Modularization and You - Everybody Boops
Alright, hey, look. I know the Coupling project has been a mess for a long time. It has desperate need of organization. 
No link because its an ongoing work, but I spent a good amount of time trying to bring some order to its chaos. 
Hopefully the changes made (and further changes to be made) will make it clearer which parts of the app are server only, 
which are client only, and which are part of the end to end tests. In some future world it'd be nice if it was a better 
demonstration of quality organization. That said, seeing as this is a free time project... no promises!

### The Squarm is Squarming!
Many of the previous blog entries were talking about the Swift language, and using examples from a game that I'd worked 
on in my free time. I took two hours to update the game to Swift 4 and... it was pretty painless! Also, it runs better
on modern hardware.  I should get back to it, or at least add three more maps and see if I can make a quick buck. Can't 
show any source here, sorry! 

### Injection by Ice Pick
In the last quarter of 2018, I spent a goodly amount of time examining how the principles and techniques outline in 
[this article](https://www.pacoworks.com/2018/02/25/simple-dependency-injection-in-kotlin-part-1/) might be applied to a
somewhat traditional project. To summarize the article, in Kotlin, dependencies can be defined in such a way that the 
dependency injection can be validated at compile-time rather than at run-time. Using this technique means taking 
advantage of Kotlin's ability to put fully-formed functions on interfaces, alongside 'abstract' functions. To do so, 
the programmer merely defines the dependency as an abstract property, and then uses the dependency in the implemented 
function. Next, in order to use the function, at some point in the code the interface must be implemented, and thus the 
dependency must be linked. Apply this technique repeatedly, and you discover that you can remove a *lot* of intermediate 
dependencies (replaced by intermediate functions), and you no longer have any of the problems associated with DI tools.

Sounds cool? Its kind of a trip, but it works. Now, actually applying it to an existing codebase suggested that some 
architectural organization was missing, so I proposed a few useful naming conventions for different kind of functions 
and function parameters that will commonly be used throughout a large codebase. Quick summary!

- Commands / Queries are simple data objects that capture all the information needed to execute a user-requested 
behavior. The name and type of the Command or Query is important because it signifies the intent of the user.
- Actions are simple data objects that capture all the information needed to do a particular function. Command and 
Queries are both considered Actions. Actions are *not* associated with a user request, and thus can be recycled, 
repeated, or orchestrated with a Command, a Query, or another Action. 
- Dispatchers are the functions that do the work associated with an Action. These functions are implemented on 
a dedicated interface, which enumerates all the dependencies the function requires. In this way, the Dispatcher objects 
are where dependencies are injected, *not* the Actions themselves.
- Syntaxes are functions that provide existing types with additional functionality. These functions are implemented on 
interfaces, and thus can link a dependency to the type being given "additional syntax". These functions are expected to 
be lightweight and highly reusable. Typically, a Dispatcher would implement syntaxes required to implement the 
dispatcher function.

Sort of make sense? Clear as mud? Its also probably worth pointing out that these structures are intended to be in a 
core application layer (aka business aka domain layer).

Anyway, this is all well and good, but I decided a better, publicly available example might be useful. So I spent a 
Friday creating [this git repo](https://github.com/robertfmurdock/kt-di-example), which in a series of commits will 
illustrate how to test drive code organized in this way. Each step of the process has an associated branch, including 
failing tests (so its easier to see what needs to be done in micro steps), and an integration test that fails until the 
work is complete. Questions welcome!

