---
layout: post
title: The Flavor of Mint
tags: testing, TDD, test design, priorities, coding
---

Given the principles described in [The Quality of Test](/2020/04/24/TheQualityOfTest), I recently started to write a library that might *nudge* a developer to keep them in mind a bit more.

To understand how I approached this though, you first have to understand that any single-feature test has an intrinsic structure - its natural form. There have been a number of different ways of describing this structure, but this one in particular I find useful: [The Four-Phase Test](http://xunitpatterns.com/Four%20Phase%20Test.html).

I'll take a stab at describing this myself.

This of your test as a simple, no argument function, that returns a result: Success or Failure (true or false). I know, most of you reading this are programmers, so I'm going to ask you to purge your mind of the organization provided by test frameworks for a moment, and think about the pure thing - a single function, no arguments, success or failure.

    fun anyTest(): Boolean {
        // test content goes here!
        return false
    }
    
Got it? Good. Now consider an extremely simple model of testing: inputs go in to the function under test, and outputs come out, and we check the outputs match our expectations.

    fun anyTest(): Boolean {
        val input = 15
        val output = functionBeingTested(input)
        return output == "FizzBuzz"
    }
    
We see some distinctions emerging: right in the middle of the test is the inflection point: the moment when the functionBeingTested is actually used. From here on out, we'll refer to that as the "Exercise" section of any test. An "Exercise" section should *only* operate the feature under test, and nothing more.

At the top of the test, we prepare the input: let us call that the "Setup" of the test. The "Setup" is where we prepare any and all data required for "exercising" the test target.

At the bottom of the test, we compute whether we succeeded or failed, and return that value. Let us call that the "Verify" section.

So if we decorate this test with comments indicating what everything is doing, it looks like so:

    fun anyTest(): Boolean {
        // Setup
        val input = 15
        // Exercise
        val output = functionBeingTested(input)
        // Verify
        return output == "FizzBuzz"
    }
    
Now, if we are operating purely functionally (with no side effects), this is enough structure to handle all tests. However, sometimes we have to test things that require modifying non-local state in some way - for example, the filesystem. Let us consider an example that highlights this, that expects the functionBeingTested to respond to data written to a config file.

    fun whenFileContains15WillReturnFizzBuzz(): Boolean {
        // Setup
        val pathInput = "./config.txt"
        val content = 15
        File(pathInput).writeText(content)
        // Exercise
        val output = functionBeingTested(pathInput)
        // Verify
        return output == "FizzBuzz"
    }
    
The structure still holds!... with a caveat. On each call of the test function, it is going to create the file. This may not be desirable for a number of reasons, especially in resource constrained situations. So in order to clean that up, we may need to do work that is not *strictly* part of the test, but is still required because of prior side effects.

    fun whenFileContains15WillReturnFizzBuzz(): Boolean {
        // Setup
        val pathInput = "./config.txt"
        val content = 15
        val configFile = File(pathInput)
        configFile.writeText(content)
        // Exercise
        val output = functionBeingTested(pathInput)
        // Verify
        val result = output == "FizzBuzz"
        // Teardown
        configFile.delete()
        return result
    }
    
And thus we discover the last phase of the Four-Phase Test - the Teardown. A normal teardown may be entirely invisible (the natural deallocation of all the test content), but depending what you setup, you may have to add teardown content.

But WHEW. With all those comments in there, it is getting harder and harder for the eye to quickly see what this test is *all about*. And even if we remove those markers:

    fun whenFileContains15WillReturnFizzBuzz(): Boolean {
        val pathInput = "./config.txt"
        val content = 15
        val configFile = File(pathInput)
        configFile.writeText(content)

        val output = functionBeingTested(pathInput)

        val result = output == "FizzBuzz"
        configFile.delete()
        return result
    }
    
It *still* takes some time for the eye to figure out what's going on here... and in order to maintain any test well, you need to be able to understand it... especially its *primary goal*.

Given this problem, in addition to knowing the natural structure of a test, and especially knowing  *these examples are extremely simple and things only get more complicated from here out*, I wanted to do what I usually do when confronted with a consistent structure: I tried to factor it into my code in a way as easy to use as possible.

So I wrote a little Kotlin library called "testmints", that makes an attempt to benefit from the formalism of the *structure* of tests (which should encourage the author to remember the testing principles enumerated earlier), without bogging down the test with *even more bloat*. Because, as we see with the comments, it is actually very easy to attempt adding clarifying content that *in practice* reduces overall code readability.

That's a struggle, and not one I claim I've solved. But still a problem worth investigating.

So here's our the same test can be written using today's version of the "testmints" library (and an assertion library).

    fun whenFileContains15WillReturnFizzBuzz() = setup(object {
          val pathInput = "./config.txt"
          val content = 15
          val configFile = File(pathInput)
      }) {
          configFile.writeText(content)
      } exercise {
          functionBeingTested(pathInput)
      } verifyAnd { output ->
          output.assertIsEqualTo("FizzBuzz")
      } teardown {
          configFile.delete()
      }
      
Now, its funny. The test has a very similar number of lines, but a few of the blank lines have been replaced by illustrative function calls: setup, exercise, verifyAnd, teardown. So this is most comparable to the version of the test that highlights the different sections with comments. However, a few more ideas are in play here:

1. The test is built as a series of functions performed on the "setup" object. The nudges the test author to think of the test in as purely functional a way as possible (while allowing for impure, side-effect producing functions).
2. Because the structure of the test is now *actually modeled in the code*, a number of things that had previously been so difficult as to be impossible suddenly become possible. Exceptions or errors happening *outside* of the verify section can be clearly marked. Performance can be measured independently for setups, exercise, and verifies, making it easier to distinguish test-design performance problems from problems with the code-under-test.
3. Setup objects, exercise functions, and verify functions can *themselves* be easily shared, reused, and recycled. Because this is a simple sequence of functions, all the regular refactoring options available to you can be used.
4. This structure uses Kotlin language features heavily, most notably combining generics and receiver parameters to provide the setup object to all the following functions... even though it is an anonymous type!
5. By using a setup object (rather than putting variables on a function scope, as is tradition), it nudges the test author to prefer pure data as setup input, which helps setups remain static and simple.

Combine this sort of structuralized test framework with a powerful log and statistics aggregation system, and suddenly tracking test performance over time becomes much more accessible to programmers. Before you used to simply discover "ugh, this test tends to be slow" and then, if interested, you'd have to do your own analytics to find out why. Now, with structure built into your test, you can get the insight as to *why* it might be slow without as much manual setup. Surprise! Your style of verification is the problem. Or maybe your setup, that you imagined would be trivial, is actually a performance burden.

For those of us who've worked hard to maintain tests in the past, this may be very attractive. I myself find it *enchanting*. By normalising the *form* of the test, we can add features that take advantage of the form.

Here's the trick: in order to take advantage of these things, it requires test-authors to actually adopt a structuralist approach. It could be by using my library, or another one... once the general programming community seizes on the idea, I'm sure they can come up with something better than what I've got!

I'd love to hear people's thoughts on the subject. If you want to play around with [testmints](https://github.com/robertfmurdock/testmints), give it a shot! I've been using it in some form for a while now for my own projects, so the dog-fooding is strong with it.
