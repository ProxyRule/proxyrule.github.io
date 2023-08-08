---
title: 恢复 Linux U 盘启动盘在 Windows 下的使用
date: 2023-07-29 12:02:23
tags:
    - Windows
    - Linux
---

通过 DD 方式制作 Linux 启动盘后，将 U 盘插在 Windows 笔记本上，要么在文件资源管理器中不显示或者显示可用空间只有 200MB，尽管在磁盘管理器中是正常的。而且，此时尝试直接格式化也是没有用的。可通过以下两步解决：

<!-- more -->

第一步，调用 diskpart 命令

```shell
diskpart
list disk
select disk <number>  # <number> 为实际磁盘编号
clean
```

![diskpart](https://img.zs.fyi/2308/diskpart.png)

第二步，在磁盘管理器中创建

![disk-management](https://img.zs.fyi/2308/disk-management.png)
