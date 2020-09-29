---
date: 2018-10-31T00:00:00+00:00
title: Why We Chose Drone to Support Our 600 Deployments to Production Each Month
author: fbcbarbosa
guest: true
draft: false
weight: 1
tags: [ Community, Interviews ]
---

[QuintoAndar](https://www.quintoandar.com.br/) is an app for landlords and renters tired of the bureaucracy of the traditional apartment rental process. The Brazilian startup screens apartment hunters, acts as a guarantor for those with solid credit histories and eliminates middlemen. Founded in 2012 by two Stanford MBA Graduates, QuintoAndar is now present in 15 cities in Brazil, including São Paulo and Rio de Janeiro.

### From Monolithic App to Microservices

Our product was born in the cloud, but in the early years scaling our systems was not a huge concern. A single monolithic application deployed by a lonely Jenkins server did the job just fine. Over time we split the monolith into a small number of Amazon Elastic Beanstalk environments. However, by 2016 the number of rentals as well as our engineering team had grown considerably. It started to become difficult to have several teams collaborating on a few, large codebases without slowing down development. This led us to begin breaking down our applications in many different domains and trying out microservices, eventually adopting Mesosphere DC/OS to orchestrate our containers.

## Replacing Jenkins with Drone

At this point we required a new continuous integration and deployment solution, one that was well adapted to our new distributed architecture. One of the big issues we had with Jenkins is that it required a lot of configuration to achieve what modern tools just gave us out of the box. With Jenkins, from a clean installation, configuring new pipelines was cumbersome and since they are not versioned we even had a few incidents where we lost some important job definitions. The lack of isolation between environments and the fact that there was not a simple way to scale up the number of agents were other deal breakers. We also thought that Jenkins had a suboptimal plugin system, which would sometimes break due to conflicts and upgrades.

Drone (which was at the 0.4 release at the time) seemed to solve most of these problems, and it was easy to set up. It embraced pipeline-as-code and isolation between environments was a given, since every step was running in a Docker container. Finally, when we upgraded to Drone 0.5, adding new agents became trivial. The plugin structure also made it easy to design a robust deployment solution for our DC/OS clusters in the form of our [marathon plugin](https://github.com/quintoandar/docker-drone-marathon). As a matter of fact, this simplicity in creating a plugin that could be shared across pipelines was one of the reasons we felt we could make the jump to DC/OS.

A few months ago we started a large project to migrate most of our infrastructure to a Kubernetes cluster. This time we developed an internal Helm plugin (we have not made it public because right now it has some custom authentication logic), which again allowed us to migrate all our deployments from DC/OS to Kubernetes just by changing a few parameters in the deployment step of each pipeline.

## 60 Engineers Using Drone, 600 Deployments Every Month

Today we have over 60 engineers interacting with Drone daily, and they really like it, due to its simplicity. We’ve always had a small DevOps team, so we tend to heavily favor solutions that don’t require a lot of maintenance overhead, and Drone fit the bill. Since most developers have at least a basic understanding of Docker, they hardly need any help maintaining their pipelines, and there have been a couple of instances of developers creating their own plugins! This way our team can focus on exciting upgrades to our infrastructure, even as teams regularly update their deployment pipelines and new projects are created every month.

Now we're running a Drone 0.8.5 cluster on Amazon ECS. We’ve heard that Drone runs very well on Kubernetes and in the future we will certainly try it out on Kubernetes. We make about 600 deployments to production every month, and just in September alone we received over 5000 build events. We’ve found that it’s best to run each agent in a dedicated machine to guarantee the best performance for individual builds, and it has helped to keep most of our deployment pipelines under 10 minutes. However, that can scale up the costs, so we’ve been eyeing the [Drone Autoscaler](https://autoscale.drone.io/) for a while, but still haven’t got around trying it out in production. Soon. 

## Future Drone Features

I’m personally really excited for the next 0.9 release and the [Configuration Plugins](https://github.com/drone/drone-config-plugin-starter), as one of the greatest demands from our engineers is how to run parts of a pipeline only when certain files change. We also have the [Vault integration](https://blog.drone.io/drone-vault-secrets/) on our radar, as secret management is probably the only area where we still have some small overhead, since right now we lack an easy way to share secrets across among multiple repositories. We look forward to all of the updates in the next release as we continue to help our development team ship code quickly. 

_Fernando Barbosa is a DevOps Engineer at QuintoAndar, and an avid user of Drone. Follow Fernando on Twitter @fbcbarbosa or on Github [here](https://github.com/fbcbarbosa), and read more from QuintoAndar Engineering team at the [QuintoAndar Tech Blog](https://medium.com/quintoandar-tech-blog). If you’re interested in sharing how you are using Drone please email us [here](mailto:hello@drone.io)._
