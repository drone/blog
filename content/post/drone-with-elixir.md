+++
date = "2016-02-08T12:40:00-08:00"
draft = false
title = "Size Matters | The quest for a smaller Docker image"
description = "Why we chose Drone for building docker images for our Phoenix/Elixir application"

[author]
  name = "Aaron Weiker"
  link = "https://twitter.com/a_weiker"
  photo = "https://pbs.twimg.com/profile_images/695131912311083008/N3NKxAU__400x400.jpg"

[twitter]
  author = "@a_weiker"
+++

Two weeks ago I discovered Drone while I was in search of a better way of creating the Docker image that would run our application. I was on the lookout for a way to compile our Elixir/Phoenix application and then place only these binaries onto the docker image. To sum up, I had the following goals:

* Generated Docker image < 100MB, was 250MB
* Fast startup time of image < 5 seconds

The following is an attempt to walk you through the journey I took to create the [Drone with Elixir Demo](http://github.com/drone-demos/drone-with-elixir). If you are just looking for the details on how to build a Docker image for [Phoenix](http://phoenixframework.org), you can just straight to the [demo](http://github.com/drone-demos/drone-with-elixir) for the details.

## [Keep it Simple, Stupid] (https://en.wikipedia.org/wiki/KISS_principle)

The more I looked into how Drone worked, the more I was drawn to the simple elegance of it. The ability to pick the exact Docker container that will run my build. With the introduction of this feature it took care of all of the following concerns:

* Only the source code varies
* Builds are isolated from one another
* Updating dependencies is kept simple

As an example, during the process of evaluating Drone there was a new version Elixir released. The evaluation of migration to this was as simple as updating the base image that was used. I have to admit that other build systems also try to solve this by allowing you to declare your dependencies and build environment. The difference with Drone is that I can be 100% confident how those dependencies are built and made available to me. In this case of Elixir, being built on top of Erlang and OTP, I can be confident that the feature set I need is made available. In fact, this is the exact [Dockerfile](https://github.com/aweiker/alpine-elixir/blob/master/1.2.1/Dockerfile) that I ended up using and you can see exactly what Erlang packages are installed.

![Build Explained](/images/drone-with-elixir_build-explained.png)

So far I have just been explaining how the _Fetch_, _Compile_, and _Test_ stages work of the build process. Where the real power and flexibility of Drone came into play for me was how I can take the artifacts created during the _Compile_ stage and package them up into a Docker image. Since we already established I can use a custom Docker image for compiling and testing, let's address the publish stage where I can generate a custom docker image. Since Elixir is a compiled language I need to be sure that I compile and test my code on the same platform/options that it will be deployed on. In my case, I have an Alpine Linux image that was setup to use for compiling the code and then a separate base image that is used for building the Docker image.

## Less is More

> _"But didn't you just say you wanted to use the same image in both places?"_

I ~~lied~~ stretched the truth. You see, in order to build Phoenix application for release, you need to [use Node to prepare static resources](http://www.phoenixframework.org/docs/deployment#section-compiling-your-application-assets) and then use [Exrm](http://www.phoenixframework.org/docs/advanced-deployment) to generate the compiled BEAM bytecode.

Remember how I said one of my goals was to create a small container. If I had gone the traditional route of running my application with `mix phoenix.server` I would have needed a larger container such as [bitwalker/alpine-elixir-phoenix](https://github.com/bitwalker/alpine-elixir-phoenix) which starts at 272.6 MB. For reference, here are the sizes of the docker images on my machine when running `docker images`.

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

As you can see from this list, the production image `dronedemos/drone-with-elixir` is only 54.32 MB. This is significantly smaller that the image that was required to compile our code.

If you remember, when Drone does the build it will do so in whatever container image is specified. In order to get the source code on this image, what Drone does is side mount the source code into the container. The source code itself is actually fetched using the [`plugins/drone-git`](http://readme.drone.io/plugins/git/) container. For more information on how this works and how the entire plugin system work, refer to the [documentation](http://readme.drone.io/devs/plugins/). The following visualization may help you understand how this works.
![Drone Workspaces](/images/drone-with-elixir_containers.png)

## Parting Thoughts

If you are interested in more details on how to run a [Phoenix](http://phoenixframework.org) application in Docker or just how Drone works, you should explore the [Drone with Elixir Demo](http://github.com/drone-demos/drone-with-elixir). One of the reasons I put it together was because there wasn't a complete end-to-end example connecting all of these pieces.

I would also love to hear from you if you have questions, suggestions, or any other type of feedback.

## About Me
I (Aaron Weiker) am a Principle Developer at [Concur](https://www.concur.com/) where I write software using [Elixir](http://elixir-lang.org/). Currently I am involved with our [GraphQL](https://facebook.github.io/graphql/) project and helping to build out an [open source implementation in Elixir](http://graphql-elixir.org/).
