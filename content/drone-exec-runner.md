---
date: 2019-09-03T00:00:00+00:00
title: Announcing Exec Pipelines
author: bradrydzewski
draft: false
weight: 1
tags: [ Announcements, Product Updates ]
---

Today Drone, the leading open source cloud native Continuous Integration (CI) and Continuous Delivery (CD) platform, is announcing official support for multiple runtime environments. This is the first of a series of announcements to highlight different runtime environments, starting with the ability to run pipelines directly on the host machine _without containers_.

# Why not containers?

There are many benefits to executing your pipelines inside containers, however, some workloads are poorly suited for container runtimes. The most obvious examples are macOS and iOS projects.

We also recognize thousands of development teams still use Jenkins and Bamboo, which execute pipelines directly on the host. We want to support these teams without forcing migration to containers or kubernetes. _If you want to migrate to container-based workloads we can do that too_ 😃.

# Introducing Exec Pipelines

An `exec` pipeline is a new type of pipeline that executes shell commands directly on the host machine without using containers. An exec pipeline is executed by an exec runner (aka agent).

Example Pipeline configuration:

```
---
kind: pipeline
type: exec
name: default

steps:
- name: backend
  commands:
  - go build
  - go test

- name: frontend
  commands:
  - npm install
  - npm test
```

The exec pipeline [defines its own yaml syntax](https://exec-runner.docs.drone.io/specification/). The system uses the `type` attribute to identify the type of pipeline and route to the appropriate runner.

Defines an `exec` pipeline:

```
---
kind: pipeline
type: exec
```

Defines a `docker` pipeline:

```
---
kind: pipeline
type: docker
```

# Improved Debugging

One challenge with installing and maintaining a distributed continuous integration system is debugging. To improve the debugging experience, each runner now provides access to execution history and system logs via the browser.

![runner logs](/images/runner_dashboard_logs.png)

![runner logs per-pipeline](/images/runner_dashboard.png)

# Final Words

Give [Drone](https://docs.drone.io/installation) a try today. You can get up and running in less than 10 minutes. If you have any questions or need assistance please [get in touch](https://discourse.drone.io).