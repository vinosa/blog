---
title: "Docker Persistance"
date: 2024-08-13T09:45:11+02:00
draft: true
categories:
  - Development
  - Docker
tags:
  - dev
  - docker
---
# Docker persistence

## Volumes

### List
````
docker volume ls
````

### Inspect
```
docker volume inspect <volume>
```

### Create
```
docker volume create [OPTIONS] <name/id>
```

### Delete unused volumes (by containers)
```
docker volume prune
```