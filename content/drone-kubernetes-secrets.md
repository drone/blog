---
date: 2018-08-28T00:00:00+00:00
title: Drone Integration with Kubernetes Secrets
author: bradrydzewski
weight: 1
tags: [ Announcements, Product Updates ]
---

Today Drone.io, the leading open source cloud native continuous integration (CI) and continuous delivery (CD) platform, is announcing the official integration with the [Kubernetes](hhttps://kubernetes.io/docs/concepts/configuration/secret/) secret manager which secures, stores, and tightly controls access to tokens, passwords, certificates, API keys, and other secrets in modern computing. This integration allows you to securely share secrets stored in Kubernetes with your deployment pipelines. 

# Get Started using Drone.io with Kubernetes

Get started by [installing](https://readme.drone.io/intro/) the 0.9 technology preview and by [installing](https://readme.drone.io/extend/secrets/kubernetes/) the Kubernetes plugin. Once installed, the first step is to create a Kuberetes secret object (below). For demonostration purposes, lets store our Docker registry credentials, used to publish images to Dockerhub.

![Kubernetes Secret Create](/images/kubernetes_secret_add.png)

The secret should be visible in the dashboard once successfully created. Note that you can use the `X-Drone-Repos` and `X-Drone-Events` annotations to limit which repositories and pipeline events have access to these secrets.

![Kubernetes Secret Info](/images/kubernetes_secret_info.png)

The next step is to define the external secrets in your `.drone.yml` configuration file. In the below example, we expose the `username` and `password` key values for secret `docker`.


{{< terminal >}}
kind: pipeline
name: default

steps:
- name: publish
  image: plugins/docker
  settings:
    repo: octocat/server
    tags: latest
    username:
      from_secret: docker_username
    password:
      from_secret: docker_password

---
kind: secret
name: docker_username
get:
  path: docker
  name: username

---
kind: secret
name: docker_password
get:
  path: docker
  name: password
{{< /terminal >}}

_Drone is modern CI/CD. Container-native and available via open source and as an enterprise on-prem edition. Try it today. Follow us on Twitter [@droneio](https://twitter.com/droneio) or on [Github](https://github.com/drone/drone). Or sign up for our email newsletter to save up to date on all Drone news._
