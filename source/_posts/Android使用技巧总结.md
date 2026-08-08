---
title: Android 使用技巧总结
categories: Android
abbrlink: Android-Skills
date: 2020-06-04 00:00:00
tags:
---

![](topic.jpg)

安卓使用技巧。

<!-- more -->

# 自动化操作

## Hamibot

Hamibot 需要保持在线才能运行，因此需要给 Hamibot 自启动权限，同时关闭省电策略，防止 APP 在后台被清理。

```
https://hamibot.com/
```

对于自身不能自动运行的脚本，可通过创建自动任务的方式实现自动运行。

## 一触即发

```
https://www.coolapk.com/apk/com.yicu.yichujifa
```

可自行编辑脚本。示例脚本中模拟点击的屏幕坐标在不同手机上可能会不同，因此需要自行修改。保持在脚本的编辑页面，切换需要点击的 APP，将准心移动到需要点击的位置，然后点一下准心，选择相应的动作即可。

## 自动精灵

通过录制创建脚本。

```
https://www.coolapk.com/apk/com.zdanjian.zdanjian
```

# 软件资源

## 破解软件库

```
https://590m.com/dir/845023-30099335-9e922b
https://306t.com/dir/14633726-31244416-26771f
https://www.uptodown.com/android
```

## 微信

### 抢红包插件

```
https://github.com/geeeeeeeeek/WeChatLuckyMoney
```

### 防撤回

原理是将通知栏的消息保存下来。

```
# 通知增强 for 微信
https://www.downkuai.com/android/115784.html

# Anti-recall
https://anti-recall.com/
```

## WPS Pro

破解版下载链接如下。

```
https://wwc.lanzouy.com/b03j3cbyd（密码 / 0000）
```

激活码如下。

```
R8R8P-MTT6F-KLRPM-J7CAB-PJM8C
7LR67-WTXPA-KLUHV-GEK2E-QW4CK
EUYTH-3KWKL-PJMX7-XBCPW-9U2DD
THUV2-32HH7-6NMHN-PTX7Y-QQCTH
A4XV7-QP9JN-E7FCB-VQFRD-4NLKC
U2PWU-H7D9H-69T3B-JEYC2-3R2NG
7G2HE-JR8KL-ABB9D-Y7789-GLNFL
```

## Google框架

部分手机如 Samsung 国行版只是将 Google Play 隐藏了起来，重新安装相关 APP 即可。Google Play 下载地址如下。

```
https://m.apkpure.com/google-play-store/com.android.vending
```

## 游戏库

```
https://wwc.lanzouy.com/b03j3vpgd（密码 / 0000）
```

## 游戏模拟器

```
# 爱吾游戏宝盒
https://www.25game.com/

# 悟饭游戏厅
https://www.5fun.com/

# 小马模拟器
http://www.ponyemu.com/

# 约战
https://www.yzkof.com/

# 游聚
https://www.gotvg.com/
```

## 应用市场

### 国际版 APP 应用市场

可用`9Apps`、`V-Appstore`、`F-Droid`。F-Droid 可用以下镜像加速。

```
https://mirrors.tuna.tsinghua.edu.cn/help/fdroid/
```

# 常用工具与资源

## 常用工具

### Gboard

好用的输入法，在 Google Play 即可下载。

由于下载简体中文语言包需要一段时间，一开始使用拼音时会出现无法弹出中文选字的情况，挂翻墙并等待下载完成即可。

## 连点器的使用

导入规则：

```
7687
0499
2650
```

要把手机的屏幕防护关掉，否则无法识别文字。

然后点一下加载，会出来很多按钮。以盲盒到店取为例：
- 01是按送到家
- 02是按到店取
- 03是识别确定并点击
- 04是确认信息并支付
- 05是确认无误
- 06是跳转回03

要把每个按钮的位置移动对。对于识别文字的按钮（03/04），需要长按跳出设置，并点击识别区域下的选择区域，重新选择文字按钮。然后改完之后点规则，保存一下规则。

01和02之间刷新频率为100-200ms随机，不会被检测为点击太快。02和03要稍微慢一些，不然按钮加载不出来，用300ms左右差不多。

然后在刚才的规则上点多开，就出来按钮了，直接用就行。

## APK安装器——SAI

```
https://github.com/Aefyr/SAI
```

## API Levels

```
https://apilevels.com/
```

## Shizuku

```
https://sspai.com/post/73294
https://shizuku.rikka.app/zh-hans/
https://www.xda-developers.com/shizuku/
https://forum.xda-developers.com/t/exploit-shizuku-support-smt-shell-v2-0-get-a-system-shell-uid-1000-within-the-app-itself-and-write-your-own-system-app-with-an-api.4561879/
https://www.reddit.com/r/fossdroid/comments/y8ewgf/a_list_of_apps_that_utilize_shizuku_for_elevated/
https://github.com/BLuFeNiX/SMTShell
```

## Fdroid

```
https://depau.github.io/fdroid_shizuku_privileged_extension/fdroid/repo/
https://f-droid.org/packages/de.marmaro.krt.ffupdater/
https://gitlab.com/sunilpaulmathew/izzyondroid
https://github.com/redsolver/skydroid
```

## Google Play版微信

```
https://www.coolapk.com/feed/45383246?shareKey=YThmNjhlNzAwMzZiNjQ0MzY5MTg~&shareFrom=com.coolapk.market_12.5.0
```

Samsung J3（armv7 架构）要用微信 8.0.2。可以在这里下载（在官网下的 armv7 装不了）：

```
https://www.apkmirror.com/apk/wechat-tencent/wechat/wechat-7-0-10-release/wechat-7-0-10-android-apk-download/
```

## Google Camera

```
https://www.bilibili.com/read/cv8122208
https://tecnoandroid.net/zh-TW/gcam-google-camera-xiaomi-huawei-samsung-oneplus-realme-redmi/
https://forum.xda-developers.com/t/downgrade-android-8-to-7-binary-3.3891802/
```

armv7 可以用的版本（不过会闪退，闪退的话要更换维护版本）：

```
https://www.apkmirror.com/apk/google-inc/camera/camera-4-2-035-141213305-release/google-camera-4-2-035-141213305-2-android-apk-download/download/?key=2997286a3ea65b09c97e3be9755b361bde73e4e8
```

# 参考教程

> [自动微信好友检测、收能量、签到打卡，不过是这款APP的冰山一角！](https://mp.weixin.qq.com/s/nX9oY6abpKLVk1IA_r8G2g)  
> [一个APP安全实现微信防撤回，iOS也行？这也太强了吧！](https://mp.weixin.qq.com/s/w1oxB0BnhQkyhI1tW_2f6g)  
> [大厂出品却冷门10年，免翻不限速下载国际版APP的绝佳选择！](https://mp.weixin.qq.com/s/8xc2uRZP2EboZz3wnR74Zw)  
