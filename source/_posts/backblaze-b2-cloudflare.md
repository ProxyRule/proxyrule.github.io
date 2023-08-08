---
title: Backblaze B2 云存储搭配 Cloudflare
date: 2023-08-08 16:04:27
tags:
    - Backblaze
    - Cloudflare
---

本文主要介绍如何设置 Backblaze B2 云存储并将其接入 Cloudflare CDN，以及通过 ShareX 上传图片和文件。

<!-- more -->

### 前置要求

- [Backblaze][] 账号
- Cloudflare 账号
- 域名并将 DNS 解析托管到 Cloudflare，请以实际域名替代示例中的 example.com
- [ShareX][] 或其他类似软件已安装

### Backblaze 设置

创建一个存储桶，文件公开还是私密访问视需求而定。如果是用于博客图片的话，选择公开即可。其他默认。

![创建存储桶](https://img.shuaizheng.org/2308/6O2l6ELCQY.png)

上传一个文件，点击左侧的浏览文件，获取“友好 URL”

![友好 URL](https://img.shuaizheng.org/2308/4nLxvoQJfb.png)

点击刚刚创建的桶，设置桶的缓存时间为 `{"cache-control":"max-age=720000"}`

![桶缓存时间](https://img.shuaizheng.org/2308/gKaVpmPQQ2.png)

### Cloudflare 设置

一、添加一条 CNAME 解析，比如 img.example.com，目标地址即为“友好 URL”图中所示

![CNAME](https://img.shuaizheng.org/2308/0pbNHUnjYz.png)

二、添加一条页面规则，URL 为 `img.example.com/*`，SSL 选择严格，缓存级别为“缓存所有内容”，边缘缓存 TTL 为“1 个月”

![页面规则](https://img.shuaizheng.org/2308/875SUdYNzB.png)

三、添加一条转换规则 - 重写 URL

规则名称随便，比如 Remove bucket name

下方选择“主机名”“等于” `img.example.com` 即可

![表达式](https://img.shuaizheng.org/2308/I68OSKYLaL.png)

然后选择“重写到”“Dynamic 动态”，框中填

```text
concat("/file/bucketName", http.request.uri.path)
```

将 bucketName 更改为你一开始设置的桶名称

![重写](https://img.shuaizheng.org/2308/POqzrOd9Kp.png)

四、添加一条修改响应头规则

规则名称随意，比如 Remove x-bz-*

设置和上方一样的自定义筛选表达式

添加一个动态响应头，名为 `ETag`，值为

```text
concat(http.response.headers["x-bz-content-sha1"][0], http.response.headers["x-bz-info-src_last_modified_millis"][0], http.response.headers["x-bz-file-id"][0])
```

删除如下响应头

```text
x-bz-content-sha1
x-bz-file-id
x-bz-file-name
x-bz-info-src_last_modified_millis
x-bz-upload-timestamp
```

![修改响应头](https://img.shuaizheng.org/2308/zPZXHPkUJd.png)

### ShareX 设置

先到 Backblaze 后台创建一个应用密钥，建议下拉选择刚刚创建的桶，限制密钥的作用范围

![创建密钥](https://img.shuaizheng.org/2308/roTfv4ihcI.png)

记下 applicationKey，仅会出现一次

到 ShareX 中找到 Backblaze 设置，依次填入存储桶的 keyID，applicationKey，桶名。设置上传路径偏好，并填入自定义域名 img.example.com

![ShareX 设置](https://img.shuaizheng.org/2308/e4LrXnkNmr.png)

### 本文参考

- [Free Image Hosting With Cloudflare Transform Rules and Backblaze B2][]
- [backblaze 接入 cloudflare 自定义域名完整过程][]

[Backblaze]: https://www.backblaze.com/
[ShareX]: https://getsharex.com/
[backblaze 接入 cloudflare 自定义域名完整过程]: https://www.jingxialai.com/4155.html
[Free Image Hosting With Cloudflare Transform Rules and Backblaze B2]: https://www.backblaze.com/blog/free-image-hosting-with-cloudflare-transform-rules-and-backblaze-b2/
