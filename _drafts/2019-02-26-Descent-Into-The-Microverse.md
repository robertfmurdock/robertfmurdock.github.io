---
layout: post
title: Descent Into the Microverse
tags: 
---

Ok, so as promised, its time for the much anticipated "what has Rob been doing with Kotlin?!?!?" post. I know, I know, I really kept you waiting there. No one expected a whole month to go by! A month which I spent... *doing more Kotlin stuff*.

Alright, lets get to the big hearty chucks. One of the things that I ran into immediately while doing Kotlin multiplatform development was that all the *super cool testing tools* that people have written for Java and Kotlin are essentially useless. [Kotlintest](https://github.com/kotlintest/kotlintest)? Unusable. [AssertJ](http://joel-costigliola.github.io/assertj/) or [AssertK](https://github.com/willowtreeapps/assertk)? Fuhgeddaboudit.

All you get is a knife, and [kotlin.test](https://kotlinlang.org/api/latest/kotlin.test/index.html). Figure it out!

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