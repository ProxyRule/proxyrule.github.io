---
title: Docker 搭建 Portainer
date: 2023-09-06 13:03:27
tags:
    - Docker
    - Portainer
---

Portainer 是一个用户友好的图形化界面工具，用于轻松管理 Docker 容器和容器编排，无需深入命令行操作。

<!-- more -->

```yml
version: '3.3'

services:

    portainer-ee:
        ports:
            - '8000:8000'
            - '9443:9443'
        container_name: portainer
        restart: always
        volumes:
            - '/var/run/docker.sock:/var/run/docker.sock'
            - 'portainer_data:/data'
        image: 'portainer/portainer-ee:latest'

volumes:
    portainer_data:
```

[Portainer 官方网站]

[Portainer 官方网站]: https://www.portainer.io/
