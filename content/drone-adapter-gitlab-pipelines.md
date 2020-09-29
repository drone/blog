---
date: 2019-02-01T00:00:00+00:00
title: Drone adds support for GitLab Pipelines
author: bradrydzewski
weight: 1
tags: [ Announcements, Product Updates ]
image: /images/drone_gitlab_ci_2.png
---

Today we're expanding Drone's support for portability across different CI/CD environments by announcing compatibility with the GitLab CI configuration format. This means you can use Drone to directly execute your `.gitlab-ci.yml` configuration file.

Drone is a container-native CI/CD platform that empowers developers, and expands their choice of tools by providing portability across multiple operating systems and architectures. Today’s expanded compatibility follows-up on our recently announced [support for Bitbucket Pipelines](/drone-adapter-bitbucket-pipelines/) and reiterates our commitment to a portable CI/CD environment. 

# Getting Started

Drone is able to convert a standard GitLab configuration file to the Drone configuration file format. Below is an example CI configuration for a Ruby project that installs its dependencies and executes its unit tests.

{{< highlight yaml >}}
image: ruby:2.2

services:
  - postgres:9.3

before_script:
  - bundle install

test:
  script:
  - bundle exec rake spec
{{< / highlight >}}


We can use the Drone command line utility to convert the GitLab CI configuration to a native Drone Pipeline configuration.

```
$ drone convert .gitlab-ci.yml

---
kind: pipeline
name: test

platform:
  os: linux
  arch: amd64

steps:
- name: test
  image: ruby:2.2
  commands:
  - bundle install
  - bundle exec rake spec

services:
- name: postgres-9-3
  image: postgres:9.3

...
```

In the above example we manually converted the GitLab CI using the Drone command line tools. Drone is also capable of automatic conversion. You can point the Drone server to your GitLab CI configuration file for automatic runtime conversion:

![change the configuration path](/images/yaml_gitlab.png)

# Compatibility Overview

Our goal is to conform as closely as possible to the GitLab CI implementation as [outlined in the specification](https://docs.gitlab.com/ce/ci/yaml/). There are, however, some underlying architectural differences that prevent full conformance when converting a GitLab CI configuration to a Drone Pipeline.

GitLab steps have ephemeral, isolated filesystems. If a step generates files that need to be shared with subsequent steps it must be [explicitly configured](https://docs.gitlab.com/ee/ci/caching/) (example below). Drone, on the other hand, provides a shared filesystem for all steps in the Pipeline and does not require explicit artifact caching and restoring.

Example GitLab cache syntax:

{{< highlight yaml "hl_lines=1-5" >}}
cache:
  untracked: true
  key: "$CI_BUILD_REF_NAME"
  paths:
    - node_modules/

stages:
  - setup
  - test

setup:
  stage: setup
  script:
    - npm install

test:
  stage: test
  script:
    - npm test
{{< / highlight >}}

Example Drone syntax, no caching necessary:


{{< highlight yaml >}}
---
kind: pipeline
name: setup

platform:
  os: linux
  arch: amd64

steps:
# this step installs node_modes in the root
# directory of your git repository. This directory
# is a mounted volume.
- name: setup
  commands:
  - npm install

# because the root of your repository is a mounted
# volume, this still will have access to the node_modules
# folder created in the previous step.
- name: test
  commands:
  - npm test

depends_on:
- setup

...
{{< / highlight >}}

# Commitment to Portability

We released multi-cloud, multi-architecture and multi-machine capabilities with the [Drone 1.0 release candidate](https://blog.drone.io/drone-1-release-candidate-1/). Drone will continue to add capabilities that make code simple and easy to deploy across teams, no matter your git management tool, cloud or operating system. Stay tuned for additional support announcements in the coming weeks. 

---

_Drone is modern CI/CD. Container-native and available via open source and as an enterprise on-prem edition. Try it today. Follow us on Twitter [@droneio](https://twitter.com/droneio) or on [Github](https://github.com/drone/drone). Or sign up for our email newsletter to save up to date on all Drone news._