---
title: 常用设置及一些镜像站
date: 2019-12-27 19:39:29
tags:
  - 环境配置
---

一些常用的代理设置方式，公共 DNS 记录以及软件镜像源，记录备用。

<!-- more -->

### 代理设置

#### CMD

```shell
# 设置
set http_proxy=http://127.0.0.1:1080
set https_proxy=http://127.0.0.1:1080
# 取消
set http_proxy=
set https_proxy=
```

#### PowerShell

```powershell
$env:http_proxy="127.0.0.1:1080"
$env:https_proxy="127.0.0.1:1080"
```

为了方便，将下面函数添加到 `$PROFILE` 中就可以通过 proxy 和 unproxy 来实现设置与取消代理了。

```powershell
# Set and unset proxy for PowerShell
function proxy {
    param (
        $ssr = "127.0.0.1:1080"
    )
    New-Item -Path Env: -Name http_proxy -Value $ssr
    New-Item -Path Env: -Name https_proxy -Value $ssr
}

function unproxy {
    Remove-Item -Path Env:\http_proxy
    Remove-Item -Path Env:\https_proxy
}
```

#### Git Bash

```bash
# 设置
export http_proxy=http://127.0.0.1:1080
export https_proxy=http://127.0.0.1:1080
# 取消
unset http_proxy https_proxy
```

#### All APPs

```shell
# 设置
netsh winhttp import proxy source=ie
# 取消
netsh winhttp reset proxy
```

#### 为 Git 设置代理

众所周知，`git clone` 有两种方式，代理设置方式也不一样：

##### Clone with HTTPS

设置代理：

```shell
# 如果是 socks5 代理的话
git config --global http.proxy socks5h://127.0.0.1:1080
# http 代理仅需将 socks5h 改为 http
git config --global http.proxy http://127.0.0.1:1080
```

取消代理：

```shell
git config --global --unset http.proxy
```

也可以仅为 GitHub 设置代理

```shell
git config --global http.https://github.com.proxy socks5h://127.0.0.1:1080
```

socks5h 和 socks5 的区别：

> In a proxy string, socks5h:// and socks4a:// mean that the hostname is
resolved by the SOCKS server. socks5:// and socks4:// mean that the
hostname is resolved locally. socks4a:// means to use SOCKS4a, which is
an extension of SOCKS4.

来源：[Differentiate socks5h from socks5 and socks4a from socks4 when handling proxy string](https://github.com/urllib3/urllib3/issues/1035)

##### Clone with SSH

需要修改 `~/.ssh/config` 文件

如果仅为 GitHub 设置代理，且使用 socks5 代理的话

```text
Host github.com
    User git
    ProxyCommand connect -S 127.0.0.1:1080 %h %p
```

这里 `-S` 表示使用 socks5 代理，如果是 http 代理则为 `-H`。connect 工具 git 自带。

我自己的话，则是设置成这样：

```text
# Reference: https://bitbucket.org/gotoh/connect/wiki/Home

Host github.com
    User git
    Hostname github.com
    Port 22
    ProxyCommand connect -S 127.0.0.1:1080 -a none %h %p

Host ssh.github.com
    User git
    Hostname ssh.github.com
    Port 443
    ProxyCommand connect -S 127.0.0.1:1080 -a none %h %p

Host *
    IdentityFile "C:\Users\Zheng\.ssh\id_rsa"
    ServerAliveInterval 30
    TCPKeepAlive yes
```

来源：[laispace/git 设置和取消代理](https://gist.github.com/laispace/666dd7b27e9116faece6)

### 公共 DNS

#### 国内

|  | 阿里云公共 DNS | 腾讯 Public DNS | 114 DNS | 百度公共 DNS | 360 公共 DNS | 红鱼 DNS |
| - | - | - | - | - | - | - |
| IPv4 | 223.5.5.5 <br /> 223.6.6.6 | 119.29.29.29 | 114.114.114.114 <br /> 114.114.115.115 | 180.76.76.76 |  |  |
| IPv6 | 2400:3200::1 <br /> 2400:3200:baba::1 |  |  | 2400:da00::6666 |  |  |
| DoT | IP <br /> dns.alidns.com | dns.pub <br /> doh.pub |  |  | dot.360.cn | rubyfish.cn |
| DoH | [阿里公共 DNS 安全传输服务](https://www.alidns.com/faqs/#dns-safe) | [DoH 和 DoT 开始公测](https://cloud.tencent.com/developer/article/1668074) |  |  | [DoH 接入方法](https://dns.360.cn/dnsPublic.html) | [面向普通消费者](https://www.rubyfish.cn/dns/solutions/) |
| 备注 |  | 首页和帮助文档中都只给出了这一个 IP | 其余几组暂时不管 |  | 传统 DNS 接入的先不管了 |  |

#### 国外

|  | Google Public DNS | OpenDNS | Cloudflare DNS | DNS.SB |
| - | - | - | - | - |
| IPv4 | 8.8.8.8 <br /> 8.8.4.4 | 208.67.222.222 <br /> 208.67.220.220 | 1.1.1.1 <br /> 1.0.0.1 | 185.222.222.222 <br /> 185.184.222.222 |
| IPv6 | 2001:4860:4860::8888 <br /> 2001:4860:4860::8844 | 2620:119:35::35 <br /> 2620:119:53::53 | 2606:4700:4700::1111 <br /> 2606:4700:4700::1001 | 2a09:: <br /> 2a09::1 |
| DoT | dns.google |  | IP <br /> one.one.one.one <br /> 1dot1dot1dot1.cloudflare-dns.com | IP <br /> dns.sb |
| DoH | [DNS-over-HTTPS (DoH)](https://developers.google.com/speed/public-dns/docs/doh) | [Using DNS over HTTPS (DoH) with OpenDNS](https://support.opendns.com/hc/en-us/articles/360038086532-Using-DNS-over-HTTPS-DoH-with-OpenDNS) | [Making Requests](https://developers.cloudflare.com/1.1.1.1/dns-over-https/request-structure/) | [DNS OVER HTTPS](https://dns.sb/doh/) |
| 备注 |  |  | [DNS over TLS vs. DNS over HTTPS \| Secure DNS](https://www.cloudflare.com/learning/dns/dns-over-tls/) | [主页](https://dns.sb/) |

#### 推荐阅读

- [如何选择适合的公共 DNS？[2020]](https://blog.skk.moe/post/which-public-dns-to-use)
- [想要上网体验有保障，如何设置一个更安全的 DNS？](https://sspai.com/post/56783)

### pypi

| 镜像站 | 帮助页 | 是否在用 |
| :------: | :------: | :------: |
| 清华 | [pypi 镜像使用帮助](https://mirror.tuna.tsinghua.edu.cn/help/pypi/) | 是 |
| 阿里云 | [使用帮助](https://developer.aliyun.com/mirror/pypi) |  |
| 网易 | [使用帮助](https://mirrors.163.com/.help/pypi.html) |  |
| 腾讯 | [使用帮助](https://mirrors.cloud.tencent.com/help/pypi.html) |  |

### NPM

| 镜像站 | 帮助页 | 是否在用 |
| :------: | :------: | :------: |
| 淘宝 | [首页](https://npm.taobao.org/) | 是 |
| 腾讯 | [使用帮助](https://mirrors.cloud.tencent.com/help/npm.html) |  |

### Anaconda

| 镜像站 | 帮助页 | 是否在用 |
| :------: | :------: | :------: |
| 清华 | [使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/) | 是 |

### MSYS2

| 镜像站 | 帮助页 | 是否在用 |
| :------: | :------: | :------: |
| 清华 | [使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/msys2/) | 是 |
| 中科大 | [使用帮助](http://mirrors.ustc.edu.cn/help/msys2.html) |  |

### RubyGems

| 镜像站 | 帮助页 | 是否在用 |
| :------: | :------: | :------: |
| Ruby China 社区 | [首页](https://gems.ruby-china.com/) | 是 |
| 清华 | [使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/rubygems/) |  |
| 阿里云 | [使用帮助](https://developer.aliyun.com/mirror/rubygems) |  |
| 中科大 | [使用帮助](http://mirrors.ustc.edu.cn/help/rubygems.html) |  |

### Go

| 镜像站 | 帮助页 | 是否在用 |
| :------: | :------: | :------: |
| Goproxy China | [首页](https://goproxy.cn/) | 是 |
| GOPROXY.IO | [首页](https://goproxy.io/) |  |
| 腾讯 | [使用帮助](https://mirrors.cloud.tencent.com/help/go.html) |  |
| 阿里云 | [使用帮助](https://developer.aliyun.com/mirror/goproxy) |  |

### CTAN

| 镜像站 | 帮助页 | 是否在用 |
| :------: | :------: | :------: |
| 清华 | [使用帮助](https://mirror.tuna.tsinghua.edu.cn/help/CTAN/) | 是 |

### Docker CE

| 镜像站 | 帮助页 | 是否在用 |
| :------: | :------: | :------: |
| 清华 | [使用帮助](https://mirror.tuna.tsinghua.edu.cn/help/docker-ce/) | 是 |
| 中科大 | [使用帮助](http://mirrors.ustc.edu.cn/help/docker-ce.html) |  |
