---
title: Scoop使用指南
categories: Skill
abbrlink: Instructions-Of-Scoop
date: 2020-04-27 08:54:29
tags:
---

![](topic.jpg)

类似Homebrew的软件管理器。

<!-- more -->

# 使用

打开PowerShell并运行以下命令以执行安装。

```
# 将PowerShell设置为可运行任意脚本
set-executionpolicy Unrestricted

# 下载脚本
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')
// 或
iwr -useb get.scoop.sh | iex
```

执行以下命令安装aria2以提升下载速度。

```
scoop install aria2
```

常用命令如下。

|                  命令                  |         说明         |
|----------------------------------------|----------------------|
| scoop help                             | 帮助                 |
| scoop install [软件名]                 | 安装软件             |
| scoop install [软件名]@[版本号]        | 安装制定版本软件     |
| scoop install -g [软件名]              | global目录下安装软件 |
| scoop bucket add [软件库名] [源地址]   | 添加软件库           |
| scoop update                           | 更新scoop和软件库    |
| scoop update *                         | 更新所有已安装应用   |
| scoop cleanup *                        | 移除所有旧版本       |
| scoop uninstall [软件名]               | 卸载                 |
| scoop list > F:/scoop/ScoopAppList.txt | 导出软件列表         |
| scoop reset [软件名]                   | 版本切换             |

软件库推荐如下。

```
scoop bucket add main // 默认
scoop bucket add extras // 推荐
scoop bucket add versions
scoop bucket add nightlies
scoop bucket add nirsoft
scoop bucket add php
scoop bucket add nerd-fonts
scoop bucket add nonportable
scoop bucket add java
scoop bucket add games
scoop bucket add jetbrains // 推荐
scoop bucket add echo https://github.com/echoiron/echo-scoop
scoop bucket add dorado https://github.com/chawyehsu/dorado
scoop bucket add dodorz https://github.com/dodorz/scoop-bucket
```

# 参考教程

## Scoop软件包管理神器

```
https://www.limufang.com/post/569.html
```


