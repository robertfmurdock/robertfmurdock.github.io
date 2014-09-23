---
layout: post
title: Filibustering Under Oath
tags: swift flow promises pledges optionals
---

Back again! Today I'm going to say a few words about one of the other Gists I put up last week: Pledges.

Swift has fantastic notation for modeling the "one or none" problem using Optionals. They let the programmer use syntax like this:

    let result: String? = goGetMeThatResult()
    if let value = result {
      // Do amazing things with the value you recieved!
    } else {
      // What does it mean to not have a value? Ponder this, then take action.
    }
    
Which is great for modeling situations where a person may / may not have something. Like a ticket, for example. Or a wallet. What's more, Swift optionals don't let you attempt to use a value without validating that it actually exists, which makes them far superior to the classic 'use a null' solution... which is traditionally the source of all evil. Beware the dread pirate NullPointerException.

But. Sometimes the reason you don't have a value might be that the program wasn't able to successfully check whether you have it or not. Say, the computer can't read the disk, can't talk to the web service, or can't read the terrible handwriting you're trying to scan. This structure, as written, can't tell you why you didn't get a value in error cases.

And so, I wrote Pledges: in order to use the power of the promise design pattern to be able to safely display and chain together errors.  Here's what some code might look like written with Pledges.

    let promise: Pledge<String> = goGetMeThatResult()
    promise.then({ result in
      // Do amazing things with the value you recieved!
    }).fail({ error in
      // Holy crap, we couldn't get the value because of an error! We'd better deal with this and log it somewhere.
    })
    
And because we're using Swift, we can make it even briefer:

    goGetMeThatResult().then { result in
      // Do amazing things with the value you recieved!
    }.fail { error in
      // Holy crap, we couldn't get the value because of an error! We'd better deal with this and log it somewhere.
    }
    
Or, if you want to be as insanely brief as possible:

    goGetMeThatResult()
    .then { println($0) }
    .fail { println($0) }
    
Kind of nice, that. I chose to use the word 'fail' instead of 'catch' for aesthetic reasons (4 characters to match 'then', so all the blocks line up).

But the real power of Pledges comes when you need to hinge a larger amount of loading based on various inputs. Lets say that you want to build an object KeyChain that depends on two strings and an integer from various services. The functions I provide with Pledges let you do this (where the goGet methods all return Pledges):

    let keychainPledge: Pledge<KeyChain> = all(promises: [
      goGetStringOne(),
      goGetInt(),
      goGetStringTwo()
    ]).then { string1, id, string2 in
      KeyChain(seed: string1, id: id, password: string2)
    }
    
The all function lets you combine existing pledges into a larger pledge easily, feeding you all the inputs of those pledges when they succeed as arguments so you can compose your final result. This naturally lets you chain any errors together with no problem, with a syntax much safer than the traditional "try-catch" structure. And there are a few variations of the way you can make a new promise from those results besides this. I'm time-strapped though, so I won't go into all of it now.

Let me know if you like and/or use this in anyway! Happy to help.
