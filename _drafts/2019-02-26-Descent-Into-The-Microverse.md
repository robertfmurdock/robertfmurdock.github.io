---
layout: post
title: Descent Into the Microverse
tags: 
---

Ok, so as promised, its time for the much anticipated "what has Rob been doing with Kotlin?!?!?" post. I know, I know, I really kept you waiting there. No one expected a whole month to go by! A month which I spent... *doing more Kotlin stuff*.

Alright, lets get to the big hearty chucks. One of the things that I ran into immediately while doing Kotlin multiplatform development was that all the *super cool testing tools* that people have written for Java and Kotlin are essentially useless. [Kotlintest](https://github.com/kotlintest/kotlintest)? Unusable. [AssertJ](http://joel-costigliola.github.io/assertj/) or [AssertK](https://github.com/willowtreeapps/assertk)? Fuhgeddaboudit.

All you get is a knife, and [kotlin.test](https://kotlinlang.org/api/latest/kotlin.test/index.html). Figure it out!

### Figuring it out.

And so I did. Inspired by some of the neat sugars that [Kotlintest](https://github.com/kotlintest/kotlintest) provides, I ended up writing a micro-framework for testing that uses some of kotlin's lesser-noticed features to add some semantics. So a test that looks like this:

    @Test
    fun plusOne() {
        // Setup, or Given
        val input: Int = Random.nextInt()
        val expected = input + 1
        // Exercise, or When
        val result = input.plusOne()
        // Verify, or Then
        assertEquals(expected, result)
    }
    
and make it look like this:

    @Test
    fun plusOne() = setup(object {
        val input: Int = Random.nextInt()
        val expected = input + 1
    }) exercise {
        input.plusOne()
    } verify { result ->
        assertEquals(expected, result)
    }
    
Now, setting aside the question of whether the code is *actually improved* by the sugar or not (I think generally being able to replace comments or implicit spacing rules with intent-declaring functions is usually pretty tops), doing a quick run through of why this works might be useful. 

Firstly, we're defining the function using an equals sign rather than a typical block:

    @Test
    fun plusOne() = 
    
Now, there's nothing stopping you from using the setup function inside a traditional function block, but there really isn't anything we'd want someone writing a test to put outside of the setup/exercise/verify sections, so omitting the block makes it less likely someone will have rogue code in there.

Next, you'll notice we're passing an anonymous object declaration to the setup function.

    @Test
    fun plusOne() = setup(object {
        val input: Int = Random.nextInt()
        val expected = input + 1
    })
    
As long as we stay within this file, the anonymous object can be used as a generic type. This is super cool! This means we can define properties (such as input and expected) on this anonymous object, and then use them wherever we pass that anonymous object. In this way, you can easily setup a unique set of inputs for each unit test. And because its just an object, you could use a traditional named class or object as well, which can be useful for sharing setup!

So lets see what happens with the exercise function:

    }) exercise {
        input.plusOne()
    }

So the exercise function takes a closure of type "fun C.() -> R". What does that mean? Well, it means it has "this" access to all of the fields available from the object passed to setup via the C generic. It also means that whatever is the last line of the closure will be returned as the result. Which makes sense, because the typical function that we're testing will probably return something that we'll be interested in later. How much later? Well, that returned value "R" will be passed on to the verify section!

    } verify { result ->
        assertEquals(expected, result)
    }

Now, this you can probably guess how it works broadly - it receives the result as a parameter, and then you can do assertions on it! But there's a little bit more going on here. The closure passed to verify is of type "C.(R)->R2". Because this closure *also* has access to C, this closure can see the setup object, and access its values (for example, the value "expected").

So that's the broad structure. I glossed over how the functions all are using trailing closures, and how the exercise and verify functions are both "infix" which allows us to call them using a space rather then a dot (although if you prefer a dot, that'll work too). But they are!

### Simple Spy Work

Because I've been trying to play with a more functional style of coding, it was fairly simple for me to create a test double utility object that met my basic needs. Here's an example of usage:

    class StubCreatePairCandidateReportActionDispatcher :
            CreatePairCandidateReportActionDispatcher, 
            Spy<CreatePairCandidateReportAction, PairCandidateReport> by SpyData() {
        override fun CreatePairCandidateReportAction.perform() = spyFunction(this)
    }
    
Unpacking it for a second, this is a test double for the "CreatePairCandidateReportActionDispatcher" function. In order to hook it up, the class simply implements the Spy interface, with the Input & Output generic types, uses a delegate SpyData object to provide the implementation, overrides the function being doubled "fun CreatePairCandidateReportAction.perform()", and calls the spyFunction to register the data.

Now, I dig it - usage is verbose. This is mostly a consequence of keeping this test-double style reflection-free, but there may be more tweaks to make it briefer.  Note thought - Staying reflection-free is important in multi-platform contexts because it will not always work as you might expect on different platforms.

That said, for me, keeping down mocking verbosity isn't exactly a primary goal... I don't think its healthy to be mocking constantly, so having a slight bar to it has some value in my mind. What IS valuable though, is avoiding writing all the repetitive data collection code that can easily be divergent or buggy with repeated implementation, so the "real" work being done here is that SpyData() class. It essentially acts as a collector object that can easily be added to any class using a delegate.

Creating this class made it *so much easier* to port existing javascript tests that relied heavily on jasmine spies, so its a worthwhile exercise. This is also kind of a sequel to [this blog post](/A-Synthetic-Creature) from a few years ago, which is always fun.

### Multi-platform quirks

Most of the effort around Kotlin multi-platform programming in January was focused on "getting Kotlin Javascript to work". If you want to learn more precise details about the techniques I ended up using, I recommend reading the Coupling source. That said, big lessons:

- Kotlin produces a nice unified JS file of every project module (and another one for test code). Knowing what this module will be named is very important when folding it into a webpack build. Remember, if you care about the size of your webpacked modules, then take advantage of [Kotlin Javascript DCE](https://kotlinlang.org/docs/reference/javascript-dce.html), to remove stuff from your libraries that are not being used.
- The kotlin multi-platform gradle plugin doesn't do a great job (or any job) of providing the javascript from a referenced library to a project. That is to say, if I make project A depend on project B, the plugins don't do any work to make the artifacts of project B available to A. I ended up writing a custom plugin that helps with that based on some code in the official kotlin frontend plugin, but its not correctly cache busting yet... which means things work, but occasionally I still have to force a manual clean so things get updated correctly.  It was a lot of work and learning, but now I can have kotlin multi-platform libraries that can be consumed by my kotlin javascript targets.
- Async testing is possible using kotlin.test and Jasmine... so long as you return a promise from the tests. This means I've gone *hard* on learning how to use the Kotlin Coroutine system, and its Javascript quirks. Overall I'm actually pretty happy with it, but I can see how some people would bounce off of it.