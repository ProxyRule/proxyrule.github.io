---
title: 记一次 Windows 蓝屏修复？
date: 2020-01-16 23:47:18
tags:
  - Windows
---

之前一直在折腾 Docker for Windows，但是网络方面的坑太深了我跨不过去，最终只得作罢。如果只是这样那还好，好聚好散、不合适就分，最恼人的是在我卸载完 Docker 到「Windows 功能」中关闭 Hyper-V 重启时竟然蓝屏了，停止代码为 `SYSTEM THREAD EXCEPTION NOT HANDLED`，且之后时不时会蓝屏一下，只能通过还原点还原系统。要是放以前，我应该早就掏出 U 盘重装了，但我实在不想折腾了，一想到那些大型软件又得重装一遍就头疼。所以，本着能修就修的想法，暂时找到以下解决办法（或许不是）。

<!-- more -->

### 查找问题

通过使用 BlueScreenView 查看 dump 文件，确定是由 `winhvr.sys` 驱动引起的 `ntoskrnl.exe` 崩溃，然后再看下面的文件描述，应该是 Hyper-V 跟网络虚拟服务导致的系统内核损坏，其他的就看不出什么名堂了。见下图：

![BlueScreenView](/images/2001/BlueScreenView.png)

另外，dmp 文件我放在这里了：[011620-8250-01.dmp](/images/2001/011620-8250-01.dmp)，大佬可以帮忙看看，有遇到类似问题的也可以作为参考。

有了以上基本信息，大致知道该往哪个方向查找资料了。

### 解决问题？

我是以 `winhvr.sys ntoskrnl.exe 蓝屏` 为关键词进行谷歌搜索的，查到的[这篇](https://social.technet.microsoft.com/Forums/zh-CN/b95905a4-5b9a-4936-9d65-b7f68df089d6/ntoskrnlexe3401323631-277142116165281240501997820256dmp?forum=win10itprogeneralCN)我认为是最有帮助的，虽然也有一点小错误。

参照上面回复中提供的解决方法，操作如下：

1. 首先以管理员身份运行 CMD，输入 `sfc /SCANNOW` 进行系统扫描验证并尝试修复，扫描结果发现确实存在损坏文件。

2. 接着，运行 `DISM /Online /Cleanup-Image /RestoreHealth` 应该会自动运行什么还原操作。

3. 待上述操作完成后，输入 `sfc /VERIFYONLY` 再进行系统扫描验证，发现已经可以了，不存在完整性冲突。

![SCANNOW](/images/2001/SCANNOW.png)

不过，至于是否真的解决了问题，我也不确定，后续没再更新的话就是没问题了。

### 更新

似乎并没有用，之后 `sfc /VERIFYONLY` 虽然扫描验证说不存在完整性冲突但还是蓝屏了几次。而且，一个奇怪的现象是，关机后启动不会蓝屏，选择重启大概率会蓝屏。这是之后两次的 dmp 文件：[011720-10140-01.dmp](/images/2001/011720-10140-01.dmp) 和 [011720-10281-01.dmp](/images/2001/011720-10281-01.dmp)

受不了了，我太菜了，实在找不到办法解决了，最后决定重装系统了。另外，我后来复盘发现，极有可能是 VMware 和 Hyper-V 冲突的原因，然而当时没有意识到。
