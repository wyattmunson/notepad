---
title: "Creating Dockerfiles"
description: ""
summary: "Reference for writing Dockerfiles to build Docker images."
lead: ""
date: 2023-01-04T00:27:10-08:00
lastmod: 2023-01-04T00:27:10-08:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "creating-dockerfiles-0ac91621d7f6d108733555d3847ab234"
weight: 999
toc: true
---

Dockerfiles are the instruction set to create a Docker Image.

## Dockerfile Basics

Dockerfiles are named `Dockerfile` with no extenstions.

Dockerfiles are built with the `docker run` command.

### Dockerfile crash course:

- Dockerfile starts with a `FROM` command
- Dockerfile ends with a single `CMD` or `ENTRYPOINT` command to be executed when the container starts
  - `ENTRYPOINT` - use for a command that should always be executed when the container starts
  - `CMD` - use for a command that can be easily overriden when the container starts
- `RUN` commands are executed when the Dockerfile is built (used for downloading packages, installing applications, ect.)
- `EXPOSE` are used to expose port for process inside the container
- `COPY` files from the local file system to the container's file system
- `WORKDIR` set working directory execute commands from

### Simple Dockerfile

```Dockerfile
FROM node:12.4
COPY . /app
RUN make /app
CMD node /app/main.js
```

Each instruction forms one layer:

- `FROM` - create layer from `node:12.4` Docker image
- `COPY` - move files into Docker directory
- `RUN` - kick off build process using `make`
- `CMD` - specify command to run within container

### Environment Variables

Can be supplied on run

```bash
docker run -e MY_VAR=hello -it ubuntu:latest bash
```

Can be supplied with `.env` file

```env
DB_HOST=localhost
DB_PASSWORD=secret
```

```bash
docker run --env-file .env -it ubuntu:latest bash
```

## Dockerfile Instruction Reference

### FROM

{{< callout icon="🔍" text="Define source image for Dockerfile; mandatory starting command" />}}

This is the first command in Dockerfile.

```bash
# pull files from Dockerhub registry
FROM alpine
FROM alpine:latest
FROM node:19.12-slim
```

{{< callout icon="💡">}}
See [Docker Hub](https://hub.docker.com/search?q=) for all available images
{{</callout>}}

---

### RUN

{{< callout icon="🔍" text="Executes a command while building the image" />}}

Executes the commands as a new layer during the building of the image (`docker build`).

This is used for things like downloading packages or installing applications during the building of the image.

There can be and often are multiple `RUN` commands in a single Dockerfile.

```Dockerfile
RUN echo "This happens during the build"
RUN npm install

RUN ls -lah &&\
  echo "multi line run statement"
```

---

### CMD

{{< callout icon="🔍" text="Executes a command while running the image; easily overriden" />}}

This is the default command executed when running a container (`docker run`); it is not executed during the `docker build` phase when the image is built.

For example, if the image was a webserver, the `CMD` command would start the server when the container is run.

- It will execute by default, and can be overridden with `docker run`, unlike `ENTRYPOINT`
- Dockerfile's should only include one `CMD` instruction at the end of the file
- `CMD` is used for when a container is started (not during the `docker build` phase)

The `CMD` command is desgined to be easily overridable.

```Dockerfile
# Shell form
CMD echo "Hello World"

# exec form
CMD ["echo", "Hello World"]
```

The `CMD` command is overridden when another command is specified with `docker run`.

```
# sample Dockerfile for sample-container
FROM alpine
CMD ["echo", "Hello World"]
```

```bash
# file running with default `echo Hello World`
docker run sample-container
=> Hello World

# overriding `echo Hello World` with `uname` command
docker run sample-container uname
=> Darwin
```

---

### ENTRYPOINT

{{< callout icon="🔍" text="Executes a command while running the image; cannot be overriden" />}}

The `ENTRYPOINT` is used as the executable for the container, executed when running a container (`docker run`); it is not executed during the `docker build` phase when the image is built.

- It will always execute when the container is run, unlike `CMD` which can be overridden with the `docker run` command.
- Dockerfile's should only include one `ENTRYPOINT` instruction at the end of the file
- `ENTRYPOINT` is used for when a container is started (not during the `docker build` phase).

Commands can be passed from `CMD` to `ENTRYPOINT`

```Dockerfile
CMD ["-v"]
ENTRYPOINT ["node"]
```

If additional arguments are added with `docker run`, the `ENTRYPOINT` is not ignored. It will always execute.

```bash
# sample Dockerfile
FROM alpine
CMD ["echo", "Hello World"]
```

```bash
# file running with default `echo Hello World`
docker run sample-container
=> Hello World

# `echo Hello World` is not overridden with `uname` command
docker run sample-container uname
=> HelloWorld Darwin
```

---

### WORKDIR

Set the working direcotry for all subsequent commands. Multiple `WORKDIR` commands can be used to update the working direcotry.

```Dockerfile
# set working dir to "/opt/north" for subsequent commands
WORKDIR /opt/north
RUN make north-app

# change working dir to "/app" for subsequent commands
WORKDIR /app
RUN ./app
```

---

### EXPOSE

Exposes a port for a process inside the container.

For exmaple, if a Node app is running inside the container on port `5000`, you can expose that port.

```Dockerfile
EXPOSE 5000
```

---

### ENV

{{< callout icon="🔍" text="Variables available during the build phase and running container" />}}

Set environment variables that can be used during the building of an image or inside a running container.

- Typically API keys, database connection strings, secrets, ect.
- They can be used for building the image or for a running container
- Environment variables specified in the Dockerfile are overridden by the `docker run -e` command
- Contrast with `ARG` where vaiables can be used in the build phase only

Set environment variables in a Dockerfile:

```Dockerfile
# set "workingdirectory" variable to "/opt/north"
ENV workingdirectory /opt/north

# consume "workingdirectory" variable
WORKDIR $workingdirecotry

# set build arg
ARG NODE_VERSION=18
# set env variable equal to build argument
ENV ENV_NODE_VERSION=$NODE_VERSION
```

Pass environment variables with `docker run`:

```
docker run -e USERNAME=wyatt -e PASSWORD=hunter2 nginx
```

---

### ARG

{{< callout icon="🔍" text="Variables only available during the build phase" />}}

Build argments are parameters only available during the build phase.

- Typically package version numbers or other details relevant to the build
- They are not available when the container is running or once the image is built
- Build arguments can be specified in the Dockerfile or the `docker build` command
- Contrast with `ENV` where vaiables can be used in the build and run phase

```Dockerfile
ARG NODE_VERSION=18.04

RUN echo "Node version is ${NODE_VERSION}"
```

Pass environment variables with docker run:

```
docker build --build-args USERNAME=mike --build-args PASSWORD=bestpassword .
```

---

### COPY

Copy files from local machine to the container's file system.

```Dockerfile
# format
COPY location/location/machine ./container/file/system

# sample
COPY package.json .
```

---

### ADD

Add files or

```Dockerfile
ADD server.js .
```

---

### LABEL

Add metadata to an image.

```Dockerfile
LABEL "author"="Greg Benish"
```

## Docker Concepts

### Shell Form vs Exec Form

The two forms are different.

#### Shell Form

```bash
INSTRUCTION COMMAND

RUN apt-get update -y
COPY ./some/directory
CMD echo "Hello Local Host"
```

#### Exec Form

```bash
INSTRUCTION ["EXECUTABLE", "PARAM_1", "PARAM_2"]

RUN ["apt-get", "update", "-y"]
COPY ["./some/directory"]
CMD ["echo", "Hello Local Host"]

# propmt shell processing to substitute variable
ENV tester Greg
CMD ["echo", "-c", "Hello $tester"]
```

### `CMD` vs `RUN` vs `ENTRYPOINT`

Containers are built to run a single process. When that process completes the container exits.

`RUN` is used to run commands like downloading software packages and installing applications. These are executed when the image is built.

`CMD` and `ENTRYPOINT` are the two commands that define what process is running in the container. These are executed when the image is run.

- `RUN` - executing a command and creates a new layer. Do something like installing software package
- `CMD` - default command to execute when running a container without supplying a command. It can be easily overriden.
- `ENTRYPOINT` - specify when making a container an executable. This is only overriden by the `--entrypoint` flag.
- `CMD` and `ENTRYPOINT` - combine when using a container with an executable that needs easily overrideable default values.

### Using `ENV` and `CMD` together

How to use environment variables in a `CMD` command.

```bash
# docker run command (with env variable)
$ docker run -e URL_PATH='example.com'
```

```Dockerfile
# Dockerfile
ENV URL_PATH $URL_PATH

CMD ["sh", "-c", "sh ./script.sh ${URL_PATH}" ]
```

- Dockerfiles are named `Dockerfile` (with no extension)

## Building an Image from a Dockerifle

```bash
# build Dockerfile in current directory
docker build .

# add tag to image
docker built -t vendor/image:0.0.1 .

# add build-time arguments
docker build --build-arg KEY=VALUE .
# multiple build-time arguments
docker build --build-arg KEY=VALUE --build-arg KEY=VALUE .
```
