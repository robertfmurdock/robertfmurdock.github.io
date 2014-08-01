---
layout: post
title: Identity Crisis
tags: swift squishy
---

I want to talk for a second about a problem I ran into recently with Swift.

I discovered when updating from the original XCode 6 beta to the beta 4 that there was a new problem - suddenly, I couldn't make a list with one of my protocols! This greatly distressed me, as substantial parts of the game I'm porting depend on ordered arrays with different classes but a similar protocol (in the original Java code, classes and interfaces, natch).
