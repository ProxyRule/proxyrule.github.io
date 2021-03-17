---
title: 加速 SSH 连接
date: 2020-02-26 00:10:35
tags:
  - 环境配置
  - SSH
---

有时候，你想通过 SSH 连接花重金购买的远在大洋彼岸的虚拟服务器进行诸如编译固件、跑程序等操作的时候，而你发现本地网络与远程服务器的连接并不是那么稳定，那么可以通过以下两种方式提升 SSH 连接速度和稳定性。

<!-- more -->

### 通过代理连接

其实就是借助 [ProxyCommand](https://man.openbsd.org/ssh_config.5#ProxyCommand) 这个选项来实现的，并且有几种不同的写法。而且，Windows 和 macOS 下的实现方式也不一样。

假定本地代理地址为 `127.0.0.1`，端口为 `1080`，代理方式为 `socks5`，要连接的远程主机用户为 `root`，主机 IP 为 `1.1.1.1`。

#### Windows - connect

如果下面无效的话请先安装 [connect](https://web.archive.org/web/20080516100455/http://www.meadowy.org/~gotoh/projects/connect) 这个小工具并将其添加至环境变量，或者直接在 Git Bash 中操作。

先解释一下参数含义：`connect` 即是上面安装的工具，`-S` 表示使用 socks5 代理，`-a none` 表示本地代理无需认证，`%h %p` 分别对应远程主机名和端口。

- 写法一

```shell
ssh -o "ProxyCommand connect -S 127.0.0.1:1080 -a none %h %p" root@1.1.1.1
```

- 写法二

```shell
ssh -o ProxyCommand="connect -S 127.0.0.1:1080 -a none %h %p" root@1.1.1.1
```

- 写法三

写到 `~/.ssh/config` 文件中，如针对 GitHub 可以这样写：

```text
Host github.com
    User git
    Hostname github.com
    Port 22
    ProxyCommand connect -S 127.0.0.1:1080 -a none %h %p
```

#### macOS - netcat

macOS 下可以直接看这篇[文章](https://www.xiebruce.top/650.html#i)。

### 借助跳板机连接

太详细的我也懒得写了，目前只写一下 Windows 如何实现。ProxyJump 和 ProxyCommand 都是可以的，并且如果同时写在配置文件中，只能是最先匹配到的那个生效。

假定跳板机和真正要登录的远程主机用户都为 `root`，跳板机 IP 为 `8.8.8.8`，要登录的主机 IP 为 `1.1.1.1`。

各参数含义与第一部分一致，`-W` 表示将客户端的标准输入输出转发到相应端口的远程主机上。

#### ProxyJump

```shell
ssh -o "ProxyJump root@8.8.8.8" root@1.1.1.1
```

#### ProxyCommand

- 写法一

<mark>注意</mark>：`%h` 和 `%p` 之间是 `:` 而不是空格。

```shell
ssh -o "ProxyCommand ssh root@8.8.8.8 -W %h:%p" root@1.1.1.1
```

- 写法二

使用这条命令，必须先安装 [netcat](https://eternallybored.org/misc/netcat/) 并将其添加至环境变量。

```shell
ssh -o "ProxyCommand ssh root@8.8.8.8 nc %h %p" root@1.1.1.1
```

#### 写入配置文件

即写入到 `~/.ssh/config` 文件中，这个我目前没有需求，同样可以查看这篇[文章](https://www.xiebruce.top/650.html#i-9)获取详细内容。
