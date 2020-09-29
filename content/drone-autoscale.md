---
date: 2018-03-08T00:00:00+00:00
title: Announcing Drone Autoscale
author: bradrydzewski
image: /images/autoscale_hero.png
weight: 1
tags: [ Announcements, Product Updates ]
---

Today we're excited to announce the launch of __Drone Autoscale__. Drone Autoscale is a standalone daemon that continuously polls your build queue and provisions or terminates instances based on volume. Our goal is to increase your capacity when you need it, while __lowering your monthly server bills__.

This is great, right? But how is it different than existing solutions? The biggest difference is integration and intelligence. Drone will never terminate your server mid-build, or worse, mid-deployment. And if it determines your environment is in an inconsistent state, it will repair things. __Automatically__.

## Cloud Providers

Drone Autoscale is architected to integrate with multiple hosting providers. This initial release includes support for [DigitalOcean](https://m.do.co/c/00500d28741b) and alpha support for [Amazon EC2](http://autoscale.drone.io/intro/amazon/). Support for Google Cloud Platform, Microsoft Azure, and Hetzner Cloud are in-progress.

# Notifications

Drone Autoscale integrates with Slack to notify your team when instances are provisioned and terminated.

![](/images/autoscale_slack_messages.png)

## Command Line Tools

The [command line tools](https://github.com/drone/drone-cli) have been updated and include commands for working with the autoscale server. You can now inspect and manage remote instances from the terminal:

{{< terminal >}}
$ drone server ls
agent-807jVFwj
agent-IOOsCNd7
agent-hNhs08tj
agent-TiefPo1u

$ drone server info agent-TiefPo1u
Name:    agent-TiefPo1u
Address: 52.3.250.114
Region:  us-east-1e
Size:    t2.medium
State:   running
{{< /terminal >}}

Sometimes you need to quickly inspect a remote instance. The command line tools help you easily connect to the remote docker daemon:

{{< terminal >}}
$ eval $(drone server env agent-TiefPo1u)
$ docker ps -a
CONTAINER ID        IMAGE                   STATUS      
b6aeabe2836f        drone/agent:0.8         2 days ago  
{{< /terminal >}}

Not quite ready for automated scaling? Not a problem. You can always run the autoscaler in manual mode and provision and terminate instances from the command line.

{{< terminal >}}
$ drone server create
Name:    agent-TiefPo1u
Address: 52.3.250.114
Region:  us-east-1e
Size:    t2.medium
State:   pending

$ drone server destroy agent-TiefPo1u
Name:    agent-TiefPo1u
State:   shutdown
{{< /terminal >}}

## Commercial License

Drone Autoscale is distributed under a freemium license. If you are interested in purchasing a commercial license, which includes enterprise support and access to our private Slack channel, please contact our [sales team](mailto:sales@drone.io).

## Getting Started

Find out more about how to use Drone Autoscale for cost effective and scalable Continuous Delivery with the following resources:

* [Repository](https://github.com/drone/autoscaler)
* [Documentation](https://autoscale.drone.io)
* [Installation](http://autoscale.drone.io/intro/)
* [Support](https://discourse.drone.io)

We’ll be adding new features and integrations over the coming weeks and months and look forward to [hearing your feedback](https://twitter.com/droneio)!
