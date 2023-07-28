---
title: "Docker Php"
date: 2023-07-28T10:08:56+02:00
draft: true
categories:
- Development
- Docker
- Php
tags:
- dev
- docker
- php
---

## Dockerfile

```
FROM php:8.2-fpm

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update -y && \
    apt-get install -y --no-install-recommends apt-utils git unzip gosu

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

RUN chmod uga+x /usr/local/bin/install-php-extensions && \
    sync && \
    install-php-extensions zip \
                           pdo_pgsql \
                           gd \
                           intl \
                           opcache \
                           xsl

COPY conf.d/opcache.ini /usr/local/etc/php/conf.d/opcache.ini

ENV COMPOSER_HOME /.composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

COPY entrypoint.sh /usr/local/bin/entrypoint.sh

WORKDIR /app

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
```

## entrypoint.sh

```
#!/bin/bash

uid=$(stat -c %u /app)
gid=$(stat -c %g /app)

if [ $uid == 0 ] && [ $gid == 0 ]; then
    if [ $# -eq 0 ]; then
        php-fpm --allow-to-run-as-root
    else
        echo "$@"
        exec "$@"
    fi
fi
  
sed -ri "s/www-data:x:[0-9]+:[0-9]+:/www-data:x:$uid:$gid:/g" /etc/passwd
sed -ri "s/www-data:x:[0-9]+:/www-data:x:$gid:/g" /etc/group

user=$(grep ":x:$uid:" /etc/passwd | cut -d: -f1)
if [ $# -eq 0 ]; then
    php-fpm
else
    echo gosu $user "$@"
    exec gosu $user "$@"
fi
```

## docker-compose.yaml
```
php:
        build: .docker/php-fpm
        volumes:
            - .:/app
            - ~/.composer:/.composer
```

## conf.d/opcache.ini

```
[opcache]
opcache.enable=1
; 0 means it will check on every request
; 0 is irrelevant if opcache.validate_timestamps=0 which is desirable in production
opcache.revalidate_freq=0
opcache.validate_timestamps=1
opcache.max_accelerated_files=10000
opcache.memory_consumption=192
opcache.max_wasted_percentage=10
opcache.interned_strings_buffer=16
opcache.fast_shutdown=1
```
