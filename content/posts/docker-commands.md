---
title: "Docker Commands"
description: "Some Docker commands"
date: 2023-01-31T14:32:31+01:00
draft: false
categories:
- Development
- Docker
tags:
- dev
- docker
---
## General

### Info
```
docker info
```

## Network

### IP of the Docker machine
```
docker-machine ip default
```
### IP ranges

```
docker inspect my_container | grep IPv4Address
```

## Shell

### Container Shell
_Ctrl + D for exit_
```
docker exec -it <mycontainer> bash
docker exec -it <mycontainer> sh
```

## Containers

### List containers

```
docker ps
```

### Print Env Vars
```
docker exec my_container printenv
```

### Logs
```
docker logs -f --details my_container
```

## Images

### Run Image
```
docker run -p 13000:8080 my_image
```

## Resources

### Clean up any resources — images, containers, volumes, and networks — that are dangling (not associated with a container)
```
docker system prune
```
### Additionally remove any stopped containers and all unused images (not just dangling images)
```
docker system prune -a
```
## Database

### MySQL Backup 
```
docker exec my_container /usr/bin/mysqldump -u root --password=root my_database > backup.sql
```

### MySQL restore
```
cat backup.sql | docker exec -i my_container /usr/bin/mysql -u root --password=root my_database
```

## Dockerfile

### Build
```
docker build -t my_image .
```