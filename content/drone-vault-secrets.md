---
date: 2018-08-21T00:00:00+00:00
title: Announcing Vault Secrets
author: bradrydzewski
draft: false
weight: 1
tags: [ Announcements, Product Updates ]
---

Today Drone.io, the leading open source cloud native continuous integration (CI) and continuous delivery (CD) platform, is announcing the official integration with HashiCorp’s [Vault](https://www.vaultproject.io/) secret manager which secures, stores, and tightly controls access to tokens, passwords, certificates, API keys, and other secrets in modern computing. This integration allows you to securely share secrets stored in Vault with your deployment pipelines. 

# Get Started using Drone.io with Vault

Get started by [installing](https://readme.drone.io/intro/) the 0.9 technology preview and by [installing](https://readme.drone.io/extend/secrets/vault/) the Vault plugin. Once installed, the first step is to create a Vault secrets resource (below). For demonstration purposes, lets store our Docker registry credentials, used to publish images to Dockerhub.

![Vault Secret Create](/images/vault_secret_add.png)

The secret should be visible in the dashboard once successfully created. Note that you can add the `X-Drone-Repos` and `X-Drone-Events` properties to limit which repositories and pipeline events have access to these secrets.

![Vault Secret Info](/images/vault_secret_info.png)

The next step is to define the external secrets in your `.drone.yml` configuration file. In the below example, we expose the `username` and `password` key values for secret `secret/data/docker`.

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
  path: secret/data/docker
  name: username

---
kind: secret
name: docker_password
get:
  path: secret/data/docker
  name: password
{{< /terminal >}}

_Drone is modern CI/CD. Container-native and available via open source and an enterprise on-prem edition. [Try it today](https://readme.drone.io)._