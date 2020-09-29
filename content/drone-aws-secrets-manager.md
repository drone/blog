---
date: 2018-09-24T00:00:00+00:00
title: Drone Integration with the AWS Secrets Manager
author: bradrydzewski
weight: 1
tags: [ Announcements, Product Updates ]
---

Today Drone.io, the leading open source cloud native continuous integration (CI) and continuous delivery (CD) platform, is announcing the official integration with the [AWS Secret Manager](https://aws.amazon.com/secrets-manager/) secret manager which secures, stores, and tightly controls access to tokens, passwords, certificates, API keys, and other secrets in modern computing. This integration allows you to securely share secrets stored in AWS with your deployment pipelines. 

# Get Started using Drone with AWS Secrets

Get started by [installing](https://readme.drone.io/intro/) the 0.9 technology preview and by [installing](https://readme.drone.io/extend/secrets/amazon/) the Amazon plugin. Once installed, the first step is to create a secret resource (below). For demonstration purposes, lets store our Docker registry credentials, used to publish images to Dockerhub.

![AWS Secret Create](/images/aws_secrets_manager_add_1.png)

Note that you can use the `X-Drone-Repos` and `X-Drone-Events` annotations to limit which repositories and pipeline events have access to these secrets (above). Next you will need to name your secret (below).

![Kubernetes Secret Info](/images/aws_secrets_manager_add_2.png)

The final step is to define the external secrets in your `.drone.yml` configuration file. In the below example, we expose the `username` and `password` key values for secret `prod/docker`.

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
  path: prod/docker
  name: username

---
kind: secret
name: docker_password
get:
  path: prod/docker
  name: password
{{< /terminal >}}

_Drone is modern CI/CD. Container-native and available via open source and an enterprise on-prem edition. [Try it today](https://readme.drone.io)._