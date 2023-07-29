---
title: Task Scheduler CPU 占用高
date: 2023-07-29 11:39:27
tags:
    - Windows
---

### 情形

1. 风扇一直转个不停

2. 查看任务管理器，CPU 占用一直是 99%

3. Task Scheduler 占用 CPU 高达 60%

<!-- more -->

![Task-Manager](/images/2001/Task-Manager.png)

### 查找解决方案

1. 个人博客：[http://www.tikas.me/task-scheduler-cpu-to-high/](http://www.tikas.me/task-scheduler-cpu-to-high/)

2. 微软社区，有个人提了个相同问题，可惜没人回答

### 尝试解决

1. <kbd>Start</kbd> + <kbd>X</kbd>，然后按 <kbd>G</kbd>，进入计算机管理

2. 定位到`任务计划程序库 - Microsoft - Windows - Customer Experience Improvement Program`，在右侧可以看到有三个计划任务，都是微软所谓的客户体验改善计划

3. 右键将它们全部禁止

4. 重启

![Task-Scheduler](/images/2001/Task-Scheduler.png)

### 最终解决

直接将这三个服务删除
