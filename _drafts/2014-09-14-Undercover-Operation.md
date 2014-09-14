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

Now, as far as I know, there are currently no mock frameworks available for Swift. To fill in the gap somewhat, I created a Spy object inspired by the design of Jasmine spies. Given that, here's a quick way to create a mock object using the Spy class:

    class MockBlender : Blender {
        
        var transform = Spy<(value1: String, value2: Int, value3: Double), String>(defaultReturn: "MOCK")
        
        override func transform(value1: String, value2: Int, value3: Double) -> String {
            return transform.call(args: (value1, value2, value3))
        }
    }

The Spy class is designed to collect information about one particular function call, so for each method you would like to mock, you create a Spy. A Spy has two generics types associated. The first is a tuple of the arguments of the function you're spying on - in this example (String, Int, Double). The second is the return value of the function, and Void is used if your function does not return. The 'defaultReturn' argument of the constructor is mandatory.

The Spy has a few useful features:
  - spy.calls is an array of all the arguments used to call the functions as tuples. You can use spy.calls.count to determine how many times a function was called.
  - spy.returnValue(value: Return, whenCalledWith: Args) allows a spy to conditionally return a value when the arguments tuple the function is called with matched the "whenCalledWith" args. Setting up the matchers is required to make this work...

Setting up matchers on your Spy allows 'returnValue' and 'verify(wasCalledWithArgs)' to work seamlessly. I'm not yet happy with verboseness of the setup, but its fairly easy to do. Example:

    class MockBlender : Blender {
        var transform = Spy<(value1: String, value2: Int, value3: Double), String>(defaultReturn: "MOCK",
            argComparators: [{ $0.value1 == $1.value2 }, { $0.value2 == $1.value2 }, { $0.value3 == $1.value3}])
        
        override func transform(value1: String, value2: Int, value3: Double) -> String {
            return transform.call(args: (value1, value2, value3))
        }
    }
    
Basically, you pass in a function or a series of functions that is used to compare one set of arguments to another, in whatever way you like. This property can be changed on the Spy at any time. Once this is done though, you can make simple calls like:

    mockBlender.verify(("Blend me", 7, 3.2))

And your test will fail is no calls match those arguments.
