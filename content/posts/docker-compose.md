---
title: "Docker Compose"
date: 2024-03-19T10:48:33+01:00
draft: false
categories:
- Development
- Docker
tags:
- dev
- docker
---

### Build & run Docker-compose

```
docker-compose up --build
```

### Get running docker compose folder

```
 docker inspect <container-name> | grep "com.docker.compose.project.working_dir"
```

