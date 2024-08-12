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
## Install (on Linux)
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt-cache policy docker-ce
sudo apt install docker-ce
sudo systemctl status docker
sudo usermod -aG docker ${USER}
su - <user>
docker run hello-world
```

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
docker inspect <container> | grep IPv4Address
```

```
vi /etc/docker/daemon.json
{
  "default-address-pools" : [
    {
      "base" : "172.240.0.0/16",
      "size" : 24
    }
  ]
}
```

## Shell

### Container Shell
_Ctrl + D for exit_
```
docker exec -it <container> bash
docker exec -it <container> sh
```

## Containers

### Create/run container

```
docker container run --publish 80:80 --detach --name <container> <image>
docker container run -it --name ubuntu_14 -d ubuntu:14.04
docker run  -d -p 3306:3306 --name mybd --env "MYSQL_RANDOM_ROOT_PASSWORD=yes"  mysql
```

### List containers

```
docker ps
docker ls -a
```

### Print Env Vars
```
docker exec <container> printenv
docker exec <container>  /usr/bin/env
```

### Logs
```
docker logs -f --details <container>
```

## Images

### List

```
docker images
```

### Run Image
```
docker run -p 13000:8080 <image>
```

### Image Layers
```
docker image history <image>
```

### Inspect
```
docker image inspect <image>
```

### Publishing
```
docker image tag <image-source:tag> <vendor/image-target:tag>
docker image ls
docker login     
cat .docker/config.json
docker image push <image>
docker logout
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
docker exec <container> /usr/bin/mysqldump -u root --password=root my_database > backup.sql
```

### MySQL restore
```
cat backup.sql | docker exec -i <container> /usr/bin/mysql -u root --password=root my_database

docker exec -i [mysql_container_name] mysql -u[username] -p[password] [DB name] < [path/to/sql/file]
```

## Dockerfile

### Build
```
docker build -t <image> .
docker build -t <image> -f Dockerfile-tasks .
```

## Network
```
docker network ls
docker network inspect <network>
```

### Container info
```
docker container port  <container> 
docker container inspect --format '{{.NetworkSettings.IpAddress}}' <container>
```

### Custom network
```
docker network create <network>
docker container run --name <container> --network <network> -d <container>
```

```
docker network connect <network> <container>
docker network disconnect <network> <container>
```
