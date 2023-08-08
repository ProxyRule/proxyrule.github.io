---
title: Windows 10 开机后浏览器自动打开 MSN 中文网
date: 2023-07-29 11:37:10
tags:
    - Windows
---

重装系统后，遇到这个问题，得到微软社区的解答后问题解决，顺便记录一下。

<!-- more -->

1. Windows 10 专业版，打开组策略

2. 依次展开`计算机配置 - 管理模板 - 系统 - Internet 通信管理`，然后单击 `Internet 通信设置`

3. 在详细信息窗格中，双击`关闭 Windows 网络连接状态指示器活动测试`，然后单击`已启用`，点`确定`

![Group-Policy](https://img.zs.fyi/2308/Group-Policy.png)
