---
title: "Docker Images"
date: 2024-08-13T09:49:08+02:00
draft: false
categories:
  - Development
  - Docker
tags:
  - dev
  - docker
---

# Docker Images

### List
```
docker images ls
```
### Run
```
docker run -p 13000:8080 <image>
```
### Show Layers
```
docker image history <image>
```
### Inspect
```
docker image inspect <image>
```

### Build from Dockerfile
```
docker build -t <image> .
docker build -t <image> -f Dockerfile-tasks .
```

### Publish on Docker Hub
```
docker image tag <image-source:tag> <vendor/image-target:tag>
docker image ls
docker login     
cat .docker/config.json
docker image push <image>
docker logout
```