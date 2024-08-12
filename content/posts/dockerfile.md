---
title: "Dockerfile"
date: 2024-08-12T14:43:29+02:00
draft: false
categories:
  - Development
  - Docker
tags:
  - dev
  - docker
---

## Mandatory parameters

### FROM
```
FROM <image source>
FROM scratch
```

### ENV
```
ENV MY_VARIABLE "value"
```

### RUN
```
RUN apt-get update && aptè-get install nginx curl -y
```

### EXPOSE PORT
```
EXPOSE 80 443
```

### EXEC COMMAND
```
CMD ["nginx", "-g", "daemon off;"]
```

