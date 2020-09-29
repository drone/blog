---
date: 2018-11-07T00:00:00+00:00
title: Announcing the Drone 1.0 Release Candidate
author: bradrydzewski
draft: false
weight: 1
tags: [ Announcements, Product Updates ]
image: /images/drone_1_screenshot.png
---

Today Drone, a cloud native continuous integration (CI) and continuous delivery (CD) platform, is excited to announce its 1.0 release candidate. This much anticipated release of Drone has been a labor of love, and brings the most advanced container-native offering to market. Drone can scale from startup to enterprise, across multiple clouds, operating systems and architectures.

>  With each release of Drone we get closer to CI/CD perfection. Drone was already the most advanced cloud native CI/CD player out there but today they are now far and away the best offering available. _Thomas Boerger, Senior DevOps Engineer at ownCloud_

# Multi-Machine, Multi-Architecture, Multi-Cloud

With this release we are focused on providing support for __multiple operating systems and architectures__, including Linux amd64, Linux arm, Linux arm64 and Windows server. The yaml configuration file has been revamped to support multiple platforms:

```
kind: pipeline

platform:
  arch: arm
  os: linux

steps:
- name: build
  image: golang
  commands:
  - go build
  - go test
```

The configuration format also supports multi-document yaml configuration files, used to represent complex, multi-machine workflows with __fan-in and fan-out__ capabilities.

```
kind: pipeline
name: backend

platform:
  arch: arm
  os: linux

steps:
- name: build
  image: golang
  commands:
  - go build
  - go test

---
kind: pipeline
name: frontend

platform:
  arch: amd64
  os: linux

steps:
- name: build
  image: node
  commands:
  - npm install
  - npm test

depends_on:
- backend
```

_Example multi-machine, multi-architecture pipeline. The first pipeline executes on linux arm and the second pipeline executes on amd64._

## Jsonnet Configuration

We are excited to provide support for [Jsonnet](https://jsonnet.org/) configuration files. Jsonnet is a templating language that includes support for functions, variables, imports and more. Jsonnet can help teams organize and manage complex configurations.

In the below example we can compare a multi-architecture pipeline configured in Jsonnet (left) with a traditional yaml configuration (right).

{{% splitview %}}
```
local Pipeline(arch) = {
  kind: "pipeline",
  name: arch,
  steps: [
    {
      name: "build",
      image: "golang",
      commands: [
        "go build",
        "go test",
      ]
    }
  ]
};

[
  Pipeline("amd64"),
  Pipeline("arm64"),
  Pipeline("arm"),
]
```

```
---
kind: pipeline
name: amd64

platform:
  os: linux
  arch: amd64

steps:
- name: build
  image: golang
  commands:
  - go build
  - go test

---
kind: pipeline
name: arm64

platform:
  os: linux
  arch: amd64

steps:
- name: build
  image: golang
  commands:
  - go build
  - go test

---
kind: pipeline
name: arm

platform:
  os: linux
  arch: amd64

steps:
- name: build
  image: golang
  commands:
  - go build
  - go test

...
```
{{% / splitview %}}

## Revamped User Interface

We are incredibly excited to work with the team at [PixelPoint](https://pixelpoint.io/) to revamp the Drone user interface. The release candidate includes a shapshot of our latest front-end development efforts and we are eager to hear your feedback.

![drone_1_screenshot](/images/drone_1_screenshot.png)

# Try it Today

This is just a small preview of all the great features and improvements we are bringing to Drone 1.0. The Drone 1.0 release candidate is available [here](https://readme.drone.io) for testing, with the final release available in the near future. We are excited to share our work with the community and are eager to [hear your feedback](https://discourse.drone.io).

_With 16,000 github stars and a robust community, Drone has been at the forefront of container-driven workflows. Drone is empowering development teams to deliver software at unprecedented rates. Follow us on Twitter [@droneio](https://twitter.com/droneio) or on [Github](https://github.com/drone) for news and product updates._