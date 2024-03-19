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
## Install
```
sudo curl -L "https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version
```

### Build & run Docker-compose

```
docker-compose up --build
```

### Get running docker compose folder

```
 docker inspect <container-name> | grep "com.docker.compose.project.working_dir"
```

