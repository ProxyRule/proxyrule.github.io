---
title: 如何让应用在进入桌面前启动
date: 2020-01-17 17:54:29
tags:
  - Windows
  - 软件
---

Windows 10 上安装了 [Fences](https://www.stardock.com/products/fences/) 用来管理桌面上一些常用文件（夹），虽然它被设置为开机自启，但是在进入桌面后很长一段时间内它还处于启动状态，桌面文件不能迅速加载出来。受[如何调整 Windows 10 软件开机启动顺序](https://meta.appinn.net/t/windows-10-windows-10/13337)启发，将这一过程记录下来，同时加入了前文中未提到的一些细节、注意事项以及如何卸载。

<!-- more -->

### 安装过程

首先，给出下面会使用到的工具的 repo：[sylveon/EarlyStart](https://github.com/sylveon/EarlyStart)

1. 到 [release page](https://github.com/sylveon/EarlyStart/releases) 下载，我此时使用的是 [1.0.0 版本](https://github.com/sylveon/EarlyStart/releases/download/1.0.0/EarlyStart.zip)。下载完后解压，并将文件夹放置在一个固定位置，比如我的是 `D:\Code\EarlyStart`

2. 右键选中 `EarlyStart.exe`，在属性中`常规 - 安全`下取消勾选。如果和我一样，没有这个复选框的话，那就不用管。更多可参考：[Unblock the main executable](https://www.tenforums.com/tutorials/5357-unblock-file-windows-10-a.html#option1)

3. 以管理员身份运行 CMD，进入第一步中的文件夹路径。如果你和我一样也将其放置在非系统盘，那么需要加 `/d` 参数才行，比如我的是 `cd /d D:\Code\EarlyStart`

4. 接着运行 `%windir%\Microsoft.NET\Framework\v4.0.30319\InstallUtil.exe .\EarlyStart.exe`，应该会有`成功完成`之类的字样，可以关闭 CMD 了

![EarlyStart](/images/2001/EarlyStart.png)

5. 在用户文件夹下，即 `C:\Users\<username>` 下新建一个 `.earlystart` 文件，然后将想要 earlystart 的应用程序路径每行写入一个。比如我的是 `"C:\Program Files (x86)\Stardock\Fences\Fences.exe"`

<mark>注意</mark>：如果你写入上述文件的某一行程序路径无效的话，那么在这之后指定的任何程序都不会被 earlystart

6. 重启观察是否有变化

### 如何卸载

1. 以管理员身份运行 CMD，进入上述第一步的文件夹内

2. 运行 `%windir%\Microsoft.NET\Framework\v4.0.30319\InstallUtil.exe /u .\EarlyStart.exe` 提示`成功`之类的字样就 OK 了

3. 删除 `.earlystart` 文件，如果喜欢的话
