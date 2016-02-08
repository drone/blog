+++
date = "2016-02-08T12:40:00-08:00"
draft = false
title = "Size Matters | The quest for a smaller Docker image"
description = "Why we chose Drone for building Docker images for our Phoenix/Elixir application"

[author]
  name = "Aaron Weiker"
  link = "https://twitter.com/a_weiker"
  photo = "https://pbs.twimg.com/profile_images/695131912311083008/N3NKxAU__400x400.jpg"

[twitter]
  author = "@a_weiker"
+++

Two weeks ago I started a journey to find a better way to have Docker images generated during the build process. It was then that I discovered [Drone](http://github.com/drone/drone). I was searching for a way to compile our [Phoenix](http://phoenixframework.org) application using [Exrm](https://exrm.readme.io/) and then place these binaries onto the Docker image. To sum up, I had the following goals:

* Generated Docker image <100MB
* <5 second startup time

The following is an attempt to walk you through the journey I took to create the [Drone with Elixir Demo](http://github.com/drone-demos/drone-with-elixir). If you are just looking for the details on how to build a Docker image for [Phoenix](http://phoenixframework.org), you can jump straight to the [demo](http://github.com/drone-demos/drone-with-elixir) for the details.

My goal for the [demo](http://github.com/drone-demos/drone-with-elixir) was to find the correct way to run a [Phoenix](http://phoenixframework.org) application in production that is simple, secure, reliable, and fast. I wasn't comfortable with the running `mix phoenix.server` like [`bitwalker/alpine-elixir-phoenix`](https://github.com/bitwalker/alpine-elixir-phoenix) and thought that [`msaraiva/alpine-erlang`](https://github.com/msaraiva/alpine-erlang) was nice, but left you with just part of a solution. So this was my attempt at providing a more complete example.

I do not include anything on how to deploy a Docker container. I needed to draw the line somewhere and get this written. So this demo will leave you with the container being published and relying on you to do the rest.

While this isn't advisable for production but works for this scenario. Drone is [currently setup](http://beta.drone.io/drone-demos/drone-with-elixir) on the demo and will create a new container whenever a new build passes. Then I have [Watchtower](https://github.com/CenturyLinkLabs/watchtower) setup to keep http://drone-demo.weiker.org/ constantly up to date.

## [Keep it Simple] (https://en.wikipedia.org/wiki/KISS_principle)

The more I looked into how Drone worked, the more I was drawn to the simple elegance of it. Everything in Drone is orchestrated together using Docker containers. From fetching the code, compiling, even publishing was all based on isolated, stateless containers. By using an approach like this, we can get these guarantees:

* Only the source code varies
* Builds are isolated from one another
* Updating dependencies is kept simple

As an example, during the process of evaluating Drone there was a [new version of Elixir](https://github.com/elixir-lang/elixir/releases/tag/v1.2.2) released. The evaluation and migration to the new version was as simple as updating the base image.

While other build systems also try to solve this by allowing you to declare your dependencies and build environment, the difference with Drone is how those dependencies are built and made available to me. Drone allows me to specify the image to use, and in doing so, I can be confident that the feature set I need is made available.

![Build Explained](/images/drone-with-elixir_build-explained.png)

So far I have just been discussing the _Fetch_, _Compile_, and _Test_ stages of the build process. Where the real power of Drone came into play for me was in the _Publish_ stage where I can take the output of the _Compile_ and create a Docker image.

## Less is More

At this point you may be wondering:

> _"Why can't I use the same base image for compiling and creating the production container?"_

Let's be clear, _you can_. ~~If~~ When you [read the documentation](http://www.phoenixframework.org/docs/deployment#section-starting-your-server-in-production) on how to run Phoenix in production, it will tell you to use `MIX_ENV=prod mix phoenix.server`. In fact, this is actually how the popular [`bitwalker/alpine-elixir-phoenix`](https://github.com/bitwalker/alpine-elixir-phoenix) image is built which runs at 272.6 MB. But remember about my goal of having a small container (< 100MB), if I want to accomplish that I need to be ~~obsessive~~ intentional about what is added.

To reach this goal, I went searching for another way. This is when I came across [Exrm](http://www.phoenixframework.org/docs/advanced-deployment) and [these instructions](http://www.phoenixframework.org/docs/deployment#section-compiling-your-application-assets). This allows me to use a base image that only has Erlang on it and copy the compiled BEAM bytecode to the container.

To give you a sense at how small of image we can start with and what can happen if you aren't paying attention, refer to the output when I ran `docker images` on my server. I ordered sizes starting the smallest to the largest.

```
REPOSITORY                       TAG       IMAGE ID       CREATED       SIZE
alpine                           3.3       14f89d0e6257   2 weeks ago   4.794 MB
gliderlabs/alpine                3.1       2cf6c9a8c8ea   3 weeks ago   5.04 MB
plugins/drone-cache              latest    a6bdd45ef09f   12 days ago   10.08 MB
drone/drone-exec                 latest    2064050fea4f   10 days ago   15.86 MB
drone/drone                      0.4       02ca0e3f9578   42 hours ago  21.57 MB
aweiker/alpine-elixir            1.2.1     6fc85632076b   3 days ago    40.97 MB
plugins/drone-docker             latest    cd14b7633550   12 days ago   44.71 MB
dronedemos/drone-with-elixir     latest    5f20e9ee9299   2 days ago    54.32 MB
plugins/drone-git                latest    96b7bec4e003   5 days ago    69.72 MB
nginx                            latest    99e9abbaeceb   11 days ago   134.5 MB
bitwalker/alpine-elixir-phoenix  2.0       be6ba4879714   3 weeks ago   272.6 MB
```

As you can see from this list, the _production_ image `dronedemos/drone-with-elixir` is only 54.32 MB. This is significantly smaller that the image that we used to compile our code which is the `bitwalker/alpine-elixir-phoenix:2.0`. Still way larger than what we started with of 4.794 MB, but we have the power of Erlang at our fingertips now.

## Creation

At this point you might be wondering how the compiled output is moved from one container to the next. (At least I was.) In order to get the source code on this image, Drone will side mount the Drone workspace on the container.

> Drone is designed around a [plugin](http://readme.drone.io/devs/plugins/) architecture. For example, the source code itself is fetched using the [`plugins/drone-git`](http://readme.drone.io/plugins/git/) container. The following visualization may help you visualize how this works.

![Drone Workspaces](/images/drone-with-elixir_containers.png)

## Parting Thoughts

If you are interested in more details on how to run a [Phoenix](http://phoenixframework.org) application in Docker or just how Drone works, you should explore the [Drone with Elixir Demo](http://github.com/drone-demos/drone-with-elixir). One of the reasons I put it together was because there wasn't a complete end-to-end example connecting all of these pieces.

I would also love to hear from you if you have questions, suggestions, or any other type of feedback.

## About Me
I (Aaron Weiker) am a Principle Developer at [Concur](https://www.concur.com/) where I write software using [Elixir](http://elixir-lang.org/). Currently I am involved with our [GraphQL](https://facebook.github.io/graphql/) project and helping to build out an [open source implementation in Elixir](http://graphql-elixir.org/).
