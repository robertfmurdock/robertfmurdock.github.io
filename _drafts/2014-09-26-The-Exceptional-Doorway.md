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

Recoverable errors are errors that are actually fairly likely to happen over the lifetime of a program in production. Some common examples are "file not found" or "file does not have correct permissions for that operation" or "could not parse integer into a string". Thus, these errors are properly considered "unexceptional".

In my opinion, when handling unexceptional errors, code should make as clear as possible that the error might occur and how it will be handled. This is in contrast to true critical errors, which occur in conditions that are unpredictable to the program and should not be considered part of the program's typical flow. Only truly exceptional and unpredictable cases should be hidden from the readable flow of code.

Exceptions as a mode of error handling tend to suffer from inability to distinguish these kinds of errors. Some languages introduced the concept of 'checked exceptions' in order to help deal with this problem, but the limitations of Exception handling syntax and lack of discipline in distinguishing whether an Exception should be 'checked' or not has severely limited the usefulness of that construct.

Lets consider a few modes of error handling that asynchronous systems have tended to use. Here's one style of error detection popular among Javascript programmers:
    
        database.findDataMatching('query', function(error, result) {
            if (error) {
                handleError(error);
            } else {
                processTheResult(result);
            }
        });
        
        
This style asks the programmer to pass forward a closure that will recieve two arguments at a future time: an error and a result. This style of error handling has a few nice properties. Because the error is the first parameter, it is very unlikely that the programmer will simply ignore the error and forget to handle it; they must include the error parameter in the function in order to obtain subsequent parameter values. This style of error handling, combined with the nature of asynchronous programming, also demands that access to the result happens somewhere within that closure. This helps to eliminate bugs related to programmers assuming have access to a value that ended up never being populated because of an error (frequently resulting in null pointers).

Of course, the limitations present themselves immediately as well. While it is unlikely a programmer will forget that ever-important 'if' statement, there's no guarentee that it or the 'else' section will be correctly included. Thus the closure is always at some risk of attempting to use a result that does not exist. Additionally, all error handling logic must be included inside of the closure as well. When multiple queries have to be chained together, the code tends to exhibit 'rightward-drift': a result that requires another result that requires another result combined with handling all of the related errors correctly can create some difficult to comprehend closure nests.

To help reduce the unwieldiness of this structure, many programmers have adopted a variation on it: the [Promise](http://www.html5rocks.com/en/tutorials/es6/promises/) design pattern.

A promise is an object that represents the potential of a task to succeed or fail. A promise can succeed or fail immediately before execution moves to the next line of code. It might also succeed or fail at a future point in time.  Here's how using a function that returns a Javascript promise looks:

    requestData("MyFavoriteData").then(function(data) {
        enjoyTheDelicious(data);
    }).then( function(data){
        playWithItSomeMore(data);
    }).catch(function(error){
        weepOpenlyForYourLackOfDataAndReportThe(error);
    });
    
Some quick notes: the 'then' function of a promise returns the reference to the promise! This allows you to continue to add additional 'then' or 'catch' clauses. This is equivalent to storing the original promise in a variable and then calling the functions from the original variable, but sometimes chaining them as in this example is a more readable choice. Even better, most promises include the ability to transform the result:

    requestData("MyDataAsString").then(parseInt).then(function(int){
        doSomethingAwesomeWithThe(int);
    });

As a structure for error handling, promises eliminate a few concerns. There is no opportunity for a programmer to accidentally attempt to use the data when the data will not be available. Rightward drift is eliminated by using transformations (including transformations that may result in errors). Unlike Exceptions, it is impossible for an error to escape from the bounds of a promise; all error handling will be dealt with by the given 'catch' closures. Chaining makes identifying the true cause of an error a breeze. With one glance at a promise, you can tell what the programmer is doing with the resulting error (if anything at all).

The promise design pattern seems to be the most robust and durable solution to error handling I've seen so far. This is likely due to its asynchronous origins, but I suspect this model is ideal even for purely synchronous code. The only difficulty I've encountered with promises is that sometimes when *writing* a promise it is distressingly easy to create a promise that never actually fulfills or rejects its contract. For the [Pledge library](https://gist.github.com/robertfmurdock/8cb608385cc432534f9d), I've been considering adding a default timeout to make this situation easier to detect. But the average programmer shouldn't have to write very many promises at all... most of your contact with promises will be transforming, chaining and adding 'then' and 'catch' clauses.

I hope this essay has been useful!
