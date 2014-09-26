---
layout: post
title: The Exceptional Doorway
tags: flow promises error handling ethics syntax
---

I think about error handling a lot. Perhaps a shocking amount. I mean, it's the first example I reach for when discussing [programmer ethics](/Heart-Of-Silicon). Its one of the first things I investigate when learning a new programming language. And my opinions (read: rants) on the topic tend to be long, detailed and interminable.

So I thought, what the hey, why not discuss it in long form here! Lets dig in.

Once upon a time, in a far away land, programmers started writing computer instructions. And it was good. Then, error entered the world due to the frailty of the silicon and the foolishness of the programmers.

We've been coping with errors ever since.

In the time of our father's fathers, detecting and handling errors was straightforward: memory was limited, programs were small, and the range of problems were few. This was generally handled by reserving a register for an error. After doing actions that could result in an error, the code would check that register for a value and respond appropriately, handling the error or continuing with execution of the program. All things considered, this is an elegant solution... but not a perfect one. In order for this technique to succeed, a few requirements must be met:
  
  - The programmer must know each and every register (or memory) that is reserved for error detection.
  - The programmer must know each and every action that could trigger an error, and handle them at the appropriate time
  - The programmer must avoid making any assumptions downstream of the error that might be false when an error happens. If a file is not read, for example, code can't assume that the content of the file is available.
  - The programmer must be talented and skilled enough to watch for all of these problems in all of their code, all of the time.
  
For small codebases with few registers, these problems are truly not a major concern. But as soon as code bases grow to even modest sizes, even the most talented programmers began having problems managing error conditions correctly.

Enter the Exception.

Programming languages gave birth to the Exception in a fit of ecstasy, fury and force. The most common form of exception systems follow from a few principles:
  
  - Exceptions shall be an object or struct containing information describing and relevant to an error.
  - Every function shall have an additional type of 'return' statement: throwing an Exception. When throwing an exception the declared return contract (returning a void, an integer, or a pointer for example) shall not be met.
  - Every function will automatically and immediately 'throw' when calling another function that throws an Exception. Lines of code subsequent to that function call will not be executed.
  - A try/catch syntax is provided that will capture any exception thrown by functions called inside the 'try' section and deliver them to the 'catch' section. Exceptions are to be handled within the catch section.
  - If an Exception is returned from the top function of a stack, that program (or thread) will exit with an error code.

These rules made certain things easier for programmers: now, instead of having to design a communication path for errors through your functions, exception handling provides it for you. Additionally, using the Exception as a template for how to capture useful error data helped add consistency to error handling systems.
