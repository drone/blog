+++
date = "2016-01-18T18:00:00-08:00"
draft = false
title = "Drone 0.4: Plugins, Matrix Builds, Compose"

[author]
  name = "Brad Rydzewski"
  link = "https://twitter.com/bradrydzewski"
  photo = "https://pbs.twimg.com/profile_images/3253271781/90284722f819b80f92b5fd93a07ee109_400x400.jpeg"

[twitter]
  author = "@bradrydzewski"
+++

Today I’m excited to present the 0.4 beta release of Drone. This release builds on the work of nearly [160 contributors](https://github.com/drone/drone/graphs/contributors) to bring you the most powerful and exciting version of Drone yet.

## Matrix Builds

This is our most requested feature. [Matrix builds](http://readme.drone.io/usage/matrix/) let you test your project against multiple configurations, for example, the ability to test your codebase against multiple versions of a language, or database, and more. Below is an example build with a matrix configuration:

```
build:
  image: node:$$version
  commands:
    - npm install
    - npm test

matrix:
  version:
    - 4.0.0
    - 0.12
```

Matrix builds are not limited to just language versions. You can test your project against multiple databases, linux distributions, docker images, docker image tags, and more. We’ll be posting a follow-up blog post dedicated to matrix builds.

Learn more about matrix builds in our [documentation](http://readme.drone.io/usage/matrix/).

## Plugins

Drone 0.4 introduces a simple, yet powerful [plugin](http://readme.drone.io/devs/plugins/) implementation that is unique compared to tradition implementations. This is because the container is the plugin.

Plugins are Docker containers that attach to your build at Runtime to publish artifacts, execute deployments, sent notifications and more. Plugins share access to the underlying build volume, but are otherwise completely isolated and run in their own container. They are downloaded automatically from the registry, and even auto-update!

The best part about plugins is that, as Docker containers, they are language agnostic and extremely easy to build. Unlike some systems that should not be named, you aren’t restricted to the Java runtime or any other language. If it runs in a container it can be a plugin.

Learn more about plugins in our [documentation](http://readme.drone.io/devs/plugins/) and see available plugins in the [plugin list](http://readme.drone.io/plugins/).

## Compose Inspired

The `.drone.yml` file has been re-designed to more closely align with the Docker compose file format. The result is a more consistent and familiar file format that exposes more of the underlying Docker configuration options to your build.

```
build:
  image: node
  commands:
    - npm install
    - npm test

compose:
  cache:
    image: redis

  database:
    image: postgres
    environment:
      - PGUSER=postgres
```

For more in-depth details on what is shown in the example above, see our documentation:

* [Build & Test](http://readme.drone.io/usage/build_test/) - How to build and test your application.
* [Service Containers](http://readme.drone.io/usage/services/) - Excellent for running dependencies like caches, databases, or queue servers.

## Machine Integration, Orchestration

Build machines are now managed from the user interface, allowing you to quickly scale up or down as your build volume grows.

To simplify setup, you can now provision and register build machines from the command line using the drone command line utility and `docker machine`.

Learn more about the command line utility in our [documentation](http://readme.drone.io/devs/cli/).

## Enterprise Supports and Tools

In the coming months we’ll begin to offer enterprise support and tooling for companies that want to manage large-scale deployments. If you are interested in piloting our program and tools please contact us at [hello@drone.io](mailto:hello@drone.io).

## Install it Today!

If you'd like to try Drone, see our [installation instructions](http://readme.drone.io/setup/overview/) for a walkthrough. We offer out-of-the-box support for [GitHub](http://readme.drone.io/setup/github/), GitHub Enterprise, [Bitbucket](http://readme.drone.io/setup/bitbucket/), [GitLab](http://readme.drone.io/setup/gitlab/) and [Gogs](http://readme.drone.io/setup/gogs/). 
