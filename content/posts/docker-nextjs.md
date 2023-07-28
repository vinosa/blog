---
title: "Docker integration Nextjs"
date: 2023-07-28T09:47:05+02:00
draft: true
categories:
- Development
- Docker
- React
tags:
- dev
- docker
- react
---

## Dockerfile 

```
FROM node:18-alpine AS deps

RUN apk add --no-cache libc6-compat

COPY . /app

RUN addgroup app && adduser -S -G app app

WORKDIR /app

RUN npm install

RUN npm run build

EXPOSE 3000

CMD [ "npm", "run", "start" ]
```

## docker-compose.yaml

```
front:
      container_name: front
      build: ./path/to/folder
      labels:
        - "traefik.enable=true"
        - "traefik.docker.network=web"
        - "traefik.http.routers.your_router.entrypoints=web,websecure"
        - 'traefik.port=80'
        - "traefik.http.routers.front.rule=Host(`myapp.docker.localhost`)"
        - 'traefik.http.services.front.loadbalancer.server.port=3000'
      expose:
        - "3000"
      volumes:
      - ./path/to/folder:/app
      command: npm run dev
      environment:
        NODE_ENV: development

networks:
    web:
        external:
            name: web
```

## Makefile 
```
npx:
	@test "${ARGS}"
	docker-compose run --rm node npx ${ARGS}
```

## Create Nextjs App
```
make npx create-next-app --example api-routes destination_folder
```

