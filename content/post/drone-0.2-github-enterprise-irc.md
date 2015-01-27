+++
date = "2014-03-18T23:35:13-08:00"
draft = false
title = "Drone 0.2: GitHub Enterprise, IRC and more"

[author]
  name = "Brad Rydzewski"
  link = "https://twitter.com/bradrydzewski"
  photo = "https://pbs.twimg.com/profile_images/3253271781/90284722f819b80f92b5fd93a07ee109_400x400.jpeg"
+++


A few weeks ago we released the [open source edition of Drone](https://github.com/drone/drone), a continuous integration server built on Docker. The level of enthusiasm and participation
from the community has been incredible. Today I'm excited to introduce the **0.2 release** and highlight the progress we've made with the help of the community. I'd also like to thank the **30+ contributors** that have added features, found bugs, fixed bugs, and provided valuable feedback.

## GitHub Enterprise Support

It took a total for 24 hours for this patch to land, courtousy of [@floatdrop](https://github.com/floatdrop)
from Yandex. You can now modify the default GitHub url and replace with your
GitHub enterprise url.

![Console](/images/oss_screenshot_github_enterprise.png)

This feature is still considered beta and has a few outstanding issues that need
to be resolved ([#91](https://github.com/drone/drone/issues/91) and
[#92](https://github.com/drone/drone/issues/92)) however, it is already being
used by a number of organizations.

## IRC Notifications

This notification plugin was added by [@jordane](https://github.com/jordane)
from Rackspace during a recent hack day event. It will send messages to an IRC channel
when your build starts and finishes.

```
notify:
  irc:
    nick: bradrydzewski
    server: irc.freenode.net
    channel: #docker
    started: true
    success: true
    failure: true
    
```

## SSH Deployments

This deployment plugin was added by [@fudanchii](https://github.com/fudanchii).
It will copy artifacts to a server using **scp** and, once complete, will execute
a remote command using **ssh**.

```
deploy:
  ssh:
    target: user@test.example.com:/srv/app/location 2212
    artifacts:
      - build.result
      - config/file
    cmd: /opt/bin/redeploy.sh
```

## Modulus Deployments

This deployment plugin was added by [@fiveisprime](https://github.com/fiveisprime)
from the [modulus.io](https://modulus.io) team. It will deploy your node.js application
to the modulus cloud service.

```
deploy:
  modulus:
    project: my-app
    token: d78c03d72e72b44a
```

## Wall Display

This is one of my favorites because it is a completely separate project,
built **on top** of Drone. The dashboard was created by [@scottferg](https://github.com/scottferg)
of Vokal Interactive to format build results for large monitor or television displays.
There are reported sightings of Scott's dashboard at multiple Bay Area offices.

You can find the project at [github.com/drone/drone-wall](https://github.com/drone/drone-wall).

![TV Dashboard](https://pbs.twimg.com/media/BiIgju7CYAA6jz-.png)

## Give it a Try!

We invite you to [download and install](http://readme.drone.io/setup/install/ubuntu/)
Drone and give it a try. We'd love to hear what you think and we're eager to
improve the platform with the help of the open source community.


