---
date: 2019-04-23T00:00:00+00:00
title: Announcing Drone 1.1.0
author: bradrydzewski
draft: false
weight: 1
tags: [ Announcements, Product Updates ]
# image: /images/drone1_homepage.png
---

Drone 1.1.0 is now available. This minor release comes just weeks after the official 1.0 release and includes a number of small features and improvements. See the project [CHANGELOG.md](https://github.com/drone/drone/blob/v1.1.0/CHANGELOG.md#110---2019-04-23) for more details. Notable features are highlighted below.

# Breaking Changes

_None._

# Organization Secrets

Organization secrets can be used to provide global secrets to all repositories in an organization, and can simplify secret management. Organization secrets are managed from the command line. _Note that we plan to add secret management screens to the user interface in the coming weeks._

```
drone orgsecret add [command options] [organization] [name] [data]
```

Example organization secret commands:

```
$ drone orgsecret add github docker_username octocat
$ drone orgsecret add github docker_password swordfish
$ drone orgsecret list github

docker_password 
Organization:       github
Pull Request Read:  false
Pull Request Write: false

docker_username 
Organization:       github
Pull Request Read:  false
Pull Request Write: false
```

Example configuration using global organization secrets:

{{< highlight yaml "hl_lines=12 14" >}}
---
kind: pipeline
name: default

steps:
- name: publish
  image: plugins/docker
  settings:
    repo: hello-world
    tags: latest
    username:
      from_secret: docker_username
    password:
      from_secret: docker_password

...
{{< / highlight >}}

# Cron Filters

Cron filters are used to limit execution of a pipeline or pipeline step based on the cron job that triggered the build. This is useful when you need different execution logic per cron job.

{{< highlight yaml "hl_lines=18-19 31-32" >}}
---
kind: pipeline
name: default

steps:
- name: test
  image: golang
  commands:
  - go test -v
  - go build

- name: deploy
  image: plugins/docker
  settings:
    repo: hello-world
    tags: latest

trigger:
  cron: [ weekly ]

---
kind: pipeline
name: default

steps:
- name: test
  image: golang
  commands:
  - go test -v

trigger:
  cron: [ nightly ]

...
{{< / highlight >}}

# Final Worlds

Give [Drone](https://docs.drone.io/installation) a try today. You can get up and running in less than 10 minutes. If you have any questions or need assistance please [get in touch](https://discourse.drone.io).

---

_With 18,000 github stars and a robust community, Drone has been at the forefront of container-driven workflows. Drone is empowering development teams to deliver software at unprecedented rates. Follow us on Twitter [@droneio](https://twitter.com/droneio) or on [Github](https://github.com/drone) for news and product updates._