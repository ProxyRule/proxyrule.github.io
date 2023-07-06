---
title: 重装系统的成本及前后工作
date: 2020-01-17 22:31:07
tags:
  - Windows
  - 日常
---

这次重装系统（为何要重装有兴趣的可翻看上上篇）我是一万个不愿意的，因为我知道系统重装的成本有多大。就单纯装个系统来说可能只需要十几分钟，但是装系统前的准备工作（主要是备份和制作安装启动 U 盘）以及系统装完后的恢复备份、软件安装调试、系统设置等，要想完全恢复之前的使用环境，差不多得需要一天的时间。现在是晚上的 10:42，先记录下该做的一些工作，按步骤来就不会出错。

<!-- more -->

### 准备工作

#### 安装 U 盘

使用 [Windows Media Creation Tool](https://www.microsoft.com/zh-cn/software-download/windows10) 制作安装 U 盘 ---> 完成

#### 文件

备份`桌面`文件。右键选中`桌面`文件夹，在属性中将文件移动到其他盘 ---> 未完成

备份`文档`文件，参照上述方法 ---> 未完成

用户目录下的一些 dotfile 如 ssh 密钥、gpg 密钥和配置文件等 ---> 未完成

#### 驱动

从官网下载驱动 ---> 完成

#### 统计安装软件

按上次重装系统后软件安装顺序排序如下：

✅ 已安装 - 谷歌浏览器 ---> 下载万物，当然得先用 IE/Edge 下载它

✅ 已安装 - 微信

✅ 已安装 - 坚果云 ---> 论文同步

❌ 未安装 - Zoommy ---> 貌似万年没打开过了

✅ 已安装 - TIM

✅ 已安装 - EndNote X8 ---> 文献管理，白嫖的中科大授权，严格意义上的盗版

✅ 已安装 - Tableau ---> 向官方申请的教育授权，本地反激活完成

✅ 已安装 - 火狐浏览器

✅ 已安装 - JetBrains Toolbox ---> 向官方申请的教育授权，无需反激活

✅ 已安装 - 1Password ---> 别忘记主密码

✅ 已安装 - R

✅ 已安装 - RStudio

❌ 未安装 - Fences ---> 购买的正版密钥，本地反激活完成

✅ 已安装 - Listary ---> 购买的正版密钥，无法/不需反激活

✅ 已安装 - Internet Download Manager ---> 购买的正版密钥，无法/不需反激活

✅ 已安装 - Notion ---> 通过 Scoop 安装

✅ 已安装 - MATLAB ---> 学校购买授权，帐号登录

✅ 已安装 - Navicat ---> 学校邮箱申请密钥,反激活完成

✅ 已安装 - Axure ---> 教育授权，本地反激活完成

✅ 已安装 - MS Office 家庭和学生版 2016 ---> 购买机器附送的，绑定账号

✅ 已安装 - 幕布 ---> 通过 Scoop 安装

✅ 已安装 - 印象笔记 ---> 登录国内和国际版两个账号

✅ 已安装 - MindManager

✅ 已安装 - PicGo ---> 通过 Scoop 安装

✅ 已安装 - MS Visio 专业版 2019 ---> 购买，绑定账号

✅ 已安装 - WinRAR

✅ 已安装 - Python 3.7 ---> 不要装 3.8，无法正常运行 Jupyter Notebook

❌ 未安装 - XMeters ---> 购买的正版密钥，无法/不需反激活 ---> 先不装了

✅ 已安装 - PotPlayer ---> 竟然被 Scoop extras bucket 收录了

✅ 已安装 - ManicTime ---> 购买的正版密钥，本地反激活完成

✅ 已安装 - Rtools

✅ 已安装 - Spotify

✅ 已安装 - TeX Live ---> 配置文件还原

### 安装系统

最简单，略过，选项该关闭该保留全凭个人喜好

不过有一点要记住，安装时先创建一个本地账户而不要登录微软账号，否则用户文件夹名会恶心你

### 恢复工作

- 驱动安装 ---> 可选，看看官网有没有适配最新系统的驱动发布

- 备份文件恢复 ---> 移动文件

- 安装软件 ---> 无尽的下载……下载……载……想到 MATLAB 和 TeX Live 就头疼

- 环境变量设置 ---> 绝大部分交由 Scoop 完成，剩余的软件安装过程中就可做到

- 自定义 DNS ---> 可选

- 一些软件设置，如语言工具设置国内镜像源

现在已经凌晨 12:18 了，明天继续。

### 实际安装步骤及观察

1. 安装完系统后，联网，等待一段时间，自动安装驱动及补丁

2. 修改设备名称后，重启

3. 通过蓝牙连接鼠标和键盘

4. 解决英文操作系统下中文乱码问题：`Control Panel - Clock and Region - Region - Administrative - Language for non-Unicode programs`，将其改为简体中文即可

5. 登录微软账号同步设置和 OneDrive 文件，先进行简单设置和覆盖原有设置，如去掉固定在任务栏的快捷图标，将默认输入语言设置为中文、默认应用语言和系统显示语言设置为英文

6. 安装 Scoop，参考的是之前写的 [Scoop 不完全上手指南](https://www.zs.fyi/archives/scoop-guidebook.html)。安装完后，用户文件夹下多了一个 `.config` 文件夹，里面有 Scoop 的配置文件

7. 先通过 Scoop 安装 shadowsocksr-csharp，前提是添加 extras bucket，而添加 extras bucket 的前提是安装 git 和 7zip。这一过程是异常痛苦的，网络环境并不太好，只能看着下载进度条慢慢地走……

8. 垃圾小米笔记本，安装 git 时自动休眠睡死了，转向处理这一问题的 bug 分支。

9. ~~初步判断是集成显卡驱动问题~~并不是，可能跟主板芯片和电池管理驱动有关，所以老老实实把所有驱动打了一遍。为了加快驱动下载速度，安装 Internet Download Manager，而密钥存储在 1Password 中，先下载安装 1P 。

10. 安装 git 后，将原来的 `.gitconfig` 文件复制到用户目录下覆盖新生成的配置文件

11. 安装完谷歌浏览器后，登录账号同步设置及插件，复制机场的订阅地址到小飞机，开启飞行模式

12. 之前通过 Scoop 安装的一些软件虽然可以[无损迁移恢复使用](https://www.zs.fyi/archives/windows-open-with.html#Scoop-%E8%BF%81%E7%A7%BB%E5%8F%8A%E9%87%8D%E8%A3%85%E5%90%8E%E6%81%A2%E5%A4%8D%E4%BD%BF%E7%94%A8)，但最终还是决定全部重装一遍，完整记录下来。

13. Scoop install：adb | android-sdk | anki | annie | aria2 | autohotkey | blender | bluescreenview | captura | chromedriver | concfg | curl | dark | dropit | everything | ffmpeg | figlet | fork | geckodriver | geekuninstaller | gimp | gpg | graphviz | honeyview | hugo | inkscape | joplin | lessmsi | motrix | msys2 | neovim | nodejs-lts | nvm | openjdk13 | openshot | pandoc | pandownload | php | proxifier-portable | pshazz | racket | screentogif | sharex | sqlite | sudo | sumatrapdf | telegram | time | touch | v2ray | v2rayn | vnote | vscode | which | winscp | yarn | youtube-dl | zotero

14. go 改为手动安装

15. 将之前备份的文件还原回去

16. 之后就剩一些「大块头」的软件以及收尾工作

17. 最后就是系统设置

18. done
