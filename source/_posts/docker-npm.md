---
title: Docker 搭建 Nginx Proxy Manager
date: 2023-09-04 09:07:32
tags:
    - Docker
    - Nginx Proxy Manager
---

Nginx Proxy Manager 是一个非常方便的 Nginx 代理管理工具。

<!-- more -->

```yml
version: '3.8'
services:
  app:
    container_name: npm
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      # These ports are in format <host-port>:<container-port>
      - '80:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port
      # Add any other Stream port you want to expose
      # - '21:21' # FTP

    # Uncomment the next line if you uncomment anything in the section
    # environment:
      # Uncomment this if you want to change the location of
      # the SQLite DB file within the container
      # DB_SQLITE_FILE: "/data/database.sqlite"

      # Uncomment this if IPv6 is not enabled on your host
      # DISABLE_IPV6: 'true'

    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```

[Nginx Proxy Manager 官网]

[Nginx Proxy Manager 官网]: https://nginxproxymanager.com/
