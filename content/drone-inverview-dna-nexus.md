---
date: 2018-12-05T00:00:00+00:00
title: An Interview with DNAnexus - Using Drone to Handle Multiple Petabytes of DNA information
author: bradrydzewski
weight: 1
tags: [ Community, Interviews ]
---

_We interviewed Dave Cadwallader, Site Reliability Architect; Steve Osazuwa, Bioinformatics Scientist; Bill Agee, SDET Team Lead and Maciej Adwent, Principal Software Engineer of DNAnexus about why they chose Drone as their CI/CD platform to handle multiple petabytes of DNA information._

__How does DNAnexus use Drone today? Does it support any public facing applications?__

When we started using Drone about a year ago, our most immediate need was to catch bugs earlier in the development process.  We set up Drone as a “quality gate” in our Github Pull Request flow, blocking the merging of pull requests to the master branch until they had passed a required series of tests.  This was great for catching mistakes like unit test failures, code syntax mistakes, etc.  

Because some of our customers are government agencies, we also have strict regulatory requirements around security.  We were able to containerize the static analysis tools we use for code security and dependency vulnerability scanning and place them within our Drone pipelines.  This allows us to catch and block security issues alongside coding errors using the same process.

Before we adopted Drone, we would often wind up with easily-preventable regressions that landed in the master branch.  This required a rotating weekly person acting as a “build cop” to track down the person who introduced the regression and ask them to fix it.  Now with Drone, we catch most of these trivial issues before a pull request lands on master. The QA community has a great name for this movement towards earlier testing: “shift left”.  On a left-to-right timeline from dev to production, Drone makes it super easy for us to shift as much testing to the left as we want.

__Why was Drone the best choice for what you are trying to accomplish? Did you look at any other CI/CD companies, and if so why did you choose Drone?__

We’re in the process of modularizing a monolithic application and breaking it into independently deployable docker containers.  As we evaluated different CI/CD solutions, the “container first” architecture of Drone was a natural fit.  Many people in our team have a background of working with other CI solutions that offered Docker support as a bolted-on afterthought, and there were often rough edges and lots of work required to get it working correctly.  On the contrary, with Drone, the work involved to set up a pipeline for building and deploying containers is so simple that at first, we were sure we had missed something!  The time savings we’ve gained from using Drone for a container-first CI/CD solution has allowed us to focus our time on other more exciting work.

__What results have you seen since using Drone?__

In addition to the benefits we’ve seen with improved code quality, and easy-to-understand process for building and publishing containers, one of the best benefits we’ve seen is that Drone promotes a culture of “tinkering”.  Speaking from experience with other CI/CD solutions that are more fragile, and prone to completely breaking at the smallest change, Drone is designed to resilient against changes in one place breaking things in another.  

Because each job, and each plugin, runs in its own isolated container, this allows engineers to experiment with new types of jobs, and new custom plugins, without needing to worry about how their experiments might affect others using the system.  We’ve written our own custom plugins to interface with other systems in our organization, and we’ve never had a time when we accidentally broke something for someone else.  

And since Drone’s declarative pipeline definitions live under source control, this promotes the concept of experimenting with pipeline changes within the comfort of a pull request, merging the results to master for others to use only when the changes are tested and ready.

__How many times do you deploy to Drone a day or week or month?__

We’re still a small shop with a few dozen engineers.  But each week we see on the order of hundreds of pull request commits verified, and dozens of containers built and published using Drone.  We’re not yet using Drone to actually deploy code into production but that’s something we hope to accomplish in the next few months.  

__What plug-ins do you use today?__

We use the docker plugin for seamless building and publishing of our docker images.

We use the Google Hangouts Chat plugin to alert developers about build failures, and newly-published docker images being available.  

We use the s3 plugin to upload security and quality reports for long-term storage, for compliance purposes.

__Are there any special considerations for Drone in the genomics industry?__

Genomics data is considered Personal Health Information (PHI).  Any system that handles PHI is subject to strict laws, both in the US and abroad.  Since the services that we build with Drone are in charge of handling multiple petabytes of DNA information, having a reproducible and auditable CI/CD system is essential.  Drone’s container-first architecture allows us to strictly isolate jobs from one another.  Since each job runs in an isolated and disposable container that’s recreated from scratch every time, that reduces the surface area in our CI/CD pipeline that could be exploited by an attacker, or corrupted by a runaway adjacent process.

Most importantly, Drone’s container-first architecture has accelerated our vision of “immutable infrastructure”.  Instead of rebuilding services each time we redeploy to a new environment, Drone has allowed us to move towards having unmodifiable, signed-tested-and-sealed docker images that we merely “promote” from one environment to another.  From a regulatory perspective, this greatly simplifies our quality management procedures.

Emphasizing an earlier point, Drone promotes a culture of “tinkering”. Often-times, Scientists will want to alter old applications and established workflows with new tools or ideas. We leverage Drone’s quick turnaround time and intuitive user experience to continuously bridge the gap between theory and practice. 

__What new features are you looking forward to seeing in future versions of Drone?__

More parallelization features like distributed pipelines and better flow control over distributed tasks. We’ve got some very big Drone pipelines that could shrink from an hour or two to mere minutes if we were able to distribute tasks smartly and combine their results.

Also, from a regulatory perspective, we’d love to see more features for navigating and searching build logs and metadata in the Drone UI. Drone’s easy-to-use database support already makes it possible to search for build information via database queries, but in the future, a built-in UI for querying builds would be a game-changer. The ability to quickly query historical CI build data is useful in situations where CI jobs are included in regulatory audits, and Drone has the opportunity to set itself apart from other CI tools by making that process even smoother.

We're also quite excited to see Drone extending its support for Windows containers. This will be advantageous for us since we build a set of CLI applications for Windows, and using Drone to containerize our Windows build and test pipelines will result in reduced CI infrastructure costs as well as the usual benefits of running Drone pipelines during development.


_Drone is container-native CI/CD with support for multi-cloud and multi-architecture workloads. If you’re interested in sharing how you are using Drone please email us [here](mailto:hello@drone.io)._