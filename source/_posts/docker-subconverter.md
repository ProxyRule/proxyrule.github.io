---
title: Docker 搭建 subconverter
date: 2023-09-02 15:25:07
tags:
    - Docker
    - subconverter
---

subconverter 是在各种订阅格式之间进行转换的实用程序。

<!-- more -->

```yml
version: '3.3'
services:
    subconverter:
        restart: always
        volumes:
            - './data:/opt/subconverter/data'
        ports:
            - '25500:25500'
        container_name: subconverter
        environment:
            - MANAGED_PREFIX=https://sub.example.com # 更改为你的域名
            - API_MODE=true
        image: 'tindy2013/subconverter:latest'
```

[Docker 内 MANAGED_PREFIX 不生效]

[Docker 内 MANAGED_PREFIX 不生效]: https://github.com/tindy2013/subconverter/issues/321
