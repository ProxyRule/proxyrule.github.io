---
title: 专治 Windows 10 各种不服
date: 2020-01-23 19:28:46
tags:
  - Windows
  - Bug
---

大约从 2015 年开始使用 Windows 10 系统，这是一个不断遇到坑然后想办法填坑的过程，以下便是遇到的各种稀奇古怪的问题以及可能的解决方案。

<!-- more -->

### Windows 10 开机后浏览器自动打开 MSN 中文网

重装系统后，遇到这个问题，得到微软社区的解答后问题解决，顺便记录一下。

1. Windows 10 专业版，打开组策略

2. 依次展开`计算机配置 - 管理模板 - 系统 - Internet 通信管理`，然后单击 `Internet 通信设置`

3. 在详细信息窗格中，双击`关闭 Windows 网络连接状态指示器活动测试`，然后单击`已启用`，点`确定`

![Group-Policy](/images/2001/Group-Policy.png)

### Task Scheduler CPU 占用高

#### 情形

1. 风扇一直转个不停

2. 查看任务管理器，CPU 占用一直是 99%

3. Task Scheduler 占用 CPU 高达 60%

![Task-Manager](/images/2001/Task-Manager.png)

#### 查找解决方案

1. 个人博客：[http://www.tikas.me/task-scheduler-cpu-to-high/](http://www.tikas.me/task-scheduler-cpu-to-high/)

2. 微软社区，有个人提了个相同问题，可惜没人回答

#### 尝试解决

1. <kbd>Start</kbd> + <kbd>X</kbd>，然后按 <kbd>G</kbd>，进入计算机管理

2. 定位到`任务计划程序库 - Microsoft - Windows - Customer Experience Improvement Program`，在右侧可以看到有三个计划任务，都是微软所谓的客户体验改善计划

3. 右键将它们全部禁止

4. 重启

![Task-Scheduler](/images/2001/Task-Scheduler.png)

#### 最终解决

直接将这三个服务删除

### 删除 Windows 10 桌面右键菜单中的图形选项和图形属性选项

我的笔记本电脑经常在集成显卡驱动更新后，点击鼠标右键要等很长时间才能弹出菜单来。作为强迫症自然不能忍受，网络搜索得到答案后，写在这作为记录。

1. 按下 <kbd>Start</kbd> + <kbd>R</kbd> 调出运行，输入 `regedit` 回车

2. 在注册表编辑器中定位到

```text
HKEY_CLASSES_ROOT\Directory\Background\shellex\ContextMenuHandlers
```

3. 删除 `igfxDTCM` 和 `igfxcui` 两个项目

### Windows 10 用户账号历史头像删除

数次更换头像后，在当前头像右侧会显示之前设置的头像，作为强迫症这是绝对不能忍的。

解决方法如下：

1. <kbd>Start</kbd> + <kbd>R</kbd> 运行，输入 `%APPDATA%\Microsoft\Windows\AccountPictures`，然后确定

2. 进入后，可以看到之前使用过的头像，删除即可

3. 再按下 <kbd>Start</kbd> + <kbd>I</kbd>，进入`账户 - 你的信息`，可以看到之前的头像已经消失

### 通过组策略禁用 Windows 10 系统的自动更新功能

众所周知近年微软发布的更新经常性的导致 Windows 10 用户出现诸如开机黑屏或者是直接无法启动的问题。显然对于绝大多数用户来说能否稳定的运行才是最重要的，同时也并不是所有的用户都有能力解决各类问题。

例如近期微软更新致使部分用户启动出现黑屏的问题，虽有临时解决方案但并不是所有的设备都可顺利解决。因此还不如直接禁用掉 Windows 10 的自动更新策略，省的每次安装更新都是提心吊胆的害怕出现新的问题。基于安全性考虑我们是不会建议大家不安装更新的，此处的禁用只是禁用掉自动更新而不是禁用更新。

1. 快捷键 <kbd>Start</kbd> + <kbd>R</kbd> 打开运行，然后在运行的对话框中填写 `gpedit.msc` 并确定

2. 打开组策略后点击左侧菜单依次展开`计算机配置 — 管理模板 — Windows 组件 — Windows 更新`

![Windows-Update](/images/2001/Windows-Update.png)

3. 双击`配置自动更新`即可打开如下图的新窗口，在新窗口左侧的选项里将默认的`未配置`更改为`已禁用`即可

![AutoUpdate](/images/2001/AutoUpdate.png)

{% note info %}
将此项变更为已禁用后系统将不再自动检查和下载安装更新， 因此你需要定期去更新里手动进行检查。原文出处为 [https://www.landiannews.com/archives/40677.html](https://www.landiannews.com/archives/40677.html)
{% endnote %}

### 如何删除 Windows 启动项中的「Program」

这世界对强迫症患者太不友好，比如 Windows 10 上某一软件卸载后可能启动项还残留在那里，并且名称显示为 Program（下图中第二项），关键他喵的名称前还没有软件图标（当然没有，毕竟软件已经被卸载了）。

![Before](/images/2001/Before.png)

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

![Registry](/images/2001/Registry.png)

接下来当然是右键直接删除该键值了

然后再打开任务管理器，发现原本闹心的「Program」果然不见了，世界又充满爱了

![After](/images/2001/After.png)

### Windows 10 升级 1803 后多了个 OEM 分区并分配盘符

#### 问题概述

手上的一台联想笔记本在更新到 Windows 1803 版后在`此电脑 - 设备和驱动器`下「多了一个 OEM 分区」并且分配了盘符。

![This-PC-Before](/images/2001/This-PC-Before.png)

![Disk-Management-Before](/images/2001/Disk-Management-Before.png)

其实也不能算多了一个分区，这个分区原本应该是用于系统恢复的，本不该分配盘符，但估计是这次更新的 Bug 导致这样的问题。分配的盘符是跟在已占用的盘符之后，比如我外接了一个硬盘里面有两个分区，占用了 D 和 E，所以之后是 F。

#### 解决办法

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

#### 问题解决

![This-PC-After](/images/2001/This-PC-After.png)

![Disk-Management-After](/images/2001/Disk-Management-After.png)

#### 参考方案

- [windows10 升级1803版后为什么多了个OEM分区并多分配了一个盘符？](https://www.zhihu.com/question/275658123)

- [win10升级1803后多了一个OEM分区是什么，可以取消吗？](https://www.zhihu.com/question/275581629)

### CPU 频率锁定在 0.4GHz

如下图，查到的解决办法各不相同，最后哪一种都没有采用。我目前的做法是，将控制面板中的电池计划选项重置为默认，然后关机重启，勉强解决燃眉之急。

![CPU](/images/2001/CPU.png)

### 更改无线网络名称

注册表地址如下：

```text
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles
```

### Windows 10 无法进入休眠状态

系统是 Windows 专业版 1909，电源计划是默认的平衡模式。有时候晚上合盖后第二天早上笔记本发烫，揭开盖子直接进入解锁登录界面。这次又遇到了，寻找解决方法，并稍作记录。

![powercfg](/images/2002/powercfg.png)

如图，我的笔记本支持 Standby (S3)、Hibernate、Hybrid Sleep、Fast Startup 这四种 sleep states。其中第一种就相当于 Sleep，中文翻译为睡眠；第二种则叫做休眠；第三种叫做混合睡眠；第四种是 Windows 8 时引入的快速关机。

它们之间乱七八糟的关系和区别可以查看以下几篇文章：

- [hybrid-sleep和sleep以及hibernate的区别](https://blog.csdn.net/Listener_ri/article/details/50835594)

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

### 恢复 Linux U 盘启动盘在 Windows 下的使用

通过 DD 方式制作 Linux 启动盘后，将 U 盘插在 Windows 笔记本上，要么在文件资源管理器中不显示或者显示可用空间只有 200MB，尽管在磁盘管理器中是正常的。而且，此时尝试直接格式化也是没有用的。可通过以下两步解决：

第一步，调用 diskpart 命令

```shell
diskpart
list disk
select disk <number>  # <number> 为实际磁盘编号
clean
```

![diskpart](/images/2011/diskpart.png)

第二步，在磁盘管理器中创建

![disk-management](/images/2011/disk-management.png)
