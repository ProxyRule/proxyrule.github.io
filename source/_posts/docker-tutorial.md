---
title: Docker for Windows 网络方面的坑
date: 2020-01-15 13:33:48
tags:
  - Docker
  - Windows
---

最先在小众软件上分享：[Docker for Windows 网络方面的坑](https://meta.appinn.net/t/docker-for-windows/13263)

<!-- more -->

### 环境

- Windows 10 LTSC 2019 Version 1809
- 启用 Hyper-V
- 安装 Docker for Windows 非 Docker Toolbox

### 问题

1. 网络状态未识别，默认安装的两个虚拟交换机 Default Switch 和 DockerNAT 一直是未识别的网络

![Internet](https://img.zs.fyi/2308/Internet.png)

![vEthernet](https://img.zs.fyi/2308/vEthernet.png)

2. 默认安装的虚拟机不显示 IP 地址，Hyper-V 也无法连接上虚拟机

![VM](https://img.zs.fyi/2308/VM.png)

3. 命令 `docker-machine ls` 显示为空，即使上图中 Docker 的宿主机是存在的

![docker-machine](https://img.zs.fyi/2308/docker-machine.png)

和这位遇到的情况差不多：[docker hyper-V 无法访问虚拟机](https://segmentfault.com/q/1010000014706486)

### 结果

放弃。有 Linux 不用，我在 Windows 上用 Docker 图的啥啊，又不馋她身子。

另外，这类的 issue 三年前就有人提了：[Docker HyperV vEthernet (DockerNAT) is Un-identified Network (Public Network) in tray](https://github.com/docker/for-win/issues/367)，至今似乎仍未解决。

### 后续

我继续查了点资料后，发现 Docker 默认的网络模式其实是 bridge，参见：[docker docs - Networking overview](https://docs.docker.com/network/)

但是，Docker bridge 的实现方式好像又跟 Linux bridge 不太一样，具体我没看太懂，参见：[探索 Docker bridge 的正确姿势 小白亲测有效！](https://www.v2ex.com/t/344321)

Docker bridge 模式下 Docker Daemon 会创建一个名为 docker0 的虚拟网桥（[配置 docker0 网桥](https://yeasy.gitbooks.io/docker_practice/advanced_network/docker0.html)），用于连接宿主机与容器，或者是容器之间的通信……

不过呢，以上所说的都是 Linux 环境下的。Docker for Windows 虽然同样默认 bridge 模式，但是由于实现方式不同，

> Because of the way networking is implemented in Docker Desktop for Windows, you cannot see a docker0 interface on the host. This interface is actually within the virtual machine.

Windows 上是不存在 docker0 这玩意儿的，参见：[docker docs - Networking features in Docker Desktop for Windows](https://docs.docker.com/docker-for-windows/networking/)

所以，我就一直在这里面转不出来。
