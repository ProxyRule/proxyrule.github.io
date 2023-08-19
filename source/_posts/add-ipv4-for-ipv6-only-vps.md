---
title: 通过 WARP 为纯 IPv6 VPS 添加 IPv4 网络
date: 2023-08-19 09:00:22
tags:
    - Cloudflare
    - WARP
    - IPv6
    - VPS
---

[WARP] 是 Cloudflare 推出的基于 WireGuard 的 VPN

以下内容基于 Debian 10+

<!-- more -->

### 安装 WireGuard

准备工作，安装 `sudo` 和 `lsb_release`

```shell
apt install sudo lsb-release -y
```

安装必要的网络工具

```shell
sudo apt install net-tools iproute2 openresolv dnsutils -y
```

安装 Wire­Guard 配置工具

```shell
sudo apt install wireguard-tools --no-install-recommends
```

通过 `uname -r` 查看内核版本，如果是 5.6 及以上内核则已经集成了 Wire­Guard，就不需要安装了，直接跳到下个部分。

否则，需要先添加 back­ports 源

```shell
echo "deb http://deb.debian.org/debian $(lsb_release -sc)-backports main" | sudo tee /etc/apt/sources.list.d/backports.list
sudo apt update
```

再安装 back­ports 仓库中的内核

```shell
sudo apt -t $(lsb_release -sc)-backports install linux-image-$(dpkg --print-architecture) linux-headers-$(dpkg --print-architecture) --install-recommends -y
```

安装完成后，再次执行 `uname -r` 确保新版内核已启用

### 通过 wgcf 生成配置文件

在安装之前，先修改 DNS 以便下面操作，将 `/etc/resolv.conf` 中的 nameserver 修改为以下

```text
nameserver 2a03:7900:2:0:31:3:104:161
nameserver 2a00:1098:2b::1
nameserver 2a01:4f8:c2c:123f::1
nameserver 2a00:1098:2c::1
```

安装 wgcf

```shell
curl -fsSL git.io/wgcf.sh | sudo bash
```

注册 WARP 账户

```shell
wgcf register
```

生成 Wire­Guard 配置文件

```shell
wgcf generate
```

### 编辑配置文件

修改 `wgcf-profile.conf` 配置文件

- 将 `engage.cloudflareclient.com:2408` 替换为 `[2606:4700:d0::a29f:c001]:2408`
- 删除或注释掉 `AllowedIPs = ::/0`
- 将 `DNS = 1.1.1.1` 修改为 `DNS = 2606:4700:4700::1111`

完成并保存后，将 Wire­Guard 配置文件复制到 `/etc/wireguard/` 并命名为 `wgcf.conf`

```shell
sudo cp wgcf-profile.conf /etc/wireguard/wgcf.conf
```

开启网络接口（命令中的 `wgcf` 对应的是配置文件 `wgcf.conf` 的文件名前缀）

```shell
sudo wg-quick up wgcf
```

使用 `curl -4 ip.sb` 看看能否顺利返回 IPv4

没问题后，执行 `crontab -e` 命令，添加 `@reboot systemctl start wg-quick@wgcf` 到文件末尾设置开机自启。

[WARP]: https://blog.cloudflare.com/1111-warp-better-vpn/
