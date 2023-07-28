---
title: "Docker Nginx"
date: 2023-07-28T10:08:17+02:00
draft: true
categories:
- Development
- Docker
- Nginx
tags:
- dev
- docker
- nginx
---

## docker-compose.yaml

```
nginx:
        image: nginx:latest
        labels:
            - "traefik.enable=true"
            - "traefik.http.routers.nginx.rule=Host(`myapp.docker.localhost`) && PathPrefix(`/api`)"
            - "traefik.http.routers.nginx.entrypoints=web,websecure"
            - "traefik.http.routers.nginx.middlewares=https-redirect"
            - "traefik.http.middlewares.https-redirect.redirectscheme.scheme=https"
            - "traefik.http.middlewares.https-redirect.redirectscheme.permanent=true"
        volumes:
            - .:/app
            - .docker/nginx/conf/default.conf:/etc/nginx/conf.d/default.conf
        links:
            - php
            - front
```

## conf/default.conf

```
server {
    root /app/path/to/public;

    location / {
        try_files $uri /index.php$is_args$args;
    }

    location ~ ^/index\.php(/|$) {
        fastcgi_pass php:9000;
        fastcgi_buffer_size 32k;
        fastcgi_buffers 32 4k;
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        fastcgi_param DOCUMENT_ROOT $realpath_root;
        internal;
    }

    location ~ \.php$ {
      return 404;
    }
}
```
