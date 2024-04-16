---
title: Docker 搭建 YOURLS
tags:
  - Docker
  - YOURLS
date: 2024-04-16 13:50:38
---

Yourls 是一款开源的短链接管理软件，可用于为您的博客文章创建简短、易于记忆的链接。它是一个自托管的解决方案，这意味着您可以将其安装在自己的服务器上，并完全控制您的数据。

<!-- more -->

Yourls 的一些主要功能包括：

* 创建自定义短链接
* 跟踪链接点击数
* 设置密码保护链接
* 使用不同的缩短策略
* 分享链接到社交媒体
* 使用 API 集成到其他应用程序

### Yourls 的优点

Yourls 使用方便，功能强大，并且可以免费使用。它是博客作者和网站管理员的流行选择，因为它可以帮助他们：

* 缩短长而笨重的 URL，使其更易于共享和记忆
* 跟踪链接点击数以了解内容的受欢迎程度
* 使用自定义短链接来创建品牌一致性
* 保护敏感内容（例如下载链接）

### 如何开始使用 Yourls

Yourls 可以轻松安装在任何 Web 服务器上。您可以在 [https://yourls.org/](https://yourls.org/) 下载软件并找到有关安装和配置的说明。

安装 Yourls 后，您就可以开始创建短链接了。只需输入要缩短的 URL，Yourls 就会为您生成一个短链接。您可以自定义短链接，使其更具品牌特色或易于记忆。

Yourls 还提供了一些高级功能，例如跟踪链接点击数和设置密码保护链接。要了解有关这些功能的更多信息，请参阅 Yourls 文档。

### 总结

Yourls 是一款功能强大且易于使用的短链接管理软件。它是博客作者和网站管理员的宝贵工具，可以帮助他们提高内容的可共享性和参与度。

以下是一些您可以使用 Yourls 改进博客文章的具体方法：

* 为每个博客文章创建自定义短链接。这将使您的文章更易于在社交媒体上共享，并使其看起来更专业。
* 使用 Yourls 跟踪链接点击数以了解哪些文章最受欢迎。此信息可用于帮助您创建更多读者会喜欢的内容。
* 使用密码保护链接来保护敏感内容，例如下载链接或仅供会员访问的页面。

如果您正在寻找一种方法来改善博客文章的可共享性和参与度，那么 Yourls 值得一试。这是一个免费且易于使用的工具，可以为您提供许多好处。

```yaml
version: "3.1"
services:
  yourls:
    container_name: yourls_app
    image: yourls
    restart: always
    ports:
      - 8080:80
    environment:
      YOURLS_DB_PASS: BM#5ztM3Z9H@zZyY
      YOURLS_SITE: https://link.zs.fyi
      YOURLS_USER: zheng
      YOURLS_PASS: z&t7LEhB7*NX54oCzybGHY4#
  mysql:
    container_name: yourls_db
    image: mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: BM#5ztM3Z9H@zZyY
      MYSQL_DATABASE: yourls
    volumes:
      - dbdata:/var/lib/mysql
volumes:
  dbdata:
```
