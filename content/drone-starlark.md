---
date: 2019-09-18T00:00:00+00:00
title: Announcing Support for Starlark Scripting
author: bradrydzewski
draft: false
weight: 1
tags: [ Announcements, Product Updates ]
url: /create-pipelines-using-starlark
---

_Starlark is a dialect of Python intended for use as a configuration language. The language is developed by Google for the [Bazel](https://bazel.build/) build tool._

Today we are pleased to announce support for [Starlark](https://digitalocean-runner.docs.drone.io/configuration/) scripting as an alternative to configuring your continuous integration pipelines with YAML. Complex pipelines can be difficult to create and maintain and YAML files can become unwieldy. Starlark brings the full power of a scripting language to help teams create complex pipelines with less hassle.


Example Pipeline using Starlark:

```
def main(ctx):
  return {
    "kind": "pipeline",
    "name": "build",
    "steps": [
      {
        "name": "build"
        "image": "alpine"
        "commands": [
            "echo hello world"
        ]
      }
    ]
  }
```


Starlark scripts can use `functions` to create complex pipelines while reducing repetition and boilerplate. This is especially useful to projects that test multiple operating systems, architectures or software versions:

```
def main(ctx):
  reutrn [
    pipeline("1.13"),
    pipeline("1.12"),
    pipeline("1.11"),
  ]

def pipeline(version):
  return {
    "kind": "pipeline",
    "name": "go-%s" % version,
    "steps": [
      {
        "name": "build",
        "image": "golang:%s" % version,
        "commands": [
          "go build",
          "go test -v",
        ]
      }
    ]
  }
```

Starlark scripts can also use repository and build details in `if` statements to dynamically create pipelines:

```
def main(ctx):
  pipeline = {
    "kind": "pipeline",
    "name": "default",
    "steps": {
      [
        "name: "test",
        "image": "node",
        "commands": [
          "npm install",
          "npm test",
        ]
      ]
    }
  }

  if (ctx.build.source.startswith("feature/") and
      ctx.build.event == "pull_request"):

    pipeline.steps.append({
      "name: "integration",
      "image": "node",
      "commands": [
        "npm run integration",
      ]
    })

  return pipeline
```

# Try It Today

If you would like to try out Starlark scripting please [download](https://docs.drone.io/starlark/overview/) and install the Starlark extension. If you need any help, or if you have any feedback or suggestions for improvement please [let us know](https://gitter.im/drone/drone).


---

Take Drone for a spin! We recently launched [Drone Cloud](https://cloud.drone.io/) which is free for the open source community, thanks to Packet who donated blazing fast x86, Arm32 and Arm64 bare metal servers.