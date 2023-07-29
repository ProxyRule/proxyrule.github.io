---
title: 删除 Windows 10 桌面右键菜单中的图形选项和图形属性选项
date: 2023-07-29 11:45:29
tags:
    - Windows
---

我的笔记本电脑经常在集成显卡驱动更新后，点击鼠标右键要等很长时间才能弹出菜单来。作为强迫症自然不能忍受，网络搜索得到答案后，写在这作为记录。

<!-- more -->

1. 按下 <kbd>Start</kbd> + <kbd>R</kbd> 调出运行，输入 `regedit` 回车

2. 在注册表编辑器中定位到

```text
HKEY_CLASSES_ROOT\Directory\Background\shellex\ContextMenuHandlers
```

3. 删除 `igfxDTCM` 和 `igfxcui` 两个项目
