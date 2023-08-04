---
layout: post
title: A Kotlin Journey
image: assets/images/KotlinGoGoGo.jpeg
tags: 
  - kotlin
  - KMP
  - software development
---

## Discovering Kotlin

The year: 2014. Young Rob (that's me) was nine years into his career, and was frustrated.

I had been working for years in typed languages, and one of the things I enjoyed the most about work was studying code usability. I had access to a lot of early-career engineers, and had the opportunity to watch how they read and write code. I could watch where they would stumble. So I spent my energy, while steadily delivering product value, experimenting with design patterns, techniques, and syntax. I learned a *lot* about what doesn't work, and a little about what does, on the projects available to me, which were mostly in Java and C#.

Today's story isn't about the many ideas attempted and failed (though mayhaps one day I'll write that one). It is about a few particular frustrations that led me down a new road. Places where I felt no amount of design could overcome the limits of the languages themselves.

Those two things were:

- The "one or none" problem (also known as "optionality", "nullability" or "billion dollar mistake")
- the "invisible return" problem (also known as exception throwing-and-sometimes-catching)

So I spent some time researching programming languages: looking for fresh blood that could provide me the kind of type-safeties and syntactical options I so desired.

This led me to deep investigations of a number of languages: notably Swift (which wouldn't hit 1.0 for another year or so), Go (golang), and, eventually, Kotlin.

Today we're going to talk about Kotlin.

I've been using Kotlin on-and-off since 2014. I used it full-time at work in 2017-2018, and then again in 2022. Throughout 2019, I converted ["Coupling"](https://coupling.zegreatrob.com)(the pairing app) from Typescript to Kotlin JS, and also wrote two suites of Kotlin multiplatform libraries: ["testmints"](https://github.com/robertfmurdock/testmints)(libraries to add some syntactical sugar to testing) and ["jsmints"](https://github.com/robertfmurdock/jsmints)(a grab-bag of Kotlin JS libraries). All of these tools continue to be maintained.

All-in-all, you could call it six years of Kotlin experience by a seasoned programmer.

## Kotlin Retrospective

So how does it hold up? Is it worth it? Where's this thing going?

Let's start by examining the two concerns that led me to my research: the "one or none" problem, and the "invisible return" problem.

### One or None

Kotlin explicitly set out to resolve the first problem. Their solution: making "nullability" explicitly enforced by the compiler. Has this solution been satisfying to me? Well... yes!

In other languages, you *could* model "one or none" using a null pointer, but it was entirely unsafe. Why was it unsafe? because there was no way of telling whether a value could be null or not. This resulted in having to lean hard on assumptions about the norms of the code written: what was the team's policy on nulls? Should I always check? Should I never bother because we never use them intentionally? To safely model "one or none", people wrote "optional" types, so you could at least tell that something was really-truly-intended to be "one or none". Interrogating these optionals was always extremely clumsy.

The Kotlin solution eliminated the necessity of a special structure by integrating this "optionality" into the language. They even provided syntax to make interrogating and responding to these "optional" values easy. These are the "?." syntax, which combines a "has a value" check with a method call, and the "?:" operator, AKA the "elvis" operator. The "elvis" will use the value to its left when it exists, and the value to its right when the left does not exist.

`
val optional: Int? = generateValue()
val result: Int = optional ?: 0
`

Having worked to program safely without nulls in Java and C#, adopting safe nullability Kotlin patterns came very easily for me. The problem was *solved*.

### Invisible Return

The "invisible return" problem is rooted in error handling: errors that are return using the "throws" syntax leave no trace, and can make it difficult to recognize programming problems until runtime.

The "Go" language attempts to solve this by dividing errors into two camps: data that can be returned (via the 'error' type), and "panics" (!). In this way, they don't solve the problem by fully eliminating it - after all, sometimes things can fall apart for reasons that have little to do with programming problems - but by changing the semantics of use, and by changing those semantics, hopefully changing the norms. You only 'panic' in a crisis, but more errors aren't a crisis.

Kotlin offers no such natural suggestions to solve this problem. They actually remove from normal syntax the old Java concept of a "checked" exception, which gestured toward the problem without truly solving it.

Indeed, many programming communities don't even consider the "invisible return" issue a problem at all, preferring error states to be runtime and immediate.

So is this a loss for Kotlin, and the programmers who *are* concerned about this issue? Not quite!

Kotlin out-of-the-box does provide one structure for error handling: the kotlin.Result type. This type is a normal object that you can return, but it *acts* as an "either", that will contain either a result value, or a "Throwable" value. So if you want to keep error returns visible, you can use this type.

This type helps in some ways, particularly when dealing with APIs that insist on only communicating errors exceptionally. But it has some real shortcomings. Primarily, its difficult to disentangle different error types and possibilities - your only option is type inspection, which can easily go stale without warning as additional error conditions pop up.

Luckily, this isn't the *only* option for error handling in Kotlin.

Kotlin provides a rich suite of tools for modeling generally. When it comes to options for avoiding "invisible return" problems, of particular use are the ["sealed interface"](https://kotlinlang.org/docs/sealed-classes.html), ["data class"](https://kotlinlang.org/docs/data-classes.html), and ["data object"](https://kotlinlang.org/docs/object-declarations.html#data-objects).

With these, instead of using a "result" class with a Java-style exception, we can define our own return type that is appropriate for the situation. For example, lets say we're build a system that can either succeed, or fail in two different ways (ConnectivityError or ValidationError). We can use a "sealed interface" to declare a "RemoteValidationResult", and declare a data class for the ValidationError that contains information about where the validations failed. We make that ValidationError inherit the RemoteValidationResult interface. We can also write the "ConnectivityError" as a data object that also inherits from the interface. Now, whenever someone receives an object called a "RemoteValidationResult", they use the Kotlin "when" syntax. This syntax uses the "sealed interface" type to smartly require all of the enumerated scenarios are handled.

<iframe src="https://pl.kotl.in/1yY5YY2JY"></iframe>

Using this technique, and perhaps others not yet explored, we can use Kotlin to make error-producing functions much more accurately modeled, and, most importantly, make the error-possibilities visible to the programmer. Visible in such a way that adding new kind of errors that are unhandled will be noticed by the compiler!

This isn't a complete and total solve, but having such an expressive syntax allows teams to have much safer conventions. In this degree, Kotlin competes well with other languages on this concern. And maybe more importantly, its *fun* to use well. And anything that encourages time-strapped engineers to be increasingly clear about errors is quite useful.

### Interlude

Given those two problems, Kotlin entirely solves one, and makes it easy for clever engineers to solve the second. Cool! That's a good start. But programming languages do not live and die on syntax... they live and die on utility.

We'll unpack that for a second. Utility for a programming language is:

- Can I use it to do the tasks I need to do?
- Does it make the tasks easier or harder compared to other languages?
- How expensive is it to hire or train new programmers on the language?
- Do people have fun using it, or is it a slog?

Lets explore these.

### Capabilities

I have successfully used Kotlin for:

- Implementing and consuming REST on Spring-boot services (PCF)
- Implementing and consuming REST and GraphQL on Ktor services (GCP)
- Implementing and consuming Websocket and REST on Micronaut services (GCP)
- Implementing REST, GraphQL, and websocket-interface on an ExpressJs service (deployed to AWS lambdas)
- Implementing an android SDK for interfacing with a REST service.
- Implementing a multiplatform SDK for REST services (browser JS, node.js, JVM)
- Implementing a React JS user interface
- Writing Kotlin multiplatform testing libraries
- Writing Gradle plugins
- Writing Kotlin compiler plugins (KSP)

Hoo boy! There's a lot to say about each. Lets break them into groups.

### Kotlin for JVM Services

One of Kotlin's big advantages in adoption is that it has access to the full library of Java tooling. Kotlin was born out of frustrations with Java, and thus is ideally suited for replacing Java in place.

On my first big project with Kotlin, we slowly rewrote the Spring services to prefer Kotlin over Java. It was relatively easy, and most of the sticking points involved Java code that handled null pointers sloppily. That stuff had to be reworked. But the team didn't feel much regret about that - those were gnarly bits of Java code that needed attention anyway, Kotlin or no.

Spring support and compatibility with Kotlin was in its infancy, but there were few things we couldn't figure out how to make work. Generally speaking, the team was very happy to make the switch, and it was easy to onboard new engineers that were willing to get their hands dirty.

Ktor and Kotlin were born for each other. That is to say, both are developed in-house by JetBrains. If you're familiar with some of the server micro-frameworks that have become popular over the years, you'll find Ktor easy to figure out.

Ktor lets you use most Java libraries, but, naturally, will not let you have access to the Spring ecosystem. For some, this may be an advantage. For others, the proposition killer.

GraphQL with Kotlin required a lot more love, as we couldn't use the provided packages out-of-the-box - if memory serves, this was because we wanted to use multiplatform Json serialization provided by kotlin-serialization instead of the cooked-in solutions. After the requisite scaffolding was put together, it was very easy to get working the way we hoped. I wrote most of that scaffolding myself, and Kotlin certainly made it easier to close gaps.

Either way, getting a Ktor server to work-as-expected is very seamless. Most of the problems experienced with the interface have been actively revised by the Ktor-team as well, so the things that confused us will probably be different than a new team adopting it today.

Kotlin with Micronaut had a lot of promise, but never lived up to it. I wouldn't say anything in particular was *difficult* with using the framework. Nothing was difficult except for the things the micronaut features we simply couldn't enable how we had hoped. Example: we were considering running the Micronaut server on GraalVM. We were never able to get it working with our feature-set, and the amount of energy we spent trying was substantial.

All in all, Kotlin works very well for implementing servers. The refactoring tools provided by IntelliJ make it very easy to maintain some software layering on the server as well: we kept our Json serialization models separate from domain models, separate from persistence models: a common pattern for maximizing safety when reworking the core domain. Creating and updating core domain models and functions in Kotlin is a joy.

### Kotlin for Node.js Services

I'm one of the few people in the world using Kotlin JS as a server-side solution. I came into it experimentally - I wanted to see if I could replace Typescript with Kotlin in an existing app.

Turns out, I could. And not only that, it works very nicely. With some caveats.

I had an express.js server, where most of the related code was in Typescript. It was bundled up by webpack, into a compact little deployable server. I figured, hey, webpack is a powerful system. As long as I can get Kotlin to generate a JS file, I should be able to include it seamlessly in the webpack bundle. After all, webpack can handle mixing JS, CSS, Typescript, coffeescript, and all sorts of things.

I did the rewrite in my free time, and relatively consistently made progress. The most challenging part of using Kotlin JS generally is working with JS or Typescript libraries. Ideally, you can find Kotlin types for the library that someone else has maintained (I'm maintaining a few myself over at [jsmints](https://www.github.com/robertfmurdock/jsmints)). If no trustworthy stubs exist, the you'll have to write your own.

Now, my attitude toward writing Kotlin stubs was forged in my time writing Typescript stubs: you're going to be reading the JS usage instructions anyway, so you may as well capture what you learn in a safe type. It's not very hard, so just get on with it. This will particularly annoy people who don't have any scars from wrestling with Typescript tigers. For those of us who do however... it's not so bad!

All things said, maintaining an express.js server in Kotlin is easy. After it was moved to Kotlin, I even changed the deployment style from a docker container into deploying a series of AWS lambdas, including the websocket implementation. During this transition, Kotlin was decidedly not a problem - we had other challenges to worry about!

### Kotlin for SDKs

I've written a few SDKs with Kotlin. Generally speaking, *writing* the SDKs isn't so bad. All you have to do is have some kind of network transport solution, and then your biggest challenge is designing your SDK for programmatic use.

Of these challenges, the hardest ones are when the consumer of your SDK *might not* be using Kotlin themselves. I say this because when I worked on an Android SDK, the Android universe was still young, and those teams were using Java as their primary language.

In these situations, you'll have access to the fullness of Kotlin's semantic power, but when you consume it from the other language's perspective, it might not seem as *nice* as you'd like. So you're best off testing its utilization in that native language, so you can better see the problems you consumers will run into.

Building an SDK that can run in node.js, and in browser js, and also on the JVM in pure Kotlin is shockingly easy now. If you use Ktor-client as the base, it transports wonderfully. Naturally, the same concerns apply - if your primary consumers are going to be in Javascript or Java, make sure that you design the interface to make it easy *for them*.

I have not built any native SDKs, though I have some desire to see how that would work in the Apple Swift universe. I expect the same rules would apply.

### Kotlin for React SPAs

Blessed be Jetbrains, for they maintain *so many wrappers* for building user interfaces in React with Kotlin. Their team is small but mighty.

Kotlin is now my preferred language for programming React SPAs. I am able to safely refactor more code than I would ever attempt in a Typescript app. The strict syntax keeps me on the straight and narrow instead of trying to construct a perfect Typescript type for my situation. And, above all, it allows me to use the same domain structures in my react code as I use on the server for domain logic.

But that said, for existing React developers, it might be a tough sell. The Kotlin React community is minuscule compared to the general React community, so you have to get good at reading Javascript and writing Kotlin.

Just like with node.js programming, you will need to write your own wrappers for anything the Jetbrains team hasn't provided. They've provided a lot! But not enough.

One sticking point for a lot of people will probably be having access to testing tools. I've spent a goodly amount of free energy making sure React Testing Library has appropriate stubs for people (again, see [jsmints](https://www.github.com/robertfmurdock/jsmints)).

And I learned how to set-up react component test suites using node.js and jsdom with react-testing-library instead of using Chrome with Karma. This is more analogous to what the JS community is accustomed to than the tools Kotlin JS provides out-of-the-box.

I even ported my end-to-end tests to Kotlin. This involved writing a wrapper for "wdio.js". Over time this wrapper evolved and added "wdio-testing-library", and then later added a gradle plugin to make configuration easier.

Sadly, I've never tried using Kotlin with [Cypress](https://www.cypress.io/). I'd love to hear from anyone who has!

Another concern: Kotlin-based JS apps currently have a bit of bloat in their file sizes. My app, which includes a number of libraries over CDN, still produces a web bundle that sums to 1.48MiB. I've split that up into six js files which compress to [brotli](https://en.wikipedia.org/wiki/Brotli) remarkably well. Whether this is acceptable to you or not depends on your solution's requirements.

Lastly, development with Kotlin-react historically has had some speed issues. Recently, the new Kotlin IR compiler with incremental compilation has made it about-as-fast-as Typescript compilation to my real-world experience, but there have been some kinks in that process. Hint - using the Gradle command `./gradlew jsBrowserRun --continuous` for best effect here.

### Kotlin for Libraries (multi-platform)

In 2019 I started a project called [testmints](https://www.zegreatrob.com/2023/05/31/testmints-retro.html). I've spilled ink about it a few times before, but I used that project as my first serious play in the open-source library development community.

These libraries were written for Kotlin consumers - they were decidedly *not* intended to be consumed by Java or any other JVM language. This substantially reduced my design & visibility burden. The other requirement? It had to run on Kotlin JS as well.

Since the libraries were originally designed *within* Coupling, the functionality was never in question. What I had to learn? I needed to learn how to distribute signed library Jars safely, within the Java ecosystem, that also had to have all the correct Gradle metadata included.

This was tricky, but not insurmountable. Blessedly, I had access to another Kotlin multiplatform library's source-code - the [korlibs](https://docs.korge.org/) project (which became KorGE). Their examples for multiplatform plugin publishing were very useful.

So I ended up deciding to publish `testmints` on as many platforms as possible, so that anyone could try the library out, regardless of what platform they happened to be developing on. At the time, there was a real possibility I'd be doing more native programming for embedded devices, so I extra made sure that a suite of native platforms were supported. Linux, Mac, iOS, and Windows binaries produced and distributed.

This meant I had to learn the quirks of Kotlin coroutine development on native platforms... at least, enough to ensure that the "async" testmints library worked-as-intended.

The verdict? For the most part, after the minor initial investment, multiplatform development on Kotlin is *easy*. Data and functions all work just as you'd expect. The challenging part - I imagine - is the bridging between the native UI and the kotlin domain, but as a library author, I didn't have to deal with that.

Its been a good experience! Of course, I have no idea how to promote the library, but that has little to do with the language, so we'll move on.

### Kotlin for Gradle

Almost as soon as it became available, I seized... seized(!) the opportunity to replace Groovy Gradle scripts with Kotlin ones.

I've never liked Groovy. I suspect the root reason is that it is very difficult to understand what it is going to do at any given moment - it has a wild amount of syntax, and - especially in a gradle context - you have to just *know* how it works in order to use it correctly. Yes, its very flexible, and has a lot of neat features... but I tend to value clear readability over features in my programming languages.

But Kotlin! The miracle of having accurate code-completion in Gradle works *wonders* on helping regular-old-developers understand how the system works. Gradle has a pretty impressive technical underbelly, but without the Kotlin DSL I suspect it'd still be destined for legacy-enterprise-only applications.

They *do* have the Kotlin-DSL now though, and my hunch is that even internally at Gradle, its increased their development cycle.

I digress.

I've always needed sufficient control over my builds to achieve the dream of a *one button-build* in increasingly complex modern applications, and the Kotlin-DSL made that achievable without friction in JVM languages.  

After collecting all the achievements for regular build-scripting, I started moving into learning Gradle plugin development. Like most people, I started by developing "convention" plugins that allowed consolidating repeated build logic. I was luckily enough to get interested in this at the time that the Kotlin DSL was ready to generate 90% of the tricky JVM constructs for me (just add one *.build.kts file in your src directory, add the `kotlin-dsl`
 and `java-gradle-plugin` plugins to the project, and it handles the rest).

Yeah, there were some headaches (frequently regarding correctly remembering package names). But convention plugins were just the first step.

My build requirements have always been pretty large, and getting larger. I ended up porting a shell script WDIO runner system into a gradle plugin, complete with its own configuration. Shell scripts are relatively easy to make into plugins - the challenge is learning the Gradle rules for custom tasks and "extensions" (it still relies heavily on "open" structures, which is unfortunate as a norm).

I've now written plugins that do the following things:

- enforce use of a "dependency-bom"
- enforce use of a particular javascript dependency constraint
- register a "wdioRun" task, which will run tests in a WDIO context
- register all dependencies listed in a package.json
- register "lifecycle logging" for all testmints-stages on JS and JVM
- configure sonatype deployment
- configure test outputs into a common directory for easy upload to github actions
- Generate utility source code for "actions" - data structures associated with a function
- Generate utility source code for "react" functions
- automatically install and run "npm-check-updates" for updating a package.json
- automatically generate a version number based on git tagging, including publishing a github release, and pushing a new tag during a release

As you can see, these run the gamut. I have a few ideas for more as well, which may or may not cook out over the next few years.

Suffice to say, writing build plugins in Kotlin is great, and *absurdly* easier compared to with Java / Groovy.

## Kotlin Compiler Plugins

You probably saw a few Gradle plugins there that generated source code: these are Gradle plugins that automatically apply [Kotlin Symbol Processing](https://github.com/google/ksp) with a given processor.

I'm still in the process of learning KSP, but it seems very powerful, though still has some quirks as its in early stages.

The core idea is that KSP runs on source code *before* it does a true regular compile, so you can generate additional code that will be included in Kotlin compile run. Generated source code is plainly available in a directory, so you can see how it all works.

By design, this is not a system that will let you *rewrite* a class: the code a developer writes in Kotlin will always do exactly what it claims. But it *can* generate other content related to any given type.

This is *perfect* for things like auto-mappers.

So far I've successfully used it to cut down on React boilerplate, and for shaving off some of the cumbersome edges of my "action" library (for modeling user actions in reusable structures).

The KSP 'model' of Kotlin source code is highly detailed, but relatively intuitive. Tools like [KotlinPoet](https://square.github.io/kotlinpoet/) make generating code shockingly easy.

For the places where default Kotlin syntax doesn't *quite* do what you need it to do, KSP seems to fill the gaps nicely, with fewer of the shortcomings that its predecessors have had.

## Kotlin Additional Feedback

There are a few other pro's and con's of Kotlin I haven't covered yet. In case it hasn't already come across, the core Kotlin semantics are best-in-class, balancing expressiveness with strict clarity. This makes the language particularly excellent at creating domain structures and functions in a variety of styles. Kotlin provides "sealed classes" and "sealed interfaces", "data classes" and "data objects", a variety of ways to express a lambda function, and a horde of convenience utility functions for manipulating single objects, as well as collection types.

Kotlin *doesn't* have a few language features that I miss, however.

There's no "implicit implementation" a la "Go". There *is* relatively lightweight lambda-to-functional-interface conversion, but I have run into a few situations where "implicit implementation" would have been useful.

While handling of "nulls" is handled with care, there's no way to notice when a function threatens to throw - for example, an index-out-of-bounds exception is very possible when using the "list" structure. There's no Golang-style array-of-fixed-size types. If these were properly supported, you could define a list of size 3 and then make a compile error out of list[9]. Many of the standard library functions include both an exception throwing version and a "safe" value-or-null returning version. This means those functions in particular should be useful only carefully and with confidence... but there's no way to clue in the casual code-reader, short of memorization.

Kotlin "data classes" are great, but it seems like there could come with more functionality. Currently they provide a nice "copy" function, and "destructuring" support. But...

Kotlin "destructuring" is index-based, and this was a mistake. Name-based destructuring would be *vastly* preferable. My current understanding is that the Kotlin devs would like to add it, but have other priorities, and so there's no ETA at all. Disappointing.

And, like most statically typed languages, serialization still has friction. The Kotlin-serialization project makes a valiant effort to make data-to-Kotlin objects safer, but there are still a number of sharp edges that only reveal themselves through testing.  

On the positive side again, the Kotlin release schedule has been steady. They're still doing big release drops, but you tend to see substantial releases at least once a quarter. Plenty to keep anyone who's pushing the cutting edge of their feature-set busy.

## Kotlin Testing

That's actually not much to say about Kotlin and testing. Like any programming language, if you want to test, or test-drive, you'll come up with some great results. Kotlin *does* provide a multiplatform minimal "kotlin-test" library to ensure that you're always have *something*, no matter which platform you're targeting.

Kotlin brings up some semantic possibilities in tests that I've tried to explore with [testmints](https://github.com/robertfmurdock/testmints). Libraries like [kotest](https://kotest.io/) bring other styles of test-organization.

Ultimately, as with all things testing, you end up getting out of it what you put into it. I've never run into a language that I've found "un-testable". *Frameworks* on the other hand...

## Recommendations

Phew! That was a lot. 

All-in-all, Kotlin is in a good state. It combines the flexibility of cutting-edge modern languages, with strong type-safety, on platforms that have vast library support. Its perfect for safely scaffolding a new suite of functionality, as well as maximizing safety for legacy code with the strong compiler.

If you're managing a legacy Java project, I'd recommend you transition to Kotlin. First, you'll probably find (and hopefully fix!) a lot of bugs while going through the transition process; a process made infinitely easier by using the "Java to Kotlin" convert function that IntelliJ provides. Secondly, it'll give your development process an ongoing sugar-rush, as your engineers find the way they'd like to use the new language. Finally, you'll benefit from the modern language syntax in all future work.

If you're a start-up with limited resources, Kotlin multiplatform development is a good deal - better today then the last few years, and it continues to improve. Writing domain structures and functions once, then using them on all of your platforms is *very potent* and eliminates a lot of friction. The more platforms you have to support, the more value you'll get.

If you're working with Javascript, you'll want to balance two concerns: your desire to work in Kotlin, and your desire to make a "conventional" Javascript code base. If you're looking to scale up and hire engineers that are only interested in working in Javascript, then you should be cautious about adopting Kotlin. But! As my experience demonstrates, Kotlin outputs conventional Javascript files that can be easily integrated into a multi-language Javascript application, using webpack or any other bundler. If you commit to Kotlin JS and decide to reverse that later, you'll find conversion to Typescript to be fairly easy, though manual (provided your Kotlin is well factored). It seems very reasonable and sustainable to be able to write core domain models and an SDK in Kotlin JS, and then share it with a robust Javascript application.

If you're considering writing a new system with C#, Rust, or Go instead of Kotlin, here's what I would ask you to consider.

C# is well maintained, and deeply embedded in a mature ecosystem. If you're already committed to the Microsoft ecosystem, its hard to argue against it. But this is a two-sided sword: Microsoft's strategy has long-been to control every aspect of their development platforms. Once you're in, they want you to never be able to leave.

Go is another very modern language, with a highly targeted syntax. If you're highly concerned with native performance, and need an easy-to-learn language, then you'll be better off with Go than Kotlin (Kotlin-Native makes no claims about optimization at this time).

Rust of course is the language to use if you're concerned about native performance and don't mind a higher learning curve.

However, most people aren't in these situations. When you're trying to automate and improve a business process, low-level performance isn't your core problem: accurate and malleable modeling *is*. And in that category, Kotlin beats all three of these languages, with its focus on flexible modeling.

## Final Word

I've enjoyed my time with Kotlin. I'm glad its gaining steam in the industry, and hope it continues to accelerate in growth. Its brought a good dose of joy to my engineering life, and I hope it can bring it to yours as well.


## Does Anybody Else Agree?

Totally fair question.

Jacob Ras just aggregated articles that places using Kotlin Multiplatform have written about their experience. See: https://medium.com/@jacobras/popular-apps-using-kotlin-multiplatform-kmp-in-2023-and-what-you-can-learn-from-them-1f94d86489b7

The Thoughtworks Radar has had the Kotlin language as ["Adopt"](https://www.thoughtworks.com/radar/languages-and-frameworks/kotlin) since 2018, and hasn't said much since. The Gradle Kotlin DSL is ('Adopt') as well, since April 2023. 

Kotlin Multiplatform (now KMP) has an old entry as ["Trial"](https://www.thoughtworks.com/radar/languages-and-frameworks/kotlin-multiplatform-mobile) from October 2021. I would guess this is going to be revised soon. 


---

Thanks to [Kevin Luo](https://www.linkedin.com/in/kevinluo117/) helping read through this piece and fix some confusing bits.

---

Image generated by Bing Image Creator, from the prompt 'a medium splash image for an article about learning the programming language Kotlin and using it over six years for a wide variety of projects'.

---
