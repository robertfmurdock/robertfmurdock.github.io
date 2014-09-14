---
layout: post
title: Undercover Operation
tags: swift testing spies
---

I've just published three new Gists, to my Gist [page](https://gist.github.com/robertfmurdock). The reason I haven't posted here in a few weeks is that I've been nearly pathologically working on the Swift game I mentioned earlier. That game led me to develop these three libraries:
  - Pledges, a Promise system for Swift
  - ArmorXml, a system that augments RaptureXml with Pledges
  - ArmorTestKit, a group of functions for simplifying testing Swift code using XCTest. This includes assertions designed for Pledges.
  
Now, they haven't been exposed to much light so far, and therefore I'm sure that there are usability quirks - if you try it out and want to suggest making a change, let me know!  I've been using them as part of the game I'm porting and figured the rest of the universe might find them valuable. My next few posts will be discussing various aspects of these libraries, and for now, I'll focus on one feature of ArmorTestKit: Spies.

Now, as far as I know, there are currently no mock frameworks available for Swift. I'm hoping that this can be a good thing and remind the test-driven that you shouldn't need a whole framework to do appropriate mocking. Given that, here's a quick way to create a mock object using the Spy class:

