---
title: 如何删除 Windows 启动项中的「Program」
date: 2023-07-29 11:53:02
tags:
    - Windows
---

这世界对强迫症患者太不友好，比如 Windows 10 上某一软件卸载后可能启动项还残留在那里，并且名称显示为 Program（下图中第二项），关键他喵的名称前还没有软件图标（当然没有，毕竟软件已经被卸载了）。

<!-- more -->

![Before](https://img.zs.fyi/2308/Before.png)

通过 Google，得知可以修改注册表来解决这一问题：

首先 <kbd>Start</kbd> + <kbd>R</kbd> 呼出运行，输入 `regedit`，回车打开注册表编辑器

- 如果是要删除系统开机启动项（影响所有用户），注册表定位到

```text
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
```

- 如果要删除当前用户的开机启动项，注册表定位到

```text
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
```

像我的就是属于第二种情况，进入注册表后找到已删除软件的残余启动项

![Registry](https://img.zs.fyi/2308/Registry.png)

接下来当然是右键直接删除该键值了

然后再打开任务管理器，发现原本闹心的「Program」果然不见了，世界又充满爱了

![After](https://img.zs.fyi/2308/After.png)
