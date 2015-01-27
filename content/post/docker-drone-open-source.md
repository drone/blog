+++
date = "2014-02-05T08:00:00-08:00"
draft = false
title = "Drone and Docker, Open Source CI"

[author]
  name = "Brad Rydzewski"
  link = "https://twitter.com/bradrydzewski"
  photo = "https://pbs.twimg.com/profile_images/3253271781/90284722f819b80f92b5fd93a07ee109_400x400.jpeg"

+++

Today I'm excited to announce a new open source continuous integration server, written in Go and built on top of Docker. We call it project Drone. You can find the repository on GitHub at [github.com/drone/drone](https://github.com/drone/drone)

And a very nice writeup in VentureBeat by Jordan Novet:<br/>
[Drone.io’s shift hints at the future of sending software to clouds](http://venturebeat.com/2014/02/07/droneio/)

![Dashboard](/images/oss_screenshot_dashboard.png)

## Built on Docker

We believe containers have a bright future and we're ready to make a big bet on
Docker. Drone integrates tightly with Docker to execute each build inside a
virtual machine and stream the live output to the web console.

![Console](/images/oss_screenshot_console.png)

Drone provides a set of pre-built Docker images for 12+ languages and nearly
every major database. This means you don't have to spend time installing
software and configuring your build environment. Of course, if you need a
highly customized environment Drone gives you the flexibility to use a custom
Docker image.

## Built with Go

Drone is built entirely in Go. It is fast and efficient, leaving valuable
system resources available for your builds. This topic probably merits a blog
post of it’s own.

## Simple Installation

Drone is distributed as a single binary file with no external dependencies,
other than Docker.

Use the installation wizard to setup and configure Drone when
first launched. This includes creating your admin account, configuring your
SMTP server, and configuring your GitHub and Bitbucket OAuth credentials.

![Console](/images/oss_screenshot_wizard.png)

## How is it different than other CI services?

* Drone is open source
* Drone is built on Docker
* Drone is easily hosted on your own infrastructure
* Drone provides a CLI to run builds locally inside Docker containers
* Drone integrates with GitHub out of the box, Bitbucket patch coming soon

We believe there is plenty of room for innovation and improvement in the CI
space and we're excited to use Drone as a platform to experiment with new ideas.

## How will this impact our paid offering?

Our open source edition will be the foundation of our next-generation cloud
offering. We expect to have a beta available in March. If you would like to
join our beta program please reach out to [support@drone.io](mailto:support@drone.io)

## What is your product Roadmap?

The roadmap will largely be determined by the community over the coming weeks,
however, these are some areas we'd like to focus on:

* Support GitHub Enterprise
* Support generic Git repositories
* Support matrix builds
* More plugins to Deploy, Notify and Publish
* Stability and bug fixes (this is a 0.1 release, after all)

This is a humble 0.1 release and still very rough around the edges. That being
said, we will be rapidly improving the platform over the coming weeks and
you can expect new features daily.

We invite you to [download and install Drone](https://github.com/drone/drone)
and give it a try. We'd love to hear what you think and we're eager to improve
the platform with the help of the Open Source community.

**Interested in beta testing? Located in downtown SF?** We're happy to visit your
office in-person to help you get setup and answer any questions you may have.
For more information reach out to us [@droneio](https://twitter.com/droneio)
or [hello@drone.io](mailto:hello@drone.io)