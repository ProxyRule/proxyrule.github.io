---
title: 通过组策略禁用 Windows 10 系统的自动更新功能
date: 2023-07-29 11:51:31
tags:
    - Windows
---

众所周知近年微软发布的更新经常性的导致 Windows 10 用户出现诸如开机黑屏或者是直接无法启动的问题。显然对于绝大多数用户来说能否稳定的运行才是最重要的，同时也并不是所有的用户都有能力解决各类问题。

<!-- more -->

例如近期微软更新致使部分用户启动出现黑屏的问题，虽有临时解决方案但并不是所有的设备都可顺利解决。因此还不如直接禁用掉 Windows 10 的自动更新策略，省的每次安装更新都是提心吊胆的害怕出现新的问题。基于安全性考虑我们是不会建议大家不安装更新的，此处的禁用只是禁用掉自动更新而不是禁用更新。

1. 快捷键 <kbd>Start</kbd> + <kbd>R</kbd> 打开运行，然后在运行的对话框中填写 `gpedit.msc` 并确定

2. 打开组策略后点击左侧菜单依次展开`计算机配置 — 管理模板 — Windows 组件 — Windows 更新`

![Windows-Update](/images/2001/Windows-Update.png)

3. 双击`配置自动更新`即可打开如下图的新窗口，在新窗口左侧的选项里将默认的`未配置`更改为`已禁用`即可

![AutoUpdate](/images/2001/AutoUpdate.png)

{% note info %}
将此项变更为已禁用后系统将不再自动检查和下载安装更新，因此你需要定期去更新里手动进行检查。原文出处为 [https://www.landiannews.com/archives/40677.html](https://www.landiannews.com/archives/40677.html)
{% endnote %}
