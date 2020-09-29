---
date: 2018-11-21T00:00:00+00:00
title: Announcing Drone Cloud, A Free Continuous Integration Service for x86 and Arm
author: bradrydzewski
draft: false
weight: 1
tags: [ Announcements, Product Updates ]
image: /images/drone_1_screenshot.png

aliases: [/drone-coud]
---

__Drone Gives Thanks to the Open Source Community with Drone Cloud, A Free Continuous Integration Hosted Service for Open Source Projects__

With the help of [Packet](http://packet.net/), the bare metal cloud provider, Drone is today announcing [Drone Cloud](https://cloud.drone.io), a free continuous integration service for open source projects. Drone Cloud runs on blazing fast x86, Arm32 and Arm64 bare metal servers donated by Packet to provide multi-architecture CI/CD to the open source community. 

{{< signup >}}

On the day before Thanksgiving, we just want to thank the open source community for providing software that powers the industry forward. As part of the open source community ourselves, we are thankful to companies like Packet who are providing a substantial multi-year donation to support Drone Cloud and therefore help any open source project with a GitHub repository.

There is a lot of talk these days about open source models being broken but at the end of the day open source software is powering the infrastructure of our future forward. So thank you Packet for your support of open source software. Thank you open source community for your support of Drone since 2013, for your help and contributions to get us to the Drone 1.0 release candidate (stay tuned for Drone 1.0) and for powering the future of our connected lives forward. 

Enough thankful talk, let’s get to details of Drone Cloud ...

Drone Cloud enables developers to run Continuous Delivery pipelines across multiple architectures - including x86 and Arm (both 32 bit and 64 bit) - all in one place.  Powered by the latest generation of Intel, AMD and various Arm bare metal servers donated by Packet, Drone Cloud gives teams unprecedented performance, the ability to parallelize workloads and reduce execution time. An industry first, Drone Cloud provides the open source community with a container-native way of delivering code quickly. 

# What are the Hardware Specs?

Drone Cloud is powered by some serious hardware. The x64 machines are powered by the 7401P EPYC processor with 24 physical cores and 64 GB of RAM. [Learn more](https://www.packet.com/cloud/servers/c2-medium-epyc/).

Hardware Specs | --
---------------|--------------------------
CPU            | 24 Physical Cores @ 2.2 GHz 
Memory         | 64 GB of ECC RAM 
Storage        | 960 GB of SSD 
Network        | 20 Gbps Bonded Network

The Arm machines are powered by a Cavium ThunderX processor with 96 physical cores and 128 GB of RAM. [Learn more](https://www.packet.com/cloud/servers/c1-large-arm/).

Hardware Specs | --
---------------|--------------------------
CPU            | 96 Physical Cores @ 2.0 GHz
Memory         | 128 GB of DDR4 ECC RAM
Storage        | 250 GB of SSD 
Network        | 20 Gbps Bonded Network

# What is Drone?

_If you are learning about Drone for the first time ..._

Drone is a modern CI/CD platform built with a containers-first architecture. Drone looks for special YAML files within repositories for the pipeline definition. The syntax is designed to be easy to read and expressive so that anyone using the repository can understand the continuous integration process.

{{< highlight yaml >}}
---
kind: pipeline
name: default

steps:
- name: backend
  image: golang
  commands:
  - go build
  - go test

- name: frontend
  image: node
  commands:
  - npm install
  - npm test

...
{{< / highlight >}}

# What Languages does Drone support?

Drone supports any language that can run inside of a Docker container. We have samples for 20+ popular languages in our documentation. [Learn more](https://docs.drone.io/samples/languages/).

# Does Drone support Plugins?

Drone provides a plugin system, but it is used differently than the one in Jenkins. In Drone, plugins are special Docker containers used to drop preconfigured tasks into the regular workflow. You can choose from hundreds of community plugins or bring your own.

{{< highlight yaml "hl_lines=12-16" >}}
---
kind: pipeline
name: default

steps:
- name: build
  image: golang
  commands:
  - go build
  - go test

- name: notify
  image: plugins/slack
  settings:
    webhook:
      from_secret: token
...
{{< / highlight >}}

# Does Drone support Matrix Builds?

__Yes__. Drone supports testing your code against multiple language versions, database versions, architectures and more. Think of it like matrix builds on steroids. You can configure an execution graph (DAG) to distribute your workflow across multiple machines and architectures. [Learn more](https://readme.drone.io/config/pipeline/multi-platform/).

_With 16,000 github stars and a robust community, Drone has been at the forefront of container-driven workflows. Drone is empowering development teams to deliver software at unprecedented rates. Follow us on Twitter [@droneio](https://twitter.com/droneio) or on [Github](https://github.com/drone) for news and product updates._