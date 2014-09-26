---
layout: post
title: The Exceptional Doorway
tags: flow promises error handling ethics syntax
---

I think about error handling a lot. Perhaps a shocking amount. I mean, it's the first example I reach for when discussing [programmer ethics](/Heart-Of-Silicon). Its one of the first things I investigate when learning a new programming language. And my opinions (read: rants) on the topic tend to be long, detailed and interminable.

So I thought, what the hey, why not discuss it in long form here! Lets dig in.

Once upon a time, in a far away land, programmers started writing computer instructions. And it was good. Then, error entered the world due to the frailty of the silicon and the foolishness of the programmers.

We've been coping with errors ever since.

In the time of our father's fathers, detecting and handling errors was straightforward: memory was limited, programs were small, and the range of problems were few. This was generally handled by reserving a register for an error. After doing actions that could result in an error, the code would check that register for a value and respond appropriately, handling the error or continuing with execution of the program.

The code for this style of error handling might look like this (in C-like code):

    int error;
    
    void main(){
      RESULT result = performFunctionOfDubiousSuccess();
      if (error) {
        handleError(error);
      } else {
        enjoyThe(result);
      }
    }
    

All things considered, this is an elegant solution... but not a perfect one. In order for this technique to succeed, a few requirements must be met:
  
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

Here's how it looks in practice:

    public void main(){
        try {
            Result result = performFunctionOfDubiousSuccess();
            enjoyThe(result);
        } catch (Exception ruhRoh) {
            handleError(ruhRoh);
        }
    }

Improvement though it may be for certain things, Exceptions still make substantial demands of the programmer. Such as:
    
    - The programmer must know each and every function that might return an exception.
    - The programmer must handle each and every kind of exception that can be returned in an appropriate way.

This is likely the right time to introduce another related topic: the difference between critical errors and recoverable errors.

Traditionally, critical errors have been classified as "errors that indicate profound programming mistakes for which the correct course of action is to terminate the process after some degree of cleanup... when possible." Classic examples include attempting to access invalid memory (such as array index out of bounds) and failing to allocate memory. Programmers should have zero tolerance for critical errors, and use more advances techniques and more advanced programming languages to make the situations tremendously difficult to achieve.

Recoverable errors are errors that are actually fairly likely to happen over the lifetime of a program in production. Some common examples are "file not found" or "file does not have correct permissions for that operation" or "could not parse integer into a string". These errors are properly considered "unexceptional", meaning that your code should make as clear as possible what happens when they occur.
