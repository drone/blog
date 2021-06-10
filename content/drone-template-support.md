---
date: 2021-06-10T00:00:00+00:00
title: Announcing Configuration Templates
author: eoinmcafee
draft: false
weight: 1
tags: [ Announcements, Product Updates ]
---
Today Drone.io, the leading open source cloud native continuous integration (CI) and continuous delivery (CD) platform, 
is announcing the official support for configuration templates.

# Get Started using Drone.io with configuration templates

Organizations are having difficulty maintaining large amounts of configuration files, especially when those configuration files are very similar across projects. A pain point is needing to change a docker image (for example) and having to update configuration files in hundreds of repositories. Configuration templates that can be shared across projects.

This will simplify configuration management at organizations that have large numbers of similar configuration files; it will reduce complexity of yaml files in individual repositories and will simplify management of configuration across repositories.

Get started by reading the documentation found at: https://docs.drone.io/template/

The first step is to define your configuration template, be it Jsonnet or Starlark. Both guides can be found in the docs
however for this blog I will be demoing a jsonnet template.
```
local stepName = std.extVar("input.stepName");
local image = std.extVar("input.image");
local commands = std.extVar("input.commands");
{
  "kind": "pipeline",
  "type": "docker",
  "name": "default",
  "steps": [
    {
      "name": stepName,
      "image": image,
      "commands": [
        commands
      ]
    }
  ]
}
```
It's important to note that all custom variables must start with 'input.' 

Once you are happy with the template, add it to the drone database using either:
- Drone CLI - https://docs.drone.io/cli/template/drone-template-add
- Drone API - https://docs.drone.io/api/templates/template_create

The next step is to define your .drone.yml
```yaml
kind: template
load: plugin.jsonnet
data:
    stepName: my_step
    image: my_image
    commands: my_command
```
- load refers to the template name you've just created
- data refers the set of freeform template inputs

Once you've pushed the updated .drone.yml file you each of your repositories, you're all set to go. 

_Drone is modern CI/CD. Container-native and available via open source and an enterprise on-prem edition. [Try it today](https://readme.drone.io)._