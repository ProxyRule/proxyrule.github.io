---
title: Scoop 不完全上手指南
date: 2020-01-03 13:11:06
tags:
  - Scoop
  - 软件推荐
  - Windows
---

{% note info %}
以下基本上都是对 [Wiki](https://github.com/lukesampson/scoop/wiki) 的整理，调整顺序的同时加入一些自己的理解，无语言障碍可直接到 GitHub 或[官方文档](https://scoop.netlify.com/)阅读。
{% endnote %}

<!-- more -->

### Scoop 是什么

借用 Mike Zick 对 Cygwin 和 MSYS 的[描述](http://sourceforge.net/mailarchive/forum.php?thread_name=200506130821.11185.mszick%40morethan.org&forum_name=mingw-msys)，他对 Scoop 作了一个类比描述：

> Scoop is an installer
>
> The goal of Scoop is to let you use Unix-y programs in a normal Windows environment

并且他也称，Scoop 并不是一个包管理器，而是通过读取 JSON 描述文件来安装程序及其依赖。Scoop 专注于开源和命令行开发工具，不符合其标准的不可能进入 main bucket（Scoop 安装后便自带的），因而虽然通过 `scoop install skype` 也能安装 Skype，但是只能放在 extra bucket 中。

在与 [Chocolatey](https://chocolatey.org/) 对比时，他提到了 Scoop 的一些特性，其中不乏吸引我选择使用它而非前者的因素。

1. Scoop 默认安装在用户文件夹下（`~/scoop/`），那么在权限方面就很友好，安装程序时不会跳出 UAC 提醒，不需要管理员权限。

2. 不会对路径造成污染。像是平常手动安装以及通过 Chocolatey 安装程序时，安装目录散落各地，有在 `C:/Program Files` 和 `C:/Program Files (x86)` 的，也有在 `C:/Users/<username>/AppData` 的，还有在 `C:/ProgramData` 的。其实这些安装位置都是跟 "install for all users" 和 "install only for me" 的区别有关的，背后对应的是不同的权限（我瞎说的），但是看上去非常乱也不好管理。Scoop 则是将程序的 shims（我理解为指向所安装软件当前版本的快捷方式，非科班的我面对这些术语流下了眼泪）集中放在一个文件夹中统一管理，并将其添加至环境变量。

3. 相比包管理器和应用仓库更简单（simpler）。使用 Scoop 最简单的形式只需 Git + JSON 就够了，通过 Git 读取同步 repo 中描述如何安装某个程序的文件（里面写明了程序的版本、下载地址、解压目录、bin 及安装前后的工作等），然后 `scoop install <app>` 完事。

4. 可安装程序的某个特定版本并可以在版本间切换。如 `scoop install python27` 便可以安装 Python 2.7 版本（当然得先通过 `scoop bucket add versions` 添加 versions bucket），同时 `scoop install python` 安装的则是 Python 的最新版本。

### 安装 Scoop

其实安装之前，应该先将 Scoop 中的几个重要概念讲清楚的，比如上面多次提到的 bucket。但是，既然是上手指南，实用为先，概念可以暂先这样理解：所谓 app 就是要安装的一个应用程序，app manifest 则是含有某个应用程序安装信息（如上所述，程序版本、下载地址等）的 JSON 文件，bucket 则是存放这些 manifest 的 repo（如托管在 GitHub 上的 [main bucket](https://github.com/ScoopInstaller/Main/tree/master/bucket)）。

首先，唤出 PowerShell（Windows 10 下都可以），`set-executionpolicy remotesigned -scope currentuser`，然后选择允许（Y）执行本地脚本。

- 如果是想安装在默认位置，即 `C:/Users/<username>/scoop` 的话，直接运行

```powershell
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')
```

或者 `iwr -useb get.scoop.sh | iex` 即可。

- 如果是想自定义安装位置，如 `D:/Scoop`，那么逐条运行下面命令

```powershell
$env:SCOOP='D:\Scoop'
[environment]::setEnvironmentVariable('SCOOP',$env:SCOOP,'User')
iwr -useb get.scoop.sh | iex
```

至于如何安装全局应用到自定义目录就先不说了。至此，如无报错信息，Scoop 安装完成。

### 卸载 Scoop

卸载非常简单，只需运行 `scoop uninstall scoop` 即可。

### 使用 Scoop

记得随时使用 `scoop help` 查看帮助信息

#### scoop search

查找软件，通常是想看看某个程序是否可以通过 Scoop 安装

```powershell
scoop search <app>
```

如

```powershell
scoop search python
```

#### scoop install

安装应用程序，分两种情况：

- 只为当前用户安装，安装在 Scoop 目录下的 apps 文件夹

```powershell
scoop install <app>
```

如

```powershell
scoop install python
```

- 为所有用户安装，默认安装在 `C:/ProgramData/scoop` 或者是上文提到的自定义的全局应用安装目录，并且需要以管理员身份运行

```powershell
scoop install <app> -g
```

如

```powershell
scoop install python -g
```

如果要安装特定版本的应用，比如说 `curl 7.56.1`，则应该这样

```powershell
scoop install curl@7.56.1
```

#### scoop uninstall

卸载某一程序

```powershell
scoop uninstall <app>
```

如

```powershell
scoop uninstall python
```

卸载程序并移除所有配置文件

```powershell
scoop uninstall <app> -p
```

如

```powershell
scoop uninstall python -p
```

卸载全局安装的应用程序，需以管理员身份运行

```powershell
scoop uninstall <app> -g
```

如

```powershell
scoop uninstall python -g
```

#### scoop update

更新 Scoop 及所有 bucket 但不更新 app

```powershell
scoop update
```

更新某一特定程序

```powershell
scoop update <app>
```

如

```powershell
scoop update python
```

更新 Scoop、bucket 及所有程序

```powershell
scoop update *
```

更新全局安装的程序，需要以管理员身份运行

```powershell
scoop update <app> -g
```

如

```powershell
scoop update python -g
```

#### scoop list

查看已安装的程序

```powershell
scoop list
```

#### scoop status

查看哪些程序可以升级

```powershell
scoop status
```

#### scoop config

需要设置的一般也就是两个，aria2 开关以及 proxy 设置

开闭 aria2 `scoop config aria2-enabled true` or `scoop config aria2-enabled false`，但不建议开启，经常有各种奇奇怪怪的问题。同时，启用 aria2 前需要先安装 `scoop install aria2`

proxy 设置，如 `scoop config proxy 127.0.0.1:1080`

#### scoop home

查看某一程序的主页

```powershell
scoop home <app>
```

如

```powershell
scoop home python
```

便唤起浏览器，打开 Python [官网](https://www.python.org/)

#### scoop reset

借用 Wiki [例子](https://github.com/lukesampson/scoop/wiki/Switching-Ruby-And-Python-Versions)

```powershell
# 先添加 versions bucket
scoop bucket add versions

# 同时安装 Python 2.7 和最新版本
scoop install python27 python

# 切换到 Python 2.7.x
scoop reset python27

# 切换到 Python 3.x
scoop reset python
```

#### scoop cleanup

删除已安装软件的旧版本，如删除所有软件旧版本

```powershell
scoop cleanup *
```

#### scoop cache

清理软件缓存，通常是下载的软件安装包。以下命令清除所有缓存，即清空 Scoop 目录下的 cache 文件夹

```powershell
scoop cache rm *
```

#### scoop bucket

查看「已知库」

```powershell
scoop bucket known
```

查看已经添加的库

```powershell
scoop bucket list
```

删除已经添加的库

```powershell
scoop bucket rm <bucket>
```

添加库，分两种情况：

- 添加「已知库」

```powershell
scoop bucket add <bucket>
```

如添加上文提到的 versions 库

```powershell
scoop bucket add versions
```

- 添加第三方库

```powershell
scoop bucket add <bucket> <bucket_url>
```

如添加 Ash258、chawyehsu 和我的库

```powershell
scoop bucket add Ash258 https://github.com/Ash258/Scoop-Ash258.git
scoop bucket add dorado https://github.com/chawyehsu/dorado.git
scoop bucket add spoon https://github.com/FDUZS/spoon.git
```

### Scoop 进阶

看完以上内容，入门足够。我也刚使用不到半年，认为进阶需要搞懂以下几点：

1. App Manifest 创建并可以实现「自动更新」

2. "Current" 文件夹及背后设计思路

3. 找出其设计缺陷之处，即让你不爽的点
