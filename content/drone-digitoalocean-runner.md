---
date: 2019-09-11T00:00:00+00:00
title: Announcing Digital Ocean Pipelines
author: bradrydzewski
draft: false
weight: 1
tags: [ Announcements, Product Updates ]
---

Last week we [announced]({{< relref "/drone-exec-runner.md" >}}) support for custom pipeline runners, expanding beyond Docker to include support for additional runtime environments. We announced [exec]({{< relref "/drone-exec-runner.md" >}}) pipelines and [ssh]({{< relref "/drone-ssh-runner.md" >}}) pipelines, capable of running workloads directly on a local or remote machine _without containers_.

Today we are excited to announce [Digital Ocean](https://digitalocean-runner.docs.drone.io/configuration/) pipelines. A Digital Ocean pipeline is executed on a _dedicated_ Droplet that is destroyed when the pipeline completes.

Example configuration:

```
---
kind: pipeline                                        
type: digitalocean
name: default

token:
  from_secret: do_token

server:
  image: docker-18-04
  size: s-1vcpu-1gb
  region: nyc1

steps:
- name: test
  commands:
  - go build
  - go test

- name: build
  commands:
  - docker build .
```

Digital Ocean pipelines are intended for workloads that require a dedicated virtual machine with full privileges, and are poorly suited for containers and multi-tenancy. You may also consider Digital Ocean pipelines if you have low build volume and want to reduce costs by provisioning droplets on-demand.

See the [documentation](https://digitalocean-runner.docs.drone.io) to install the Digital Ocean runner and learn how to configure the Digital Ocean pipelines. If you have any questions or feedback please [let us know](https://gitter.im/drone/drone).
