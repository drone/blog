---
date: 2019-01-24T00:00:00+00:00
title: Drone adds support for Bitbucket Pipelines
author: bradrydzewski
weight: 1
tags: [ Announcements, Product Updates ]
image: /images/drone_bitbucket_pipelines.png
---

Today Drone.io, the container-native CI/CD platform, is announcing support for the Bitbucket Pipeline configuration format. This means Drone can directly execute your `bitbucket-pipelines.yml` configuration file, providing portability across CI/CD environments.

With this announcement, Drone makes it easier for teams to move workloads from Bitbucket Cloud to an on-premise Bitbucket Server environment, Atlassian's Enterprise. Drone is the missing CI/CD system for Bitbucket Server.

<!-- It is no secret that companies use vendor-specific configuration files to make it easier to lock you into their products. This causes friction when an organization outgrows a hosted offering, and requires better performance, custom hardware, or has special data privacy and regulatory requirements. Here at Drone we are committed to portability. No matter where you store, test or deploy your code, Drone provides a common platform for software delivery across clouds and version control systems. -->

<!--

It is no secret that companies use vendor-specific configuration files to make it easier to lock you into their products. This causes friction when an organization outgrows a hosted offering, and requires better performance, custom hardware, or has special data privacy and regulatory requirements. Here at Drone we are committed to portability. No matter where you store, test or deploy your code, Drone provides a common platform for software delivery across clouds and version control systems.

-->

# Getting Started

Drone is able to convert a standard Bitbucket Pipeline configuration file to the Drone configuration file format. Below is an example Pipeline for a node project that will install, test and build a JavaScript application.

{{< highlight yaml >}}
pipelines:
  default:
    - step:
        name: Build and test
        image: node:8.5.0
        script:
          - npm install
          - npm test
          - npm build

definitions:
  services:
    postgres:
      image: postgres:9.6.4
{{< / highlight >}}


We can use the Drone command line utility to convert the Bitbucket Pipeline configuration to a native Drone Pipeline configuration.

```
$ drone convert bitbucket-pipelines.yml

---
kind: pipeline
name: default

platform:
  os: linux
  arch: amd64

steps:
- name: Build and test
  image: node:8.5.0
  commands:
  - npm install
  - npm test
  - npm build

services:
- name: postgres
  image: postgres:9.6.4

...
```

In the above example we manually converted the Bitbucket Pipeline using the Drone command line tools. Drone is also capable of automatic conversion. You can point the Drone server to your Bitbucket Pipeline configuration file for automatic runtime conversion:

![change the configuration path](/images/yaml_bitbucket_pipelines.png)

# Compatibility Overview

Our goal is to conform as closely as possible to the Bitbucket Pipeline implementation as outlined in the specification. There are, however, some underlying architectural differences that prevent full conformance when converting Bitbucket Pipelines to Drone Pipelines.

__Filesystem Compatibility__

Bitbucket steps have ephemeral, isolated filesystems. If a step generates artifacts that need to be shared with subsequent steps it must be [explicitly configured](https://confluence.atlassian.com/bitbucket/using-artifacts-in-steps-935389074.html) (example below). Drone, on the other hand, provides a shared filesystem for all steps in the Pipeline and does not require explicit artifact caching and restoring.

Example Bitbucket artifacts:

{{< highlight yaml "hl_lines=10-12">}}
pipelines:
  default:
    - step:
        name: Build and test
        image: node:8.5.0
        script:
          - npm install
          - npm test
          - npm build
        artifacts:
          - dist/**
          - reports/*.txt
{{< / highlight >}}

__Service Compatibility__

Bitbucket service containers are accessible by localhost. Drone service containers are accessible using a hostname that corresponds to the name of the service (i.e. `postgres`).

__Caching Compatibility__

Bitbucket provides native syntax in the yaml for [caching dependencies](https://confluence.atlassian.com/bitbucket/caching-dependencies-895552876.html). Drone supports caching and restoring dependencies using plugins, and defining explicit cache and restore steps. When Drone converts a `bitbucket-pipelines.yml` file it does not attempt to emulate caching.

Example Bitbucket cache syntax:

{{< highlight yaml "hl_lines=4-5" >}}
pipelines:
  default:
    - step:
        caches:
          - node
        script:
          - npm install
          - npm test
{{< / highlight >}}

Example Drone cache syntax:

{{< highlight yaml "hl_lines=5-10 18-23" >}}
kind: pipeline
name: default

steps:
- name: restore
  image: plugins/s3-cache
  settings:
    restore: true
    mount:
      - node_modules

- name: build
  image: node
  commands:
    - npm install
    - npm test

- name: rebuild
  image: plugins/s3-cache
  settings:
    rebuild: true
    mount:
      - node_modules
{{< / highlight >}}

# Commitment to Portability

We released multi-cloud, multi-architecture and multi-machine capabilities with the [Drone 1.0 release candidate](https://blog.drone.io/drone-1-release-candidate-1/). Drone will continue to add capabilities that make code simple and easy to deploy across teams, no matter your git management tool, cloud or operating system. Stay tuned for additional support announcements in the coming weeks. 

---

_Drone.io has been at the forefront of how to deploy with containers and the first to deliver a container CI/CD model. Drone has a robust ecosystem of plug-ins such as Kubernetes, Helm, GitHub and more, to help developers with their pipelines. With 17,000 GitHub stars and a passionate community, Drone is empowering developers to quickly fix bugs and deliver new applications to market faster. Drone is pluggable, repeatable and portable by nature, putting the power of deployment in developers hands. Follow us on Twitter [@droneio](https://github.com/drone/drone)._
