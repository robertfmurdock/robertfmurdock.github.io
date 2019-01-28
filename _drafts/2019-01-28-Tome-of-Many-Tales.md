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
   