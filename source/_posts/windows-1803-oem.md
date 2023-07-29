---
title: Windows 10 升级 1803 后多了个 OEM 分区并分配盘符
date: 2023-07-29 11:54:46
tags:
    - Windows
---

### 问题概述

手上的一台联想笔记本在更新到 Windows 1803 版后在`此电脑 - 设备和驱动器`下「多了一个 OEM 分区」并且分配了盘符。

<!-- more -->

![This-PC-Before](/images/2001/This-PC-Before.png)

![Disk-Management-Before](/images/2001/Disk-Management-Before.png)

其实也不能算多了一个分区，这个分区原本应该是用于系统恢复的，本不该分配盘符，但估计是这次更新的 Bug 导致这样的问题。分配的盘符是跟在已占用的盘符之后，比如我外接了一个硬盘里面有两个分区，占用了 D 和 E，所以之后是 F。

### 解决办法

网上搜索一番，发现有这个问题的还不在少数。但是很奇怪，我的另一台笔记本没有出现这种情况。

下面是解决办法（感谢知乎用户 [@Fisher Sam](https://www.zhihu.com/people/fisher-sam-5)）和我的参考示例：

```shell
以管理员权限运行命令提示符，逐行运行

diskpart

list disk

select disk x      （x 表示磁盘序号）

list volume

select volume x    （x 表示卷号）

remove letter=x    （x 表示盘符）
```

![DiskPart](/images/2001/DiskPart.png)

### 问题解决

![This-PC-After](/images/2001/This-PC-After.png)

![Disk-Management-After](/images/2001/Disk-Management-After.png)

### 参考方案

- [windows10 升级 1803 版后为什么多了个 OEM 分区并多分配了一个盘符？](https://www.zhihu.com/question/275658123)

- [win10 升级 1803 后多了一个 OEM 分区是什么，可以取消吗？](https://www.zhihu.com/question/275581629)
