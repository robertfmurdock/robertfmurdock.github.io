---
layout: post
title: Intention Expression
subtitle: The Meaning of Driving with Tests 
tags: review
---

After rereading my last essay "The Test Ascent", it occurred to me that I haven't directly addressed the subject of "test-driving", commonly encountered as TDD (test-driven development).

I define test-driving as "designing and implementing new features by declaring *what it should produce* first, then *how it should be produced* second." In this way, any change to a system is predicated on the enumeration of a *new requirement* (or a modification of an *old requirement*). Ergo, tests *drive* the system forward.

Now, I think test-driving has a strict form, and a general form.

The strict form is what programmers may have encountered: by test, we mean *automated* test, as this allows us to verify that *everything in the system is working as intended* at low-cost. This is not hyperbole - when a system has been built exclusively with test-driving, the tests capture the intention behind the entire system.

The general form is slightly more domain agnostic: it could be used for programming, or interface design, or business strategy. At its root, the steps are the same, but the cycles may be longer. Express what results you need the system to produce, and *then* work on the implementation. Capture that expression, so as the system changes and new requirements evolve, you can verify that nothing has been lost. This is a mirror to the scientific method (in its commonly understood, extremely simple form); where the scientific method is analytical, test-driving is constructive. "Hypothesis, observations, conclusions" becomes "test, solution, result". Of course, multiple solutions may be tried, additional tests will be added, but the idea at the root is extremely useful: say what you want, *then* worry about how we get it. Show your work. [^1]

[^1]: Doing this in non-programming domains *effectively* is a deep and worthy topic, as there are a lot of pitfalls and/or ineffective ways to apply this thinking. I may take up this subject another day, but for now I encourage you all to consider the possibilities.

Regardless of form, test-driving is ultimately about one thing: expressing intent. That is to say, a test must successfully communicate to two audiences. The first audience is the machine itself - a test needs to correctly *succeed* or *fail*. The second audience is the team working on the system. This is important. Implicit in the logic of test-driving is that tests are cumulative - as time marches onward, tests have to be added, and the number of features your system supports will grow.

The human audience has a few characteristics. Firstly, they are primarily members of the team, and thus you can depend on a shared team-cultural background (one of many reasons maintaining a healthy team-culture is important). Secondly, these are people who are positively motivated: the tests are not there to *stop them from doing something intentionally bad*. So the tests act as guidelines; stopping well-intentioned errors and basic misunderstandings. Thus, they do not need to be exhaustive - they only need to illustrate to an invested audience *what you were trying to do*.

At their root, tests are fundamentally *linguistic*. Yes, they have mechanical functionality, but they are a description of how the features of your system work.

This is why advocates of test-driving frequently shed industry conventions about programmatic comments - after all, you've already described what it should do once, why do it again? This is why *the dream* of making the tests your *user-facing documentation* is so attractive. They are very nearly the same! Tests are examples of how the features work.

Thinking of "tests as intent" challenges the programmer when desirable properties are *difficult* to test. Is the difficulty in the expression of intent? Is it the assessment of the result? Or is it the operation of the feature? Sometimes we think of things as "requirements" that are mere preferences or bias regarding implementation. Sometimes we decide to declare something *not* a requirement because writing the test would be difficult - we have difficulty expressing ourselves.

Test-driving asks you to consider your choices, and what testing (and not testing) communicates. When test-driving, *anything not tested is not required*. The question is not "should I test this?", but "how will this be tested?". When a software developer decides a desirable feature should not be automatically tested, they are surrendering their personal responsibility for the functionality to other parties. The feature *will* be tested - the only question is, who will do the testing? Will it be some other role (perhaps a quality assurance team)? Will it be your customers? Who?

As a test travels further downstream from feature development, performing the test becomes more and more expensive, as does correcting errors that the test detects. *Stop The Line* is a Lean technique used in manufacturing - it gives every step in the manufacturing process the ability to stop *the entire production line* when there is a problem. This is *desirable* because it forces the production line to smooth out problems in every part of the pipeline *as soon as they occur*. Problems in a pipeline, after all, are multiplicative - they compound on one another. So *Stop The Line* increases the short-term cost of problems in order to ensure they get addressed before the long-term costs show up. Test-driving does the same.

The question "should I test this" makes the question of testing *about the programmer*. Remove them. "Should *anyone* test this?" If it is a desired feature, the answer will always be yes. "Who should test this?" is a question of responsibility - who will be responsible for making sure this works? Where that responsibility lands will determine its expense. "How should I test this?" is a question of *strategy*, and as such, will have variable answers depending on the situation. That said, a *well-phrased* test should be broadly *impervious* to test implementation changes. The mechanisms that a test uses to operate an interface may change, but the spirit and core language of the test will stay the same. Intent must be preserved as details change.

Test-driving is a binding expression of intent. The more successful you are at expressing your intent, the better understood you will be. And understanding is the foundation of collaboration - even self-collaboration.

Now, apply the logic of test-driving to test-driving itself. Wait what? Well, consider this: the process called "test-driving" is a solution. It is a technique that was created to *solve problems*.

So what are the *problems* that suggest the application of test-driving? What are the *tests* that illustrate our intention?

Given a feature-rich system:
  
  - When adding a new feature that is in concord with the existing system, production continues.
  - When adding a new feature that breaks an existing feature, production stops.
  - When adding new features in a team context with concurrent work, production continues quickly.
  - When fixing problems, each fix is treated as a feature, such that if the problem reemerges, production stops.
  - When production has been stopped, the broken version of the system is not shared, including to customers.

The process of test-driving is *a process implementation* of these intended features. Compared to the alternatives, test-driving is both simple and reliable, so long as it is the universal process for a system. To perform these tests (by watching the team handle these scenarios) is to see if test-driving is producing the intended benefit.
  
We recommend *test-driving* because *it lets us focus on new features and refinements* rather than *old ones*. So that we can speed up the integration of independent work to the point where it is *continuous*. We test-drive so we can remove the *technical* cost of integration, and focus on the *conceptual* integration - making sure the team is working toward the same goals, on the right things. And we test-drive so that we have a systemically-reinforced record of intention, so that we can always remember why we made these choices.

Test-driving is expressing intent, then capturing it for the long haul. Express where you'd like to go next. Let's hit the road.
