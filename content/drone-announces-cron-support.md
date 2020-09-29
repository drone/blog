---
date: 2018-11-08T00:00:00+00:00
title: Native Support for Cron Scheduling
author: bradrydzewski
weight: 1
tags: [ Announcements, Product Updates ]
---

This week we announced our first [release candidate]({{< ref "drone-1-release-candidate-1.md" >}}) for Drone 1.0 and we highlighted some exciting new features, including support for multi-machine, multi-os and multi-architecture Pipelines.

Today I wanted to take an opportunity to highlight native support for scheduling. Drone 1.0 includes support for scheduled Pipeline executions using cron expression syntax. You could, for example, schedule automated deployments at `0 17 * * 5`.

![deploying on friday](/images/deploy_on_friday_meme.jpg)

Managing cron jobs is pretty straightforward. You simply navigate to your repository settings screen, where you can create cron jobs by filling out a simple form. You can alternatively manage cron jobs from the CLI.

{{< terminal >}}
$ drone cron add octocat/hello-world nightly "0 0 0 * * *"
$ drone cron info octocat/hello-world nightly

nightly 
expr: 0 0 0 * * *
next: 2018-11-08 00:00:00 -0800 PST
{{< /terminal >}}

If you are like me and you sometimes have difficulty cron experssion syntax you may use one of several pre-defined schedules in place of a cron expression:

* `@yearly`
* `@monthly`
* `@weekly`
* `@daily`
* `@hourly`
* `@every <duration>` such as `@every 1h30m10s`

# Try It Today

If you would like to try out the new Cron scheduler please [download](https://readme.drone.io) our 1.0.0 release candidate. If you need any help, or if you have any feedback or suggestions for improvement please [let us know](https://discourse.drone.io).


_With over 16,000 github stars and a robust community, Drone has been at the forefront of container-driven Continuous Delivery workflows. Follow us on Twitter [@droneio](https://twitter.com/droneio) or on [Github](https://github.com/drone) for product updates and other announcements._