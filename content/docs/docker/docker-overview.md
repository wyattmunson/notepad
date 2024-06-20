---
title: "Docker Overview"
description: ""
lead: ""
date: 2023-01-04T00:17:04-08:00
lastmod: 2023-01-04T00:17:04-08:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "docker-overview-965d581a1dd6844827e9424e95219ede"
weight: 1
toc: true
---

Docker is a OS virtualization software. Similar to virtual machines in their outcome, their differ in their function.

### What's it purpose

The general idea is to promote interoperability. If a Docker container runs on your local machine, it should run on someone else's machine, or a Kubernetes cluster, or Elastic Container Service cluster.

### What problem it solves

Let's say your developing a Node.js application running Node v12 on your computer. If someone else tries to run said program, but they're running Node v8, it may not run correctly.

With Docker, the version of Node is built directly into the Docker image. So if we're using the same Docker image, we're using the same Node version.

In this case what you want to do is run the Node.js application, not spend time figure out dependencies and ensuring your "development evnironment" is set up to run the Node.js application.

### Container vs Images

A Docker Image is like a CD; it's the instructions.

- Docker images are built with `docker build`
- Docker images are downloaded to a machine with `docker pull`

A container is a running version of the image.

- Docker images are started with `docker run`

### Who created Docker

Docker is created by Docker, Inc. (see: [recursion](#who-created-docker))

### Docker vs. VMs

In short:

- VMs abstract the hardware layer so you can focus on the OS level
- Docker abstracts the OS level, so you can focus on the app
