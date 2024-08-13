---
title: "Docker Tips"
description: "Miscellaneous Docker tips"
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

### Info
```
docker info
```

### IP of the Docker machine
```
docker-machine ip default
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




