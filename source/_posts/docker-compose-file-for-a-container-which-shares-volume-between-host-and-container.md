---
title: docker-compose file for a container which shares volume between host and
  container
description: Here we want to create a docker-compose file which build a local
  docker image and run the container which share a data volume with host.
date: 2021-07-08 17:46:57
updated: 2021-07-08 17:46:57
tags:
  - docker
  - docker-compose
categories:
  - 技術
comments: true
---
## Introduction

Here we want to create a docker-compose file which build a local docker image and run the container which share a data volume with host. Source code could be found [here](https://github.com/wuhaibo/dockerComposeExample).

## Docker

We create here a very simple python container which writes to a file every second.  

```docker
# syntax=docker/dockerfile:1
FROM python:3.7
WORKDIR /app
COPY . /app
CMD ["/usr/local/bin/python","/app/main.py"]
```

## Docker-Compose

In docker compose we simply create a service called monitor and a data volume shared between host and container. 

```
version: "2.0"
services:
  monitor:
    container_name: docker-compose-example-container
    build: .
    volumes:
      - /opt/docker-share:/var/docker-share
```

## Deploy to Server

* First make sure docker and docker-compose are installed on server. We use Ubuntu.
* Upload all files to server
* run following cmd to build and start the container in background

```bash
# build image before start container and run the container in background
sudo docker-compose up --build -d 

# stop service/container, monitor is the service name in docker-compose.yml
sudo docker-compose stop monitor 
```