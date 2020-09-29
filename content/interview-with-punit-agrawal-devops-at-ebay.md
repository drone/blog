---
date: 2018-08-13T00:00:00+00:00
title: An Interview with Punit Agrawal, DevOps Lead @ eBay
author: bradrydzewski
weight: 1
tags: [ Community, Interviews ]
---

__How eBay uses Drone to deploy 200 Services to Kubernetes__

_Punit leads the Devops team for the New Product Development Group at eBay, helping deploy microservices efficiently on Kubernetes. He has been front and center as eBay has transitioned to the public cloud. With the transition, Punit has been focusing on automation technologies to help eBay developer productivity. One of those tools is Drone, where eBay has 100 users using Drone for 200 services, releasing 15,000 times a quarter._

__How did you get into DevOps?__

I’ve been at eBay for five years. Prior to leading the DevOps team for the new product development group, I was working as an application backend engineer. When we started to move to the cloud and when we, as a company, went all in on Kubernetes, I wanted to help eBay with that transition. I was interested in how we would help developers increase their productivity using tools like Kubernetes, docker and containers and how CI/CD could help us with deploying our applications.

__Why did you choose Drone?__

When we started using containers, Drone was the obvious choice. We moved from Jenkins CI/CD to Drone. We did online research and came to the conclusion that Drone would be the best to work with in our cloud native stack. We started out using the open source version but eventually bought an enterprise version because the global secrets feature was valuable to us.

__How do you use Drone today?__

For public facing products, both our eBay [ShopBot Messenger](https://shopbot.ebay.com/) and [Haitao China](https://haitao.ebay.com) are powered by Drone. In addition there are a lot of internal products (about 200 services) using Drone for day to day. To give you a perspective of how much we use Drone, there are 100 developers using Drone, deploying about 15,000 times a quarter. 

__What Drone plug-ins do you use today?__

We use a managed Kubernetes service to host our applications, so we use Drone with Kubernetes and Google Cloud. We are interested in [The New York Times](https://open.blogs.nytimes.com/2017/01/12/continuous-deployment-to-google-cloud-platform-with-drone/) Drone plug-in for [GKE](https://github.com/NYTimes/drone-gke).  We write our scripts in Golang, we encapsulate it in Docker and use Helm to deploy our applications. 

__As an early adopter of Kubernetes, what do you think is the biggest challenge the Kubernetes and cloud native community faces today?__

First off there are a lot of good things about Kubernetes but we knew as we started adding more and more people, we faced scalability challenges. Since Kubernetes was adding new features, at the beginning we were spending a lot of our time educating our developers on the latest changes and also updating the software. Although Helm helps with abstracting some of the Kubernetes concepts from the developers,  I feel the biggest challenge facing the Kubernetes community is to develop canonical standards on its usage.

__What other cloud native projects do you use?__

We also use Istio and Prometheus as well. Almost all of the cloud native projects take time to learn and implement within your environment. It is my experience that none of this is easy, as advertised, but I see the value when our developers can be more productive. 

_Drone is modern CI/CD. Container-native and available via open source and an enterprise on-prem edition. Try it today._
