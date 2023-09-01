---
title: Microsoft Visual C++ 14.0 or greater is required
date: 2020-09-19 23:43:22
tags:
  - Python
  - 环境配置
---

「error: Microsoft Visual C++ 14.0 or greater is required」是不是令你痛苦不堪、眉头紧皱？是不是在万般搜索后仍旧无果？是不是网上经验教训一大堆，但是都不能从根本上解决问题？那么，以下可能是你能找到的最全面准确的办法。

<!-- more -->

### Python 版本

以我正在使用的 `Python 3.7.6` 为例。

### Visual C++ 版本

与上面相对应，Visual C++ 版本应为 `14.x`。

为什么是 14.x 呢？这是因为目前有 14.0.X、14.16.X、14.27.x 三个版本，它们分别是 Visual Studio 2015，Visual Studio 2017，Visual Studio 2019 自带的编译工具。并且，根据官方的描述

> Visual C++ 2015, 2017 and 2019 all share the same redistributable files.
>
> For example, installing the Visual C++ 2019 redistributable will affect programs built with Visual C++ 2015 and 2017 also. However, installing the Visual C++ 2015 redistributable will not replace the newer versions of the files installed by the Visual C++ 2017 and 2019 redistributables.

你安装一个就行了。

以我安装的 `14.27.29016.0` 为例。

### 三者对应关系

<table>
<thead>
  <tr>
    <th colspan="2">Visual C++</th>
    <th>CPython</th>
    <th>Visual Studio</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td rowspan="3">14.x</td>
    <td>14.27.x</td>
    <td rowspan="3">3.5, 3.6, 3.7, 3.8</td>
    <td>2019</td>
  </tr>
  <tr>
    <td>14.16.x</td>
    <td>2017</td>
  </tr>
  <tr>
    <td>14.0.x</td>
    <td>2015</td>
  </tr>
  <tr>
    <td colspan="2">10</td>
    <td>3.3, 3.4</td>
    <td>2010</td>
  </tr>
  <tr>
    <td colspan="2">9</td>
    <td>2.6, 2.7, 3.0, 3.1, 3.2</td>
    <td>2008</td>
  </tr>
</tbody>
</table>

### 开始安装

<mark>注意</mark>：以 Python 3.7.6，只安装（不用费劲安装 Visual Studio 2019）Microsoft Visual C++ 14.2 为例。

1. `pip install --upgrade setuptools` 确保 setuptools 版本为 34.4.0 及以上

2. 到[下载页面][Download Visual Studio Tools]往下翻找到 Tools for Visual Studio，点开后下载最底下的 Build Tools for Visual Studio 2019。当然，你也可以直接到[这里][Microsoft C++ Build Tools]下载 Microsoft C++ Build Tools。点击运行后，应该会提示需要先下载一些文件

3. 勾选左侧最上方 C++ build tools，然后查看右侧，确保 MSVC v142 - VS 2019 C++ x64/x86 build tools 和 Windows 10 SDK 被选中。默认情况下应该不需要动，直接点击安装，等待完成即可

![VS-Build-Tools-2019](https://img.zs.fyi/2307/VS-Build-Tools-2019.png)

### 其他

- Python 官方文档：[Which Microsoft Visual C++ compiler to use with a specific Python version?]

- 各版本 Visual C++ 下载地址：[The latest supported Visual C++ downloads]

- Visual Studio 2017 及之前的版本下载地址：[Still want an older version?]

[Download Visual Studio Tools]: https://visualstudio.microsoft.com/downloads/
[Microsoft C++ Build Tools]: https://visualstudio.microsoft.com/visual-cpp-build-tools/
[Which Microsoft Visual C++ compiler to use with a specific Python version?]: https://wiki.python.org/moin/WindowsCompilers#Which_Microsoft_Visual_C.2B-.2B-_compiler_to_use_with_a_specific_Python_version_.3F
[The latest supported Visual C++ downloads]: https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads
[Still want an older version?]: https://visualstudio.microsoft.com/vs/older-downloads/
