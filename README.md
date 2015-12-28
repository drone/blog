[![Build Status](http://beta.drone.io/api/badges/drone/blog/status.svg)](http://beta.drone.io/drone/blog) [![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/drone/drone?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

Blog for the [Drone](https://github.com/drone/drone) continuous integration server, found at [blog.drone.io](http://blog.drone.io). We would love to start accepting guest blog posts, so if you are interested please send us a message on Gitter to discuss.

## Setup

This website uses the [Hugo](https://github.com/spf13/hugo) static site generator. If you are planning to contribute you'll want to download and install Hugo on your local machine.

Install on OSX with brew:

```
brew install hugo
```

Install using the Go tool:

```
go get github.com/spf13/hugo
```

Or follow these instructions for your platform: http://gohugo.io/overview/installing/

## Build

Generate the website and output to `./public`

```
hugo -t drone
```

Generate the website, serve on localhost:1313, and automatically refresh the browser when a change is detected:

```
hugo server -w -t drone
```
