---
title: "Docker Network"
date: 2024-08-13T09:53:37+02:00
draft: false
categories:
  - Development
  - Docker
tags:
  - dev
  - docker
---
# Docker Network

### List
```
docker network ls
```

### Inspect
```
docker network inspect <network>
```

### Container info
```
docker container port  <container> 
docker container inspect --format '{{.NetworkSettings.IpAddress}}' <container>
```

### Create custom network
```
docker network create <network>
docker container run --name <container> --network <network> -d <container>
```

### Connect to container
```
docker network connect <network> <container>
docker network disconnect <network> <container>
```
