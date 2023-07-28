---
title: "Docker Node"
date: 2023-07-28T10:10:49+02:00
draft: true
categories:
- Development
- Docker
- Node
tags:
- dev
- docker
- node
---

## Dockerfile

```
FROM node:18

RUN apt-get update -y && \
    apt-get install -y --no-install-recommends gosu

COPY entrypoint.sh /usr/local/bin/entrypoint.sh

WORKDIR /app

EXPOSE 3000

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
```

## entrypoint.sh

```
#!/bin/bash

uid=$(stat -c %u /app)
gid=$(stat -c %g /app)

sed -ri "s/node:x:[0-9]+:[0-9]+:/node:x:$uid:$gid:/g" /etc/passwd
sed -ri "s/node:x:[0-9]+:/node:x:$gid:/g" /etc/group
chown -R node:node /home/node

user=$(grep ":x:$uid:" /etc/passwd | cut -d: -f1)
if [ $# -eq 0 ]; then
    echo exec "$@"
    exec exec "$@"
else
    echo gosu $user "$@"
    exec gosu $user "$@"
fi
```

## docker-compose.yaml
```
node:
        build: .docker/node
        volumes:
            - ./path:/app
```
