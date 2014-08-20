---
layout: post
title: Closing Time
tags: closures functions swift testing
---

I've been powering through converting a lot more of my Java game code to Swift in this last week, and while doing so I've been trying my best to learn about the closure syntax provided. For example, this code snippet is made possible:

        assertNotNil(withinDistance.last) { last in
            assertSame(last, withinDistance.first)
            assertSame(expectedUnitPair, last)
        }
        
So lets take a moment and unpack what this is actually doing. 'assertNotNil' is a convenience function I created to minimize possible forced unpacking of Swift optionals in my test code. Here's what the signature currently looks like:

    func assertNotNil <T>( candidate : T?, andContinue : ((withValue : T) -> Void)? = nil)
    
As you can see, it actually takes *two* arguments rather then the one we might expect from looking at how its used. The first argument is pretty much exactly what you'd expect - a value of an optional generic type. More interestingly, the second value is a function type. That function type takes as its argument a value of the generic type without the optional aspect. It also defaults to 'nil' if no function is passed along, using the default argument syntax.

Knowing this, a programmer might call this method in this way:

        func valueHander(withValue : T) -> Void {
          assertSame(withValue, withinDistance.first)
          assertSame(expectedUnitPair, withValue)
        }
  
        assertNotNil(withinDistance.last, andContinue: valueHandler)
        
And that's totally valid. You could also define it inline... which should look familiar to Javascript programmers.

        assertNotNil(withinDistance.last, andContinue: { last in
          assertSame(last, withinDistance.first)
          assertSame(expectedUnitPair, last)
        })
        
It wouldn't surprise me if some programmers preferred that way of calling the function: it does let you use the named argument, and makes clear that the closure is being passed to the method. But... the trailing closure syntax, which automatically will use the closure in the last argument of the function, lets you create functions that act more like 'if' statements.  Repeating the initial example:

        assertNotNil(withinDistance.last) { last in
            assertSame(last, withinDistance.first)
            assertSame(expectedUnitPair, last)
        }

This code is intended to convey that the block will only be run if the value given is not nil, providing safe test results. This is vastly preferable to using the ! operator constantly (which causes the test runner to crash when it cannot find a value), or copying "if else fail" to every test that has to check for optional values.

This line of thought made me want a similar construct for production code, and so naturally, as programmers do, I made my own implementation of the Promise API so I could create structures like this one:

      findValue().then { value in
                    // Have a party with the value! Woo!
                }.catch { error in
                    // Deal with your problems.
                }
                
If I get enough time to make it presentable, I'll put it up somewhere in the form of a gist or github project.
