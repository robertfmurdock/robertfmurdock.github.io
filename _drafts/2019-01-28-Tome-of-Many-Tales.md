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
 
   