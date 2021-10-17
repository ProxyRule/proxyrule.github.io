---
title: Windows 右键「打开方式」
date: 2020-01-04 17:39:38
tags:
  - Windows
---

虽然标题是右键「打开方式」，但可能会把近期碰到的问题、发现、猜想都揉在一起。

<!-- more -->

### 右键中的「打开方式」

之前通过 Scoop 安装了 vscode，用 vscode 打开过 `.py` 文件，而后又把 vscode 卸载了。我发现，当你右键单击 `.py` 文件选择打开方式时，会发现残存的东西（如下图），不用说肯定又和注册表有关。

![open-with](/images/2001/open-with.png)

打开注册表编辑器，搜索 `C:\Users\Zheng\scoop\apps\vscode\current\Code.exe` 在路径 `计算机\HKEY_CLASSES_ROOT\py_auto_file\shell\open\command` 下发现了相关注册表值，将其删除，问题似乎解决，在这篇[文章](https://www.lmdouble.com/1820352339.html)中也得到印证。但是，这个问题是否真的解决了呢，有没有其他的注册表残留项。问题的关键在于，我对 Windows 注册表不了解，不了解它的文件结构和各部分功能，下次遇到其他问题又得像瞎子一样乱撞，在网络上疯狂查找资料。所以，得了解有关注册表的知识，包括设计思路、演变发展、各部分功能等。

### 从 CMD 打开文件夹

在解决上述问题的过程中，还学习到了一点，从 CMD 快速打开一些特殊文件夹。

假设说，你当前的 CMD Prompt 是在桌面，即 `C:\Users\Zheng\Desktop>`，那么

`start .` 可以快速打开当前所在目录，`start ..` 打开的则是用户目录，`.` 和 `..` 和 Linux 中是一致的。

类推，`start %appdata%` 打开的便是用户程序安装目录，`start shell:startup` 打开的是「用户启动文件夹」，等等。

### 关于软件安装位置的困惑

以我 64 位的 Windows 10 系统来说，软件可能安装在如下位置：

- `C:/Program Files` 和 `C:/Program Files (x86)`
- `C:/Users/<username>/AppData`
- `C:/ProgramData`

这些安装位置的不同跟权限和系统架构有着直接关系。很多软件现在都默认安装在用户文件夹下，因为可以实现静默安装，尤其是一些流氓软件。此外，像上面后两个目录默认是隐藏状态的，一般人可能还不容易找到，更别谈修改删除，当然可能这部分（甚至是大部分）用户也不会想要去干点什么。

### Scoop 迁移及重装后恢复使用

根据 GitHub issue 「[How to use scoop after reinstalling the system](https://github.com/lukesampson/scoop/issues/2894)」 及参考该[文章](https://jiayaoo3o.github.io/2019/03/19/%E9%87%8D%E8%A3%85%E7%B3%BB%E7%BB%9F%E5%90%8E%E5%A6%82%E4%BD%95%E6%81%A2%E5%A4%8D%E4%BD%BF%E7%94%A8scoop/)（文章作者即为上述 issue 的提出者），步骤如下：

1. 将 Scoop 安装目录完整保存至他处

2. 将上述保存的文件夹放置在目标处

3. 在用户环境变量中，新建一个 SCOOP 变量，值为第二步中 scoop 文件夹地址，如 `D:\Scoop`

4. 在用户变量 Path 中新增一条，值为第二步中 scoop 文件夹下 shims 文件夹地址，如 `D:\Scoop\shims`

5. 允许脚本执行：`set-executionpolicy remotesigned -s currentuser`

6. `scoop reset *` 等待 scoop 重置完

### Python 3.8.1 下 Jupyter Notebook 启动报错

原本打算将 Python 转到也使用 Scoop 进行安装，安装完后发现 Python 版本为 3.8.1 并且启动不了 Jupyter Notebook。在网上查到，[这一问题](https://stackoverflow.com/questions/58422817/jupyter-notebook-with-python-3-8-notimplementederror)跟 tornado 有关并且知道了解决办法。

但是呢，我不想这样做，于是尝试通过 `scoop install python37` 安装 3.7.6 版本的 Python，安装完后 `scoop reset python37` 但是 `python --version` 依然显示 3.8.1。几番尝试之后，无果，作罢，不知道是我的姿势有问题，还是说 `scoop reset` 目前只支持在 2.7 和 3.x 版本之间切换而不支持 3.7.6 与 3.8.1 版本间的切换？最后，还是采用手动的方式安装 Python 3.7.6，问题解决。Python 和 R 也是为数不多的没有通过 Scoop 进行安装的软件。
