---
layout: post
title: Identity Crisis
tags: swift squishy
---

I want to talk for a second about a problem I ran into recently with Swift.

I discovered when updating from the original XCode 6 beta to the beta 4 that there was a new problem - suddenly, I couldn't make a list with one of my protocols! This greatly distressed me, as substantial parts of the game I'm porting depend on ordered arrays with different classes but a similar protocol (in the original Java code, classes and interfaces, natch).

Here's the protocol as it was defined:

  @class_protocol protocol UnitBuff : Equatable, Printable {
  
  }
  
  func ==(left: UnitBuff, right:UnitBuff) -> Bool {
      return left === right
  }

Seems pretty simple, right? This is a case where the only thing the classes implementing the protocol need to do is be equatable by instance (made possible by the @class_protocol flag), and for debugging purposes be printable (so I can quickly tell which buffs are applied in logging).

Unfortunately, in the new version of Swift, that equals function does not compile: the error it provides is
  Protocol 'UnitBuff' can only be used as a generic constraint because it has Self or associated type requirements
  
