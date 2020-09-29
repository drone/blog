---
date: 2018-12-07T00:00:00+00:00
title: Drone CI/CD Goes Kubernetes-Native
author: bradrydzewski
draft: false
weight: 1
tags: [ Announcements, Product Updates ]
image: /images/drone_kubernetes_logs.png
---

Today we’re announcing official support for Kubernetes. While many organizations have already been combining Drone and Kubernetes for their deployments, today we’re delivering a native integration for a better user experience. Now your CI/CD Pipelines are translated into native Pods, Secrets, and Services.

We’ve been at the forefront of the containerization movement; we started support for Linux containers, and when Docker came around we fully embraced their container runtime. Since then we’ve seen the hyper growth of Kubernetes platforms and users like [eBay](https://blog.drone.io/interview-with-punit-agrawal-devops-at-ebay/), [Reddit](https://www.reddit.com/r/sysadmin/comments/9x577m/were_reddits_infrastructure_team_ask_us_anything/e9pnrle/) and [The New York Times](https://open.blogs.nytimes.com/2017/01/12/continuous-deployment-to-google-cloud-platform-with-drone/) adopting Drone and Kubernetes together. We’ve heard from many of you, wanting a simpler, better experience with Kubernetes, we’ve listened and believe you’ll love the new service. 

We’re happy to fully support Kubernetes today, and wherever this movement and users needs are in the future. We look forward to hearing your feedback.

# What is Drone?

_If you are learning about Drone for the first time ..._

Drone is a modern CI/CD platform built with a containers-first architecture. Pipelines are configured using a special [YAML file](https://docs.drone.io/config/) that you check-in to your git repository. The syntax is designed to be easy to read and expressive so that anyone using the repository can understand the continuous delivery process.

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

# How does Drone integrate with Kubernetes?

Drone integrates seamlessly with your version control system to receive a notification any time a user opens a pull request, pushes code, creates a tag and more. Upon notification, Drone creates a Kubernetes Job to execute your Pipeline.

This Kubernetes Job spawns a Pipeline Controller container. This container is responsible for orchestrating your Pipeline steps, collecting the results, and reporting back to the Drone server.

Kubernetes Jobs are not automatically purged from the system. This ends up being a really nice side-effect. If you are a Drone operator and are responsible for managing Drone as a shared service in your company, this gives you the ability to inspect the Job for troubleshooting purposes. For automated cleanup enable the TTLAfterFinished feature gate.

![kubernetes jobs](/images/drone_kubernetes_jobs.png)

As previously mentioned, the Pipeline Controller is responsible for orchestrating your Pipeline steps. The Controller creates a new Namespace for your Pipeline, and creates all Pipeline objects within this namespace. This ensures your Pipeline is isolated from other Pods and Services running on your cluster, and allows for easy cleanup when execution is complete (_credit to Kelsey Hightower for this suggestion_). You can explore the namespace and watch your Pipeline running in real-time.

In the screenshot below we can see the log output of a Pipeline step in both Drone and the Kubernetes Dashboard.

![kubernetes jobs](/images/drone_kubernetes_logs.png)

# How is this different from Drone for Docker?

There is no visible difference from an end-user perspective! The YAML configuration file is portable across container and orchestration engines. You can use the exact same Pipeline configuration with Kubernetes and with Docker runtimes.

From an administrator perspective, if you are running Drone on Kubernetes today, this will significantly simplify your deployment. Perhaps the biggest improvement is you no longer have to install, upgrade and manage agents. Drone for Kubernetes is agentless! It also means no more mounting the host machine Docker socket or running Docker in Docker.

# Is Drone for Kubernetes Production Ready?

The Kubernetes Runtime is still considered experiment, however, initial testing has been very positive. There are some known [issues](https://github.com/drone/drone-runtime/issues) and areas of improvement, however, I expect rapid progress over the coming weeks. If you are interested in contributing to our Kubernetes Runtime, I have put together a [small guide](https://discourse.drone.io/t/contributing-to-drone-for-kubernetes/3159) to help you get started.

<!-- # What are the benefits of Drone on Kubernetes?

One benefit of our Kubernetes-Native runtime is the elimination of the Agent. Previous versions of Drone required you to install an agent on every server node. 

# Untapped Potential

__Scheduling__

Drone has always had a very simple scheduling system, routing builds to agents based on simple concurrency limits. I see a lot of potential to take advantage of Kubernetes scheduling features, including resource allocation and node affinity, leading to more efficient resource allocation and server utilization.

__Autoscaling__

Drone already provides an [autoscaler](https://autoscale.drone.io/) that provisions server instances, however, these capabilities are rather primitive compared to 

__Multi-Cloud__
 -->

---

Take Drone for a spin! We recently launched [Drone Cloud](https://cloud.drone.io) which is free for the open source community, thanks to Packet who donated blazing fast x86, Arm32 and Arm64 bare metal servers. 

_Drone.io has been at the forefront of how to deploy with containers and the first to deliver a container CI/CD model. Drone.io has a robust ecosystem of plug-ins such as Kubernetes, Helm, GitHub and more, to help developers with their pipelines. With 16,000 GitHub stars and a passionate community, Drone.io is empowering developers to quickly fix bugs and deliver new applications to market faster. Drone.io is pluggable, repeatable and portable by nature, putting the power of deployment in developers hands. Follow us on Twitter [@droneio](https://github.com/drone/drone)._
