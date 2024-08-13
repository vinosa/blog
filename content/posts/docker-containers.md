---
title: "Docker Containers"
date: 2024-08-13T09:51:42+02:00
draft: false
categories:
  - Development
  - Docker
tags:
  - dev
  - docker
---
# Docker Containers

## Info

### List
```
docker container ls -a
```
### Inspect
```
docker container inspect <name/id>
```


### Print Env Vars
```
docker exec <container> printenv
docker exec <container>  /usr/bin/env
```

### Logs
```
docker container logs -f --details <container>
```

### IP ranges
```
docker inspect <container> | grep IPv4Address
```

## Create/run
```
docker container run [OPTIONS] --name <container> <image:tag>
docker container run -it --name ubuntu_22 -d ubuntu:22.04
docker run  -d -p 3306:3306 --name mybd --env "MYSQL_RANDOM_ROOT_PASSWORD=yes"  mysql
```

### Options
```
--publish <host-port:container-port> // port binding
--name <container-name>
-d // --detach
-v <volume-name:path-on-container> ex: mysql-db:/var/lib/mysql
-e <environment-variable=value>
--help
```

### Shell
_Ctrl + D for exit_
```
docker exec -it <container> bash
docker exec -it <container> sh
```


