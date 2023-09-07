---
title: Docker 搭建 Cloudflare Tunnel
date: 2023-09-07 23:25:10
tags:
    - Docker
    - Cloudflare
---

Cloudflare Tunnel 除了最基本的内网穿透（隧道）功能外，还带有一套身份认证功能用于保护应用安全，且非常简单易用。

<!-- more-->

```yml
version: "3.9"
services:

  tunnel:
    container_name: cloudflared_tunnel
    image: cloudflare/cloudflared
    restart: unless-stopped
    command: tunnel --no-autoupdate run --token <token> # 将 <token> 替换为你的
```

[产品介绍] 和 [使用文档]

[产品介绍]: https://www.cloudflare.com/products/tunnel/
[使用文档]: https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/
