---
title: "Docker CLI Commands"
description: ""
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

# View all running containers
docker ps
```

### Build Docker Image

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
# run docker container
docker run alpine:latest

# exec into running container
docker exec -it <CONTAINER_ID> <COMMAND>
docker exec -it fe93b2ca2 /bin/bash

# run exec against stopped container
docker run --rm -it <REPO_NAME> <EXEC_COMMAND>
docker run --rm -it gitlab/gitlab-runner:2.0.3 bin/bash

# run exec against stopped container (override entrypoint)
docker run --rm -ti --entrypoint='' munsonwf/nslookup:1.0.0 /bin/ash

# run
docker run -e DB_PASSWORD=hello -it ubuntu:latest some-command
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

```bash
# take snapshot of running image
docker commit <CONTAINER NAME>

# tag image you've created
docker tag <IMAGE_ID> <NEW_NAME>

# inspect container (see entrypoint, CMD)
docker inspect <IMAGE ID>
docker inspect 2bf89faaab70

# list and inspect docker networks
docker network ls
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
