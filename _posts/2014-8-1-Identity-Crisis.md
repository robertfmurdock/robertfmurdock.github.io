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

Unfortunately, in the new version of Swift, that equals function does not compile: the error it provides is:
  Protocol 'UnitBuff' can only be used as a generic constraint because it has Self or associated type requirements
  
Now that's already a problem, because it implies that in order to compare two UnitBuffs, they MUST already know that they're the same type - That would be the effect of using a generic with the rule that the type must implement UnitBuff. And making it even more problematic, the same error occurs when declaring an array as such:
  var buffs : [UnitBuff] = []
  
Annoyingly, for now the simplest solution I have is to change the protocol to be a class, and then inherit from the class instead. But, of course, Swift doesn't currently implement anything like an 'abstract' class, so the 'Printable' protocol that UnitBuff extends immediately causes a failure because the UnitBuff class does not implement it. Ugh. And thus, I have to drop the 'Printable' requirement entirely (which I can do currently because nothing critical relies on it).

If I understand the problem correctly, they changed the Equatable protocol to require Self, which can only apply to a fully formed class or struct. I don't know if Apple is planning to fix this issue but I rather hope they do... making members of a protocol comparable seems like a pretty useful use case.

-------
Update:
-------

Since I've been thinking about this problem more, I'm considering removing the Equality protocol entirely and using some kind of custom "contains" method or closure that will handle the lack of a == implementation. I'll update this post when I get a chance to try it.
