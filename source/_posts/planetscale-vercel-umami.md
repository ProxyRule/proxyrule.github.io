---
title: 使用 PlanetScale 和 Vercel 搭建 Umami 统计
date: 2023-08-08 18:31:57
tags:
    - PlanetScale
    - Vercel
    - Umami
---

本文基本可以视作是 [GeekPlux][] 文章的中文翻译，插图亦来自于[原文][Setting Up Umami as Your Google Analytics Alternative: A Step-by-Step Guide]。

<!-- more -->

### 前置要求

- [PlanetScale] 账号，免费版仅支持创建一个数据库
- [Vercel] 账号
- [GitHub] 账号

### PlanetScale 设置

在面板创建一个数据库，命名为 umami-db

![创建数据库](https://img.zs.fyi/2308/planetscale.webp)

### GitHub 设置

Fork [umami] 项目到自己账号，同时建议安装 GitHub 应用 [Pull]

### Vercel 设置

将 Vercel 与 GitHub 账号关联，并导入上面 fork 的项目

此时，Vercel 尝试自动部署但应该会失败，因为还没设置好数据库连接

### 数据库关联

回到 PlanetScale 面板，点击 Integrations 并选择 Vercel，选中 umami-db 和对应的 Vercel 项目

至于框架，可能名称与下图不完全相同，选中带有 Prisma 的即可

![数据库关联](https://img.zs.fyi/2308/integrate.webp)

### Umami 设置

最后，回到 Vercel 面板，点击重新部署

部署成功后，可以添加自定义域名

umami 后台默认用户名为 admin，默认密码为 umami

具体如何使用请参考 [umami 文档]

[GeekPlux]: https://geekplux.com/
[Setting Up Umami as Your Google Analytics Alternative: A Step-by-Step Guide]: https://geekplux.com/posts/setup-umami
[PlanetScale]: https://planetscale.com/
[Vercel]: https://vercel.com/
[GitHub]: https://github.com/
[umami]: https://github.com/umami-software/umami
[Pull]: https://github.com/apps/pull
[umami 文档]: https://umami.is/docs
