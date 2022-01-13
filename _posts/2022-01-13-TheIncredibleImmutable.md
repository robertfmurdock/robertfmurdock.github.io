---
layout: post
title: The Incredible Immutable
tags: immutable web apps immutablewebapps webpack static config environment
---

Over the last year and a half or so, my coworkers and technical friends have probably grown tired of hearing me talk about "Immutable Web Apps". "Sounds nice!" they say, politely suppressing their confusion and disinterest.

Despite a very compelling writeup at [https://immutablewebapps.org/](https://immutablewebapps.org/), I find people have a lot of difficulty getting traction with the idea. For some, it's difficult to envision the benefits - "Rob seems really excited about it, but I really don't know how it would make my life better". For others, making the change to such a system seems out of reach - "that sounds like something I'd have to fight with my ops/devops/architecture team about, and I just don't have the energy for that". And for all these groups, actually *doing* it seems like something they wouldn't be comfortable owning - "I'm really not sure how to *do* this, and it seems really hard!".

I feel that.

As such, I want to walk you through my own path converting one of my personal projects to the immutable style.

## The Beginning

Starting point: my web app consisted of two components.

1. A client - a react app, running in a web browser, downloaded from a URL.
2. An api service - a programmatic interface, available by HTTP, that runs code on request.

These two things were both copied into a Docker image, and when the Docker container starts, the API service would "serve" the client at one of its paths. All-in-one, badda-bing, badda-boom-goes-the-dynamite.

Now, a typical React or angular application these days will put "configuration" information in at build time. For example, if I needed to support three different environments, and my client needed access to an identifier (such as an Auth0 Client ID), that means I would have to build my client application THREE TIMES. And what's more, if someone needed a new environment for any reason, I'd have to build it a FOURTH time!

It gets worse though - because my client was bundled with my server, that means I'd need to have THREE DIFFERENT DOCKER IMAGES. A different docker image per environment.

This is where warning bells may be going off in your brain. Let's put some familiar words to the intuition those bells are dinging about - "Build once, deploy anywhere". One of the biggest virtues of using a system like Docker is that you should be able to add new environments by simply spinning up containers from the image, right? But if you have to build an image per-environment... well. There's a lot of value being left on the table, our source-code-to-deployment pipeline gets slow, and there's suddenly zero distinction between a "build" step, and "deploy to specific" steps. *Even worse*, everytime we need to change one of these configuration settings, we'll have to rebuild the software. This can be a real problem if one of your credentials gets compromised and you need to switch to a different one, quickly!

And thus, we have stipulated that this situation isn't great.

So! The first step - lets get *ALL* of the configuration that the client needs to operate out of there.

And this is where most engineers find themselves stumped. "So, if it can't come from the build process... where is it going to come from? When a user hits my system, they just download the index.html that loads the app, and they're off to the races... that seems like a pretty closed system, honestly."

This is a pretty reasonable place to get stuck, honestly. But it already holds the kernel of the answer - the *entrypoint* for the client application is that "index.html" file (or other homepage equivalent). Therefore, all configuration that the client application needs *must* ultimately come from the index.html.

And this means, the index.html can't be part of the build process. Or at least... not the version of the file the browser will ultimately download. More on that in a moment.

Immutablewebapps.org states clearly "index.html is deployable configuration" [source](https://immutablewebapps.org/#indexhtml-is-deployable-configuration). It is *not* static, it is *not* part of the build, and they recommend treating it as a deployment manifest, versioning it wherever configurations are sold near you. They also declare that it must *never* be cached by a browser! Why? Well, because the configuration needs to be able to change on a dime. "To allow for web application environments to be changed instantly, index.html must never be cached by the browser or a public cache that cannot be purged on-demand"([source](https://immutablewebapps.org/#indexhtml-must-never-be-cached)).

When I read that myself, I started making this face:

<img src="https://media.giphy.com/media/2zoFXIZs5F6tSPs8fB/giphy.gif" alt="umm-yeah-face" />

I had an issue - my webpack build, which produces my client application, had a *robust* index.html generator. It attaches all sorts of links, source files, subdivides CSS files, and transforms images to make versions appropriate for all kinds of devices. Some of that I suppose I could move into being loaded later via the client app itself... but then downloading of those assets would only be able to start *after* the client downloaded, and that *stinks*. Browsers are super good at concurrent downloading of static assets! It would be a real shame to give that up, and I really-really-really-really-really didn't wanna.

But I didn't give up, and I decided to embrace the philosophy that motivates me whenever presented with a choice of dessert:

<img src="https://media.giphy.com/media/3ohzdMDbNXvnWdeOZi/giphy.gif" alt="why not both" />

I like cake, I like possessing it, I like eating it, and darnit, I will FIND A WAY.

So here's what I did.

## So, our new business is "index.html... *as a service*."

My solution is simple: since I needed index.html to be configurable, and I also needed it to be produced during my build, I... as should be very obvious now... *did both*.

But let's get more specific and literal. My build continued to produce a cool dynamically generated index.html based on the needs of my webpack resolutions. But it *didn't* include any configuration information. At all.

The index.html that was being built is *not* the index.html that users receive. Or at least, not exactly.

The URL given to users to download that file *did not go* to a static file on a server. Instead, it ran a function on my API. This function, in turn, read the index.html from the client build (as at this point in time, it was just a file sitting in the docker image), and added a new tag that looks like this:

```
<script>
    window.auth0ClientId = "client-safe-client-id";
    window.auth0Domain = "client-safe-auth-domain";
    window.basename = "thing";
    window.expressEnv = "production";
    window.websocketHost = "socket.mycoolsite.com/";
</script>
```

That tag is inserted as the very first element of the <head> tag - so it runs before anything else on the page loads.

So *really* what's happening is that there are *two* index.htmls. One is the prototype - an index.html that is truly part of the client, and the other - the index.html that is burdened with glorious *configuration*. But *really*, this latter index.html is *forwarding* configuration information from another source - the API service.

This style of injection is actually as old as the web itself, of course... back when client apps were just a few lines of javascript held together by string and hope, servers were presenting them with contextual data via the initial download. But by doing this here, it opened up the door for the next move...

## Set that Client Free

Immutablewebapps.org talks a *lot* about caching that client. Personally, I boil it down to this: think of your client application as more akin to a library, rather than a live service. There are a lot of advantages to this, but principal is that this "immutable client" can be downloaded from the same location for *all environments*. [source](https://immutablewebapps.org/#static-assets-must-be-hosted-at-locations-that-are-unique-and-independent-of-the-web-application-environments)

But... my app didn't do that. My client was still bundled inside my docker image, and presented as static assets on whichever host that container was running on (lots of hostless "/here-is-my-resource.jpg" paths).

So now, if I wanted to take advantage of these features, I had to make my client come from a full different server. And it had to deal with all the quirks that come with that sort of thing. That means CORS, baby.

But not just that - my client app had a lot of static assets besides pure javascript. Images, Css files, markdown, and even graphql scripts. The client app needed to be able to determine the correct URL to find these assets at, but that URL wouldn't even *exist* until the build was complete (and a new version number was provisioned).

Suddenly my client app needed to be a little more configurable than it had been before, and I needed to quick-do-research and level up before this client could move out of its parent's basement.

## Enter the `__webpack_public_path__`...

Most of these self-references were being generated by webpack, and so were somewhat unpredictable. It was extremely difficult to figure out how to solve this problem, until I discovered and ran a few experiments based on [this documentation](https://webpack.js.org/guides/public-path/#on-the-fly).

The quick version is that the `__webpack_public_path__` variable lets you *at runtime* set the root path for all webpacked assets. That is to say, your client will use that variable to resolve all client assets.

This is huge. Suddenly, I could add a new configuration property:

`window.webpackPublicPath = https://assets.mycoolsite.com/1.0.69/`

And added a "preflight" javascript file that forwards that information before my main webpack assets load:

#### App.js

```
if (window.webpackPublicPath)
    __webpack_public_path__ = window.webpackPublicPath;
```

#### webpack.config.js
This makes sure the app.js is run before anything else.

`config.entry.main = [path.resolve(resourcesPath, "app.js")].concat(config.entry.main);`

And like magic, at configuration time, I could tell the client where to download more of itself from.

As the goal now was to be able to deploy this client to a static file location (an S3 bucket), I also had to configure my server to *find* the index.html at the new location. So I introduced a new server config variable: "CLIENT_URL".

In theory, all I had to do was set the CLIENT_URL to the location that the client was hosted, and alakazham! it would slurp up the index.html file, send it down through the browser, and everything would be hooked right up.

Naturally, this thing fell apart on the first try.

Remember earlier when I said that webpack added a lot of dom elements to the index.html? As one probably could have predicted, all of those fields were *not affected* by the `__webpack_public_path` setting. Why not? Well, the javascript hadn't even downloaded yet! And the thing simply was never designed to rewrite existing dom elements to conform to its standards.

There was one piece missing to this puzzle, and that was adding a little more magic to my index.html function: I had to, at runtime, fix the URLs for static assets... including the javascript application reference.

Truly, this wasn't very difficult. I configured webpack so it produced a string token I could replace, and then scanned the index.html content for that token, and instead substituted the "CLIENT_URL".

#### webpack.config.js

`config.output.publicPath = '/app/build/'`

#### IndexRoute.kt
Server code in kotlin, because reasons.

```
private fun rewriteLinksToStaticResources(tag: Tag) {
    tag.attrs = tag.attrs.map { attribute ->
        attribute.apply {
            this.value = this.value.replace("/app/build", Config.clientUrl)
        }
    }.toTypedArray()
}
```

And then, all of a sudden... it kind of just worked!

## But... how do you know?

I uploaded a few versions of the client app to different URLs so I could test that my server picked them up correctly. Because everything was now handled via configuration, I was able to set my local development server to target either of them.

I exported the appropriate CLIENT_URL and... boom, there's the old version. Switched it to the newer version? Hey! I see the difference.

I could see the possibilites unfolding before my eyes - now, I didn't have to have a local build of my client *at all* in order to confirm that changes to my server were safe. If I got reports about weirdness, and I thought it might be client related, I could connect my local server to that build and *see*, without fear of screwing up the production data (defended by the production API).

As a great man once said,

<img src="https://us.v-cdn.net/6029252/uploads/editor/fg/sg7m70jtecaq.gif" alt="'I'm Really Feeling it' - Shulk">

## Wrapping it up

Now that I knew the server could handle it, and the config was forwarded correctly, I just had to fold it into my build process.

That meant:

- provisioning unique version numbers for each build of the client (since I use a monorepo for my client/server, I use the same version numbers for both, at least for now)
- automating the upload to a directory in S3 based on that version number
- passing the URL of that directory to the server's configuration
- deploy the new server config, so the server will start using the new client

As I believe these are less interesting, I'll pass on describing them for now.

Also, the server and host of my static assets needed to have CORS configured correctly. Not super complicated? But can be a source of frustration if you run into it unexpectedly. Better writers than I have addressed that topic in great detail though, so I'll leave you to fend for yourself on that one.

## Is it all its cracked up to be?

Well, at time of writing, I decided I wanted a new environment yesterday - a sandbox environment, so I could test the latest version of my app without affecting production data at all.

All I had to do was deploy my server again with new configuration (basically pointing at a different database, and having different hostnames). No changes were made to the client at all. Incredibly easy, incredibly fast, and I can imagine adding 10 more "environments" with ease.

Add on to that the less-obvious-but-still-rad features provided by a service like cloudfront, and I'm very happy with my choice. And choosing to do this setup is *way harder* when going in blind. Now that I know how to solve these problems, it will be easy to do it again on all my future projects.

## Ktnxbye

I hope that was an interesting read for any interested, and if you're going on this journey yourself, please let me know if this write-up has been helpful!

- Rob
