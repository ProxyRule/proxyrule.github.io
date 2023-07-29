---
title: Windows 10 无法进入休眠状态
date: 2023-07-29 12:00:56
tags:
    - Windows
---

系统是 Windows 专业版 1909，电源计划是默认的平衡模式。有时候晚上合盖后第二天早上笔记本发烫，揭开盖子直接进入解锁登录界面。这次又遇到了，寻找解决方法，并稍作记录。

<!-- more -->

![powercfg](/images/2002/powercfg.png)

如图，我的笔记本支持 Standby (S3)、Hibernate、Hybrid Sleep、Fast Startup 这四种 sleep states。其中第一种就相当于 Sleep，中文翻译为睡眠；第二种则叫做休眠；第三种叫做混合睡眠；第四种是 Windows 8 时引入的快速关机。

它们之间乱七八糟的关系和区别可以查看以下几篇文章：

- [hybrid-sleep 和 sleep 以及 hibernate 的区别](https://blog.csdn.net/Listener_ri/article/details/50835594)

- [睡眠、休眠、混合睡眠三者之间的关系与区别](https://blog.csdn.net/y97523szb/article/details/78108777)

- [关于电脑的待机、睡眠、休眠，这篇应该可以解答你所有的疑问](https://zhuanlan.zhihu.com/p/47006051)（推荐）

- [What’s the Difference Between Sleep and Hibernate in Windows?](https://www.howtogeek.com/102897/whats-the-difference-between-sleep-and-hibernate-in-windows/)

下面则是电池平衡模式下默认的高级设置选项，留存方便日后对比：

![default_settings](/images/2002/default_settings.png)

上面第三篇文章中有这么两段话点醒了我

> 不过呢，随着移动时代的到来，微软肯定也意识到了这个问题。从 Windows 8 开始，Windows 引入了一种新的电源状态，叫 S0 Standby，或 Modern Standby，原理和我上面分析的 iPhone 熄屏状态一模一样。该状态在一些 Windows 平板电脑上实现了，比如 Surface Pro 3，小米平板二代。
>
> 用 `powercfg -a` 可查得当前系统是否支持 Modern Standby。Modern Standby 又分两种，一种是不带网络连接的，另一种是带网络连接的，后者在进入熄屏状态时可以被特定的网络数据包唤醒（需要网卡硬件支持），后者也称 Connected Standby。小米平板二代支持后者。

同时结合这篇[文章](https://blog.csdn.net/hanziyuan08/article/details/89396894)，我查看了 `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power` 下的「CsEnabled」键值，竟然默认是 1。

![CsEnabled](/images/2002/CsEnabled.png)

那么问题来了，上面第一张图明明显示我的电脑不支持 S0 Standby，电池驱动管理却依然将其开启。我觉得很可能就是这个问题了。
