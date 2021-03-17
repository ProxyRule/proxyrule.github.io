---
title: Excel 2016 打开空白
date: 2019-12-27 20:00:55
tags:
  - Office
---

如题，有时候莫名其妙 Excel 直接双击打开文件一片空白。其实之前也遇到过，在网上找了相关资料暂时解决了，但是今天又遇到了，尝试了另一种方法成功解决，特此记录。

<!-- more -->

查到的解决方法中，最多的是修改注册表。已知的需要修改以下三处：

```text
HKEY_CLASSES_ROOT\Excel.csv\shell\Open\command
HKEY_CLASSES_ROOT\Excel.Sheet.12\shell\Open\command
HKEY_CLASSES_ROOT\Excel.Sheet.8\shell\Open\command
```

修改前看到的值应该是 `"C:\Program Files\Microsoft Office\Root\Office16\EXCEL.EXE" "/DDE"`，需将其修改为 `"C:\Program Files\Microsoft Office\Root\Office16\EXCEL.EXE" "%1"`。

但不确定除这三处外其他地方是否也需要修改。所以，如果你对如何修改注册表不了解的话，可以采取「修复 Office 应用程序」这种更简单稳妥的方式。

如果是 Windows 10 的话，流程如下：<kbd>Start</kbd> + <kbd>I</kbd> 打开「Windows 设置」，找到「应用」，然后在「应用和功能」中找到安装的 Office，点击「修改」，选择「联机修复」即可。更多详情可查看：[修复 Office 应用程序](https://support.office.com/zh-cn/article/%E4%BF%AE%E5%A4%8D-office-%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F-7821d4b6-7c1d-4205-aa0e-a6b40c5bb88b)
