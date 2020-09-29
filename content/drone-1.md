---
date: 2019-04-17T00:00:00+00:00
title: Announcing Drone 1.0
author: bradrydzewski
draft: false
weight: 1
tags: [ Announcements, Product Updates ]
# image: /images/drone1_homepage.png
---

Today Drone, a cloud native continuous integration (CI) and continuous delivery (CD) platform, is excited to announce its 1.0 release. It has been nearly 5 years since the first commit was pushed to GitHub in 2014. This much anticipated release of Drone has been a labor of love, and brings the most advanced container-native offering to market. Drone can scale from startup to enterprise, across multiple clouds, operating systems and architectures.

>  With each release of Drone we get closer to CI/CD perfection. Drone was already the most advanced cloud native CI/CD player out there but today they are now far and away the best offering available. _Thomas Boerger, Senior DevOps Engineer at ownCloud_

# What is Drone?

_If you are learning about Drone for the first time ..._

Drone is a modern continuous integration system built with a containers-first architecture. Pipelines are configured using a special [YAML file](https://docs.drone.io/config/) that you check-in to your git repository. The syntax is designed to be easy to read and expressive so that anyone using the repository can understand the continuous delivery process.

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

{{< / highlight >}}

Drone executes build, test and deployment commands against your code inside isolated containers. The major benefit of containerized testing environments is the portability of your testing infrastructure. With containers, Drone can automatically download, install and create your testing environment without having to either manually set up and maintain infrastructure or sacrifice environmental fidelity. 

<!--
Drone also provides a simple yet powerful plugin system. In Drone, plugins are special Docker containers used to drop preconfigured tasks into the regular workflow. This makes it easier to accomplish common tasks by calling the plugin with a few parameters rather than scripting the entire process manually. In this sense, Drone plugins are somewhat similar to Unix utility commands that are designed to do one narrowly-focused task well.

{{< highlight yaml "hl_lines=11-15" >}}
---
kind: pipeline
name: default

steps:
- name: test
  image: golang
  commands:
  - go test

- name: notify
  image: plugins/slack
  settings:
    channel: general
    webhook: https://hooks.slack.com/services/...

{{< / highlight >}}

_Example pipeline configuration that sends a Slack notification. You can learn more about the Slack plugin, and discover more plugins, at our [plugin marketplace](http://plugins.drone.io)._

-->

# Updated Branding and User Interface

Drone 1.0 introduces a new user experience and branding courtesy of the team at [PixelPoint](https://pixelpoint.io/). This is the first phase in our improved user interface enhancements. We will continue to iterate on the design and rollout new features and improvements over the coming weeks and months.

![homepage](/images/drone1_homepage.png)
![screenshot](/images/drone1_screenshot.png)

# Multi-Machine, Multi-Architecture, Multi-Cloud

Drone 1.0 introduces support for multiple operating systems and architectures, including Linux amd64, Linux arm, Linux arm64 and Windows server. The YAML configuration file has been revamped to support multiple platforms:

{{< highlight yaml "hl_lines=4-6" >}}
---
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

{{< / highlight >}}

Drone also supports multi-document YAML files, used to model complex, multi-machine workflows with fan-in and fan-out capabilities.

{{< highlight yaml "hl_lines=3 16 31-32" >}}
---
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

{{< / highlight >}}

_Example multi-machine, multi-architecture pipeline. The first pipeline executes on linux arm and the second pipeline executes on amd64._

## Faster Pipelines with Parallelization

Drone 1.0 introduces optional support for executing your pipeline as a directed acyclic graph using the `depends_on` keyword. This can be used to run tasks in parallel, resulting in faster pipeline execution and better hardware utilization.

{{< highlight yaml "hl_lines=6 12 22-24" >}}
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
  image: golang
  commands:
  - npm install
  - npm test

- name: build
  image: plugins/slack
  settings:
    channel: general
  depends_on:
  - backend
  - frontend

{{< / highlight >}}

_Example pipeline will fan-out and run the backend and frontend steps in parallel, and once complete, will fan-in and send a Slack notification._

## Complex Configurations Simplified

Drone 1.0 introduces native support for [Jsonnet](https://jsonnet.org/) configuration files. Jsonnet is a templating language that includes support for functions, variables, imports and more. Jsonnet can help teams organize and manage complex configurations.

_In the below example we can compare a multi-architecture pipeline configured in Jsonnet (left) with a traditional YAML configuration (right)._

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

```
{{% / splitview %}}

## Flexible Secret Management

Drone 1.0 introduces a new category of plugins for sourcing secrets from external providers such as [Vault](/drone-vault-secrets), the [AWS Secrets Manager](/drone-aws-secrets-manager) and [Kubernetes Secrets](/drone-kubernetes-secrets). Enterprises can create custom plugins to integrate with their existing security infrastructure and access controls for enhanced security and compliance.

{{< highlight yaml "hl_lines=2-6 23-24" >}}
---
kind: secret
name: slack_webhook
get:
  path: secret/data/slack
  name: webhook

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
    channel: general
    webhook:
      from_secret: slack_webhook
{{< / highlight >}}

_Example pipeline sources a Slack credentials from an external key value pair stored in Vault. The secret is requested at runtime and injected into the pipeline._

## Cron Scheduling

Drone 1.0 introduces support for scheduling `@hourly`, `@dailys`, `@weekly` and `@monthly` jobs. Sometimes we need to execute pipelines not because we have changed anything, but because maybe our dependencies have changed, or we want to have an automatic build to get a nightly release out.

_Cron scheduling can be managed from the repository settings screen in the user interface, or with the command line utility._

![cron](/images/drone1_cron.png)

# Conclusion

Drone 1.0 provides a lot of flexibility and power to help your organization automate software testing and delivery. You can get started with Drone in a matter of minutes by downloading our official Docker image and following our [installation guide](https://docs.drone.io/installation). If you need any help, or if you have any feedback or suggestions for improvement please [let us know](https://gitter.im/drone/drone).

_With 18,000 github stars and a robust community, Drone has been at the forefront of container-driven workflows. Drone is empowering development teams to deliver software at unprecedented rates. Follow us on Twitter [@droneio](https://twitter.com/droneio) or on [Github](https://github.com/drone) for news and product updates._