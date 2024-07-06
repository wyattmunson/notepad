---
title: "Docker CLI Commands"
description: ""
summary: "Docker CLI command reference for building and running images, and managing Docker"
lead: ""
date: 2023-01-04T00:19:53-08:00
lastmod: 2023-01-04T00:19:53-08:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "docker-cli-commands-38307441e95adc87bca230d8426ade2e"
weight: 5
toc: true
---

The Docker CLI is used to communicate with the Docker Engine to build, run, and maintain images.

## Basic Commands

```bash
# View all docker images saved locally
docker images

# Download a docker image onto local machine
docker pull <IMAGE_NAME>:<IMAGE_TAG>
docker pull library/nginx:latest

# View all running containers
docker ps
```

### Build Docker Image

Use `docker build` to build a Docker image from a Dockerfile, from a specified location (`.` specifies the current directory).

{{< callout context="tip" >}}
Docker images file the syntax `<VENDOR>/<REPO>:<TAG>`.
{{< /callout >}}

- `<VENDOR>` - the organization, company, or user. Set to the DockerHub username if pushing to that location.
- `<REPO>` - the repository or application name for this image. All versions of an application share the same repo name.
- `<TAG>` - reference to the version number or specific image.

```bash
# build a docker image
docker build .

# build docker image and specify tag
docker build . -t munsonwf/express-sample:1.2.3

# feed env variables in a command line
docker build . --build-arg DB_PASSWORD=hunter2
```

### Run Docker container

```bash
# run docker container (using image name and tag)
docker run <IMAGE_NAME>:<IMAGE_TAG>
docker run alpine:latest

# run docker container (using container Id)
docker run <CONTAINER_ID>
docker run 183582d52a03
docker run 18

# run with env vars
docker run -e DB_PASSWORD=hunter2 -it ubuntu:latest some-command

# exec into running container
docker exec -it <CONTAINER_ID> <COMMAND>
docker exec -it fe93b2ca2 /bin/bash

# run exec against stopped container
docker run --rm -it <REPO_NAME> <EXEC_COMMAND>
docker run --rm -it gitlab/gitlab-runner:2.0.3 bin/bash

# run exec against stopped container (override entrypoint)
docker run --rm -ti --entrypoint='' munsonwf/nslookup:1.0.0 /bin/ash
```

### Logs from Docker container

```bash
# View container logs and tail output
sudo docker logs CONTAINER-NAME --follow

sudo systemctl start docker
service docker status

# install docker compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# add docker compose to path for sudo by creating symlink
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

```

### Take Snapshot of Container

```bash { title="Take snapshot of running container" }
# take snapshot of running container (get id with `docker ps`)
docker commit <CONTAINER_ID>
```

### Tag Docker Image

```bash { title="Tag image" }
# tag docker image (get id with `docker images`)
docker tag <IMAGE_ID> <NEW_NAME>
```

### Inspect Running Container

```bash { title="Inspect running container" }
# inspect container (see entrypoint, CMD)
docker inspect <IMAGE ID>
docker inspect 2bf89faaab70
```

### Inspect Network Configuration

```bash { title="Inspect network configuration" }
# list docker networks
docker network ls

# inspect given docker network
docker network inspect <IMAGE ID>
docker network inspect 74e
```

### Push to DockerHub

```bash
# login to docker
docker login

# push sepcific image and tag to docker push
docker push vendor/app:ver
```

### Build for different platforms

The platform for an M1 MacBook is not the same as the Linux container it may be running in on the cloud.

```bash
docker buildx build --platform=linux/amd64 -t <image-name> .
```
