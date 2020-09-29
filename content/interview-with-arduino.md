---
date: 2018-09-18T00:00:00+00:00
title: Arduino Uses Drone to Power Microservices on Kubernetes
author: bradrydzewski
weight: 1
tags: [ Community, Interviews ]
---

_An Interview with Luca Cipriani, Matteo Suppo and Edoardo Tenani_

> We love Drone because on Jenkins we could not upgrade the Go version without impacting other projects (since Jenkins doesn't do isolation or Docker by default). But with Drone we can upgrade Go per-project or even per-branch given the container-based approach. Drone is a huge step forward than Jenkins

__For those that don't know, what is Arduino?__

Arduino is an open-source hardware, software, and content platform with a worldwide community of over 30 million active users. The company offers IoT developers, makers, and educators the ability to build smart, connected and interactive devices using affordable technologies. Arduino has powered millions of projects over the years, from everyday objects to complex scientific instruments. Check out [arduino.cc](https://arduino.cc) for more information.

__How does Arduino Use Drone?__

We use it to power our web sites (the main ones being arduino.cc and create.arduino.cc) and APIs, but we would like to expand our use of Drone to other parts of Arduino. In particular, we are looking forward to using Drone to setup hardware test pipelines for our boards.

Thanks to our chat-ops bot that is connected to Drone, the full team (around 10 people) is able to deploy code in the development environment.

__How are you using Drone to power your microservices?__

Right now we have about 15 microservices running on Kubernetes,  and we use Drone to test and deploy them. Our microservices are written in Go, and our pipelines run unit and integration tests, build the artifacts and run Helm to deploy the development and production pods. The deploy step currently involves custom built Drone plugins with required software ( gpg, sops, helm, etc. ) and bash commands to do the deployment.

Our frontend applications are HTML/JavaScript/CSS and we use Drone to run unit tests and deploy as static HTML on our frontend servers.

We also use Amazon S3, Saltstack and Ansible to keep the services updated.

__Why Drone Over Jenkins or Travis CI?__

We like Drone for multiple reasons. Overall Drone is simple and perfect for DevOps - it flows really nicely with a DevOps culture. In addition it has a great interface that is simple and easy to use. We needed the testing pipeline to be included in the code itself, not outside.

Lastly, we love Drone because on Jenkins we could not upgrade the Go version without impacting other projects (since Jenkins doesn't do isolation or Docker by default). But with Drone we can upgrade Go per-project or even per-branch given the container-based approach. Drone is a huge step forward than Jenkins.

__When did you start using Drone?__

We introduced the company to Drone, back when it was Drone 0.3 and have been using it ever since.

__What results have you seen since using Drone?__

Since using Drone we have had more deploys during the day, and we are able to now detect errors at every commit pushed, not just right before the deploy.

We don’t have a fixed release schedule, but generally we deploy in the development environment at least once a day (but often it is multiple times a day). We tend to deploy in our production environment a couple of times a week. 

__How do people get involved with Arduino community if they want to?__

The Arduino community counts millions of active users online, but most importantly a significant part of these people can get involved in their local communities. The best way of getting started with Arduino is either visiting our website (arduino.cc), forum (forum.arduino.cc) and social channels (facebook.com/official.arduino, twitter.com/arduino, instagram.com/arduino.cc/), joining a local Arduino User Group ([https://www.arduino.cc/en/aug/](https://www.arduino.cc/en/aug/)). Another good way of getting started is also checking out Create Project Hub, a community based platform with projects from our community members and Arduino experts ([https://create.arduino.cc/projecthub](https://create.arduino.cc/projecthub)).

__Do you have any advice for people using Drone with Arduino?__

The best advice is to use Drone outside of Arduino, then when you apply it to Arduino it will be extremely simple. With our new arduino-cli [https://github.com/arduino/arduino-cli](https://github.com/arduino/arduino-cli) there is a way to partially test Arduino Libraries and Code by running the tests on a Linux box.

_Drone is modern CI/CD. Container-native and available via open source and as an enterprise on-prem edition. Try it today. Follow us on Twitter [@droneio](https://twitter.com/droneio) or on [Github](https://github.com/drone). Or sign up for our email newsletter to save up to date on all Drone news._
