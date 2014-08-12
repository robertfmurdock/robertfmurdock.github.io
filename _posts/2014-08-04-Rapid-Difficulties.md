---
layout: post
title: Rapid Difficulties
tags: swift
---

As I've been porting more and more code to Swift, I've noticed a problem emerge around 3-4 times already: when writing tests to measure the performance of a section of code, I frequently (though not always) run into EXC_BAD_ACCESS problems.

What I've sussed out so far is that in these situations, the Automatic Reference Counting system (ARC) is somehow deallocating an object while my code still intends to use it. And then, when my code makes the attempt to access it... KA-BLOOEY!... EXC_BAD_ACCESS. Here's what one of the test that gives me problems looks like:

    func testCheckVectorIntersectsUnitPerformance_WhenAwayFromUnit()
    {
        let unit = MockSquishyUnit()
        let location = GameCoordinate(108, 107)
        let startPoint =  GameCoordinate(0, 0)
        let endPoint =  GameCoordinate(100, 100)
        let wall =  BoundVector.makeBoundVector(startPoint, endPoint)
        let wallThickness = 10
        
        let startTime = NSDate()
        for  iteration in 0 ..< 500000 {
            CollisionDetector.checkVectorIntersectsUnit(unit, location, wall, wallThickness)
        }
        
        let endTime = NSDate()
        XCTAssert(endTime.timeIntervalSinceDate(startTime) < 75)
    }
    
Usually when this problem happens, its on the second iteration of the loop. By the way, check out that # of times Swift loop syntax! Great stuff. In this particular example, the unit is being deallocated when the first iteration of the loop finishes, even though there's clearly a reference in the enclosing method.

I don't actually know what precisely the problem here is: the last time I ran into this, I thought it was caused by using structs... but in this case no structs are involved meaningfully.

Basically, Swift is awesome, but the ARC system is still awfully painful, trapped halfway between completely manual memory management and a proper garbage collection system. Hopefully down the road they'll add actual GC to Swift... given the way they've been releasing new versions of the language, it seems very possible for them to add those kind of meaty features. Until then, I suppose we'll have to get used to coding happily along, then periodically getting our legs blown off by a landmine.
