+++
date = "2014-12-15T23:35:13-08:00"
draft = false
title = "Drone 0.3: Gitlab, Bitbucket, Slack and more"

[author]
  name = "Brad Rydzewski"
  link = "https://twitter.com/bradrydzewski"
  photo = "https://pbs.twimg.com/profile_images/3253271781/90284722f819b80f92b5fd93a07ee109_400x400.jpeg"
+++

I wanted to take the opportunity to providate an progress update for the 0.3 release. This release includes a lot of fundamental changes to the Drone architecture based on feedback and real world usage of the tool. While it has slowed down our release cycle a bit, I really believe we are building a strong foundation for the project.

While we haven't officially cut a 0.3 release, it is already being heavily used in production by some really great teams. Feel free to grab the latest snapshot from master and give it a try. Here are some of the great features that are waiting for you:

## Improved Design

The user and repository dashboards were updated to a card-based layout. This layout attempts to surface the most relevant repositories and commits so that you don't have to visually parse a long list. If you are logging into Drone you are most likely responding to a failure -- failed builds are therefore styled to immediately capture your attention:

![Build Status](/images/drone_0.3_commit_list.png) 

The commit detail is now full screen and optimized for auto-scrolling. The commit information is affixed while your browser automatically scrolls with the streaming output, and links back to the original commit (or pull request) in GitHub.

![Build Status](/images/drone_0.3_commit.png)

This is the third major iteration of the user interface and yet we still have a lot to learn about how people use this software. You can expect a lot of changes (and improvements) over the next few months as we continue to gather feedback and optimize the user experience.

## Delegated Authentication

Remember how in 0.2 you had to create an account with your email address and password? And how you had to invite your colleagues via email to create accounts? Well, things just got much easier. Drone now uses your native GitHub account, GitHub login, and GitHub permissions.

## Flexible Configuration

Remember how in 0.2 you could manage all Drone configuration from the website? This made for an awesome demo, however, it turns out the approach doesn't scale too well. We found that Drone requires a good amount of customization and configuration to meet the diverse needs of the developer community. These needs are best expressed in a configuration file.

## GitLab Support

[GitLab](https://about.gitlab.com/) is one of the most popular self-hosted version control solutions out there, which is why we were extremely excited to accept pull requests from [@Bugagazavr](https://github.com/Bugagazavr) and [@fudanchii](https://github.com/fudanchii) adding GitLab integration. We currently support version 7.7 and higher. Please [see our documentation](http://readme.drone.io/setup/config/gitlab/) to get started.

## Bitbucket Support

[Bitbucket](https://bitbucket.org/) is one of the most popular saas version control solutions out there, especially if you prefer mercurial to git, or if you just don't want to pay for private GitHub repositories (it's ok, we all do it). Thanks to contributions from [@fd](https://github.com/fd) and [@asabil](https://github.com/asabil) you can now integrate Drone with Bitbucket. Please [see our documentation](http://readme.drone.io/setup/config/bitbucket/) to get started.

## Slack Notifications

This is probably our most popular new features implemented by [@ciarand](https://github.com/ciarand) and refined by [@brettlangdon](https://github.com/brettlangdon), [@ap4y](https://github.com/ap4y) and [@compressed](https://github.com/compressed), [@marksteve](https://github.com/marksteve) (I told you it was popular). Add the following configuration to your project to post build results to your Slack channel:

```
notify:
  slack:
    webhook_url: 'https://hooks.slack.com/services/...'
    username: 'drone'
    channel: '#general'
    on_started: false
    on_success: true
    on_failure: true
```

## Gitter Notifications

[Gitter](https://gitter.im) is a collaboration platform optimized for developers using GitHub (very cool). The Gitter notification plugin was contributed by [@Bugagazavr](https://github.com/Bugagazavr). Add the following configuration to your project to post build results to your Gitter room:

```
notify:
  gitter:
    room_id: cf23db8155e6700caccc
    token: c90f0e53d4973302fe0b6159488423d6
    on_started: false
    on_success: true
    on_failure: true
``` 


## Try 0.3 Today

Even though 0.3 hasn't been officially released we highly recommend you [download and install](http://readme.drone.io/setup/install/ubuntu/) the latest snapshot from master. There are a ton of great features waiting for you and we are anxious to hear your feedback.

