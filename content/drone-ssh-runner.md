---
date: 2019-09-05T00:00:00+00:00
title: Announcing SSH Pipelines
author: bradrydzewski
draft: false
weight: 1
tags: [ Announcements, Product Updates ]
---

Earlier this week we [announced]({{< relref "/drone-exec-runner.md" >}}) support for custom pipeline runners, expanding support beyond Docker to support additional runtime environments. We also announced [exec]({{< relref "/drone-exec-runner.md" >}}) pipelines, capable of running workloads directly on the host machine without containers.

Today we are excited to announce [SSH](https://ssh-runner.docs.drone.io/specification/) pipelines. An SSH pipeline executes on a remote machine using the SSH protocol. You can use SSH pipelines with Windows, Linux and Posix-compliant operating systems.

Example configuration:

```
---
kind: pipeline
type: ssh
name: default

server:
  host: 1.2.3.4
  user: root
  password:
    from_secret: password

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

The target use case for an SSH pipeline is when you need to run a series of commands on a static, remote server. You can also use SSH pipelines on [cloud.drone.io](https://cloud.drone.io) to bring your own servers.

See the SSH pipeline [documentation](https://ssh-runner.docs.drone.io) to learn more.

# Final Words

Give [Drone](https://docs.drone.io/installation) a try today. You can get up and running in less than 10 minutes. If you have any questions or need assistance please [get in touch](https://discourse.drone.io).