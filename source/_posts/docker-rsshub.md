---
title: Docker 搭建 RSSHub
date: 2023-09-03 08:35:37
tags:
    - Docker
    - RSSHub
---

RSSHub 是一个开源、简单易用、易于扩展的 RSS 生成器，可以给任何奇奇怪怪的内容生成 RSS 订阅源。

<!-- more -->

```yml
version: '3'

services:
    rsshub:
        container_name: rsshub_app
        # two ways to enable puppeteer:
        # * comment out marked lines, then use this image instead: diygod/rsshub:chromium-bundled
        # * (consumes more disk space and memory) leave everything unchanged
        image: diygod/rsshub
        restart: always
        ports:
            - '1200:1200'
        environment:
            NODE_ENV: production
            CACHE_TYPE: redis
            REDIS_URL: 'redis://redis:6379/'
            PUPPETEER_WS_ENDPOINT: 'ws://browserless:3000'  # marked
        depends_on:
            - redis
            - browserless  # marked

    browserless:  # marked
        container_name: rsshub_browserless
        image: browserless/chrome  # marked
        restart: always  # marked
        ulimits:  # marked
          core:  # marked
            hard: 0  # marked
            soft: 0  # marked

    redis:
        container_name: rsshub_redis
        image: redis:alpine
        restart: always
        volumes:
            - redis_data:/data

volumes:
    redis_data:
```

[RSSHub 官方网站]

[RSSHub 官方网站]: https://docs.rsshub.app/
