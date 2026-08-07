---
title: Android Xposed 框架指南
categories: Android
abbrlink: Android-Xposed-Framework-Guide
date: 2020-06-02 00:00:00
tags:
---

![](topic.jpg)

Xposed 框架的安装与模块使用。

<!-- more -->

# 安装

## 已 ROOT 方法

Xposed Installer 下载链接如下。注意资源需要翻墙才能下载。

```
https://forum.xda-developers.com/t/official-xposed-for-lollipop-marshmallow-nougat-oreo-v90-beta3-2018-01-29.3034811/
```

由于 APP 资源地址由 http 更改为 https，而 Xposed Installer 中并未更改，因此安装后会出现`could not load available zip files`错误，刷新下载页面时会出现`下载http://dl.xposed.info/repo.xml.gz时出错`的提示。这需要在安装前修改 APP 包的资源地址解决。

打开 MT 文件管理器，选择 Xposed Installer 的 APK 安装包，点击查看。选择 classes.dex，用 Arsc 编辑器打开，在字符串页面搜索`http`，将所有搜索结果的`http://`都改为`https://`，保存并退出编辑器。选择刚才修改好的 APK，点击自动签名，完成后点击安装即可。

若仍出现`could not load available zip files`错误，可提前下载 Xposed 的 SDK 包，然后放置到 Android/data/de.robv.android.xposed.installer/cache/downloads/framework 下，手机对应的 SDK 版本可在 Xposed Installer 主界面看到。下载链接如下。

```
https://dl-xda.xposed.info/framework/
```

如果仍未出现，需要点击右上角的横杠符号，勾选 Show outdated version，才会出现 Xposed 版本。

找到下载包后，点击云朵符号，选择 Install 即可。

## 免 ROOT 方法

可使用模拟框架。

```
# 太极
https://taichi.cool/zh/download.html

# Virtual Xposed
https://vxposed.com/

# 应用转生
https://lanzoui.com/b05xrjlbc
```

也可使用虚拟机 VMOS Pro，打开并安装一个支持 Xposed 的安卓 ROM，然后点击系统-设置，打开 Xposed 即可。

也可使用将 Xposed 模块嵌入 APP 的方式。

```
# LSPatch
# 得到的 APP 已经嵌入模块，直接使用即可
https://github.com/LSPosed/LSPatch

# Xpatch
# 需要安装得到的 APP 和 Xposed 模块
https://github.com/WindySha/Xpatch
```

# 模块

在下载页面即可下载模块。也可通过以下网站下载。

```
https://repo.xposed.info/module-overview
```

一些模块如下。

```
# 去广告
https://repo.xposed.info/module/tw.fatminmin.xposed.minminguard
https://www.coolapk.com/apk/com.dahuo.sunflower.xp.none
https://repo.xposed.info/module/com.gy.xposed.skip
```

# 参考教程

> [Xposed下载模块89 repo.xml.gz失败的解决方法](https://www.52pojie.cn/forum.php?mod=viewthread&tid=1461477)  
> [教你如何简单高效的对软件(apk文件)进行修改](https://www.juyifx.cn/article/291723297.html?ivk_sa=1024320u)  
> [daltonfury42 - Github gist](https://gist.githubusercontent.com/daltonfury42/c33fdfa7a44f261018a5d35dea7eb245/raw/5fc372ec0d36117fa3e7698d8de1952c1bac6b6a/platina.xml)  
> [抱歉，安卓有了Xposed真的可以为所欲为！](https://mp.weixin.qq.com/s/n7Rqn6e8zQyvghvSRJ61XA)  
> [安卓神器的天花板Xposed，这次真的能让你用上！](https://mp.weixin.qq.com/s/V7mK6ahKN8rLOkvHExUpCA)  
