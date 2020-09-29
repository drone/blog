---
date: 2020-05-26T00:00:00+00:00
title: Drone Integrates with MacStadium, Launches Cloud-Native Continuous Integration for Mac and iOS
author: mattnorrisdev
image: https://storage.pardot.com/563672/62751/MacStadium_Macmini_datacenter_screen.jpg
weight: 1
tags: [ Announcement, Product Updates ]
url: /continuous-integration-for-mac-osx
---

Drone has always enabled your busy team to deliver software quickly and securely. Now we're bringing Drone's simple Continuous Integration & Delivery experience to Apple developers everywhere.

Drone has integrated with [MacStadium](https://www.macstadium.com/), the only provider of enterprise-class cloud solutions for Mac and iOS app development. The two create a remarkable cloud native experience.

# From Closet to Cloud

Apple hardware is beautiful, but you still need to manage it.

"How many devices will we need? Can we amortize them? Where do we put them? Who is the admin? I need to access one. Do you have the closet key? What's that vnc path again?"

With Drone and MacStadium, your Mac and iOS infrastructure is:

* Cloud native
* Dedicated and fully managed
* Automatically scaled
* Orchestrated by Kubernetes using [Orka](https://www.macstadium.com/orka)
* Private and secure

# Works on my machine. And yours.

The benefits of Drone with MacStadium aren't exclusive to Mac and iOS development. As a web developer, you want to be sure your code works on all browsers - desktops, laptops, and mobile. As a conscientious developer, you know testing many configurations now saves time and trouble later. Use Drone to test, verify, and gain peace of mind.

# Example

A Drone [pipeline](https://docs.drone.io/pipeline/overview/) is a YAML file. Its simple syntax enables any teammate to understand your Continuous Delivery process.

Create a [MacStadium pipeline](https://docs.drone.io/pipeline/macstadium/overview/). Commit it to your repository.

```
---
kind: pipeline                                        
type: macstadium
name: default

settings:
  image: MojaveWithXcode.img

steps:
- name: install
  commands:
  - pod --version
  - pod install

- name: test
  commands:
  - xcodebuild -version -sdk
  - X build test
```

Drone executes a pipeline every time you commit code. In this example, the pipeline uses [Orka](https://www.macstadium.com/orka) to provision a new MacStadium virtual machine. It then builds, tests, and distributes your code at scale. After the pipeline completes, it destroys the virtual machine. This ensures a clean run next commit.

# Try it Now

Give [Drone](https://docs.drone.io/installation) a try today. You can get up and running in less than 10 minutes. If you have any questions or need assistance please [get in touch](https://discourse.drone.io/).


_With 21,000+ GitHub stars and a robust community, Drone is a leader in container-driven workflows. Drone empowers development teams to deliver software at unprecedented speed. Follow us on Twitter [@droneio](https://twitter.com/droneio) or on [Github](https://github.com/drone/drone) for news and product updates._