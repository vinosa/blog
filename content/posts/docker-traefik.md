---
title: "Docker Traefik"
date: 2023-07-28T10:09:50+02:00
draft: true
categories:
- Development
- Docker
- Traefik
tags:
- dev
- docker
- traefic
---

## docker-compose.yaml

```
traefik:
        image: traefik:v2.6
        command:
            - "--api.insecure=true"
            - "--providers.docker=true"
            - "--providers.docker.exposedbydefault=false"
            - "--entrypoints.web.address=:80"
            - "--entrypoints.websecure.address=:443"
            - "--entrypoints.websecure.http.tls.domains[0].main=docker.localhost"
            - "--entrypoints.websecure.http.tls.domains[0].sans=myapp.docker.localhost,*.localhost,localhost,traefik"
        ports:
            - "80:80"
            - "443:443"
            - "8080:8080"
            - "3000:3000"
        volumes:
            - /var/run/docker.sock:/var/run/docker.sock:ro
        links:
            - nginx
            - front
```
