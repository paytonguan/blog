---
title: iOS越狱方法
categories: iOS
abbrlink: iOS-Jailbreak-Method
date: 2023-11-10 13:33:29
tags:

---

![](topic.jpg)

iOS越狱方法。

<!-- more -->

# 相关知识

越狱工具适用范围如下。

有根越狱指的是拥有系统根目录权限，系统文件可以读取和写入。无根越狱指的是没有系统根目录权限，系统文件只能读取而无法写入。

完美越狱指的是设备重启后依旧是越狱状态。不完美越狱指的是设备重启后会恢复到未越狱环境，需要使用越狱工具重新激活环境。iOS 9.3以后均为不完美越狱。

|             应用            |    基板    | 设备要求 |                   系统范围                   | 有根/无根越狱 |
|-----------------------------|------------|----------|----------------------------------------------|---------------|
| Dopamine                    |            | A12-A15  | 15.0-15.4.1                                  | 无根          |
| XinaA15                     |            | A12-A15  | 15.0-15.1.1                                  | 无根          |
| SaiGon                      |            | A9-A11   | 15.0-15.8.2                                  | 无根          |
| Def1nit3lyN0tAJa1lbr3akTool |            | iPhone X | 15.7 & 16.5                                  | 无根          |
| NekoJB                      |            | A9-A11   | 15.0-15.7.6                                  |               |
| Palera1n                    |            | A8-A11   | 15.0-16.4.1                                  |               |
| Checkra1n                   | Substrate  | A7-A11   | 12.3-14+                                     |               |
| Taurine                     |            |          | 14+                                          |               |
| Odyssey                     |            | A9-A13   | 13.0-13.7                                    |               |
| Unc0ver                     | Substitute | A7-A13   | 12.0-12.2，12.4-14.3，14.3-14.5.1，14.6-14.8 |               |
| Chimera                     | Substitute | 64位设备 | 12.0-12.2，12.4                              |               |
| Electra                     | Substitute | 64位设备 | 11.0-11.4.1                                  |               |
| Th0r                        |            | 64位设备 | 11.2-11.3.1                                  |               |
| DoubleH3lix                 |            | 64位设备 | 10.0-10.3.3                                  |               |
| Meridian                    |            | 64位设备 | 10.0-10.3.3                                  |               |
| H3lix                       |            | 32位设备 | 10.0-10.3.3                                  |               |
| Yalu                        |            | 64位设备 | 10.1-10.2                                    |               |
| Phoenix                     |            | 32位设备 | 9.1-9.3.6                                    |               |
| kok3shi9                    |            | 64位设备 | 9.2-9.3.5                                    |               |
| 叉叉助手                    |            | 64位设备 | 9.2-9.3.3                                    |               |
| Jailbrea me                 |            | 32位设备 | 9.1-9.3.4                                    |               |
| wtfis                       |            | 64位设备 | 8.0-8.4.1                                    |               |
| EtasonJB                    |            | 32位设备 | 8.0-8.4.1                                    |               |
| Pangu                       |            |          | 7.0-7.1.2                                    |               |
| P0sixspwn                   |            |          | 6.0-6.1.6                                    |               |
| RedSn0w                     |            |          | 5.0-5.1.1                                    |               |
| Greenp0sion                 |            |          | 4.0-4.3.5                                    |               |

低版本的越狱可以用爱思助手。

各种系统版本的安装方法可参照以下链接。

```
https://cydia-app.com/
```

可在手机上打开以下链接安装各越狱工具。

```
https://apporanges.lanzoui.com/b0cfcmzwj
```

# Dopamine

## 应用安装

越狱后需要在Sileo安装Ellekit deb、PreferenceLoader-Rootless、AltList、libSandy、CCSupport、uikittools。

```
https://ellekit.space/dopamine/
https://github.com/opa334/Dopamine
```

## 清除越狱

打开Filza，进入Private/Preboot，打开以jb为名的文件夹，确认内部有一个名为Proucursus的文件夹，且该文件夹内部有fugu15max的相关文件。回到上一层并删除jb为名的文件夹。返回根目录，删除var/jb。

完成后打开TrollStore，进入Settings，点击Rebuild Icon Cache即可。

## 常见问题

```
https://mp.weixin.qq.com/s/5yzh14U7xC3Y0yVtVwdbIg
```

### 越狱后打开APP闪退

需要重启SpringBoard。打开TrollStore，进入Settings，点击Respring即可。

# XinaA15

## 应用安装

```
https://zhuxinlang.github.io/
https://xina.ss03.cn/
https://apt.xina.vip/
https://github.com/NotDarkn/XinaA15
```

## 应用商店

可使用Sileo-Xina。

```
# 官方版
https://github.com/Sileo/Sileo/releases/tag/xina-beta-5

# 修改版
# 全面汉化，支持安装插件后注销、全部忽略更新
https://github.com/HuChundong/Sileo
```

也可用Saily。

```
https://github.com/SailyTeam/Saily/releases
```

## 插件

### Xina清除35条更新

安装XinaA15后Sileo会出现35个更新。可通过安装该插件删除Procurs自带源以消除提示。

```
https://apt.ss03.cn/
```

### MilkyWay2

由于未添加iOS 15支持，需要安装MilkyWay2和MilkyWay2-iOS14Fix。或者安装lenglengyu源的版本即可。

```
http://www.lenglengyu.com/
```

### 更换字体

添加以下源，搜索Xina即可。

```
https://apt.ss03.cn/
```

## 使用

### 手动安装Deb插件包

打开XinaA15内置文件管理器，长按点击Deb安装即可。

## 常见问题

### 电脑端无法访问手机文件

安装Apple File Conduit"2"插件后爱思助手可能仍然无法访问手机文件。可以打开爱思助手，选择工具箱-SSH通道-SCP客户端，即可访问。

若需要在电脑端给手机安装插件，则将插件通过SCP客户端复制到手机端后，点击SCP客户端左上角的终端，输入以下命令即可。

```
dpkg -i [deb插件名称]
uicache —all
killall SpringBoard
```

### 安装应用级插件出现白图标

安装XinalConFix插件，然后重新安装XinaA15工具，激活越狱。

```
https://opa334.github.io/
```

### Sileo刷新源出现522错误

在Sileo越狱商店精选页面上，点击右上角头像，下拉找到缓存，直接点击两下，然后点击语音，开启使用系统语言，再点应用即可。

# SaiGon

```
https://github.com/H0aHuynh/SaiGon15x/releases
```

# Def1nit3lyN0tAJa1lbr3akTool

```
https://github.com/KpwnZ/Def1nit3lyN0tAJa1lbr3akTool
```

# NekoJB

```
https://nekojb.hhls.xyz/
```

# Unc0ver

Unc0ver版本与机型和系统的对应表格如下。

|       系统      |  支持机型 | 可用unc0ver版本 |
|-----------------|-----------|-----------------|
| iOS 13.0-13.7   | A9-A13    | 5.0.0-6.0.0     |
| iOS 14.0-14.3   | A9-A14    | 6.0.0-6.2.0     |
| iOS 14.4-14.5.1 | A12及以上 | 7.0.0-7.0.2     |
| iOS 14.6-14.8   | A12-A13   | 8.0.0-8.0.2     |


## 应用安装

### 普通安装

官网如下。

```
https://unc0ver.dev/
```

按照掉签IPA方法进行配置，然后打开以下链接进行安装。如果安装后无法打开，则尝试从电脑端通过AltStore自签。

```
https://www.lanzous.com/tp/icxyqaf
https://www.lanzous.com/icxyqaf?p
```

打开安装好的unc0ver并进行安装。越狱完成后打开Cydia并刷新，更新所有的依赖后安装PreferenceLoader、AppList、RocketBootstrap这三个插件。

注意，如果需要越狱的系统为14.3-14.5.1，且为iPhone XS（A12）及更新机型，则需要先安装AltStore，然后将unc0ver的安装包在手机上通过分享方式导入到AltStore进行自签。此时会利用Fugu14，使安装的unc0ver在重启后也能打开。

```
https://github.com/LinusHenze/Fugu14
```

### 永不过期版本

仅支持iOS 14.0-15.4.1系统。

使用Unc0ver越狱后，才能安装永不过期版本。添加以下源后搜索unc0ver安装，注意需要选择正确的版本，可选择`6.0.2`、`6.1.1`、`6.1.2`、`6.2.0`、`8.0.2`。

```
https://cydia.ichitaso.com/
```

也可下载以下deb后，通过Filza安装。

```
https://apporanges.lanzoub.com/b0cfc799a
https://www.aliyundrive.com/s/hzmbkEx8YaF
https://pan.baidu.com/s/1AT6vYKDyPmawrW_6Pp3gQA?pwd=ybhk（提取码 / ybhk）
```

## Sileo商店

添加以下源并安装其中的Sileo Prep（checkra1n）。完成后打开Sileo刷新，找到Sileo源，找到Cydia Installer并安装，即可使Sileo和Cydia共存。

```
https://repo.getsileo.app
```

## 清除越狱

打开Unc0ver，在设置中打开Restore RootFS，回到主页点击按钮即可。

## 常见问题

### 进度条卡2或9

换其他签名版本的unc0ver尝试。

### 进度条卡25

unc0ver签名有问题，需重新进行安装unc0ver。

### 进度条卡31

unc0ver的广告，直接往下拉即可，不要点进去广告详情。

# Odyssey

适用于iOS 13.0-13.5，A9-A13设备。下载链接如下。

```
https://theodyssey.dev/
```

## 常见问题

### 第二次及以后出现无限黑屏/菊花并重启

部分插件的冲突问题，导致引导RSD的时候出现卡滞。

重启设备，在Odyssey关闭Enable Tweaks后进行激活越狱。

越狱后打开Sileo，添加嘻哈源，搜索Odyssey Activation并安装即可，该方法不会有权限错误。也可打开NewTerm并执行以下命令，但有可能会出现权限错误。

```
rm /.disable_tweakinject && killall backboardd
```

### 提示ERR_JAILBREAK

打开Sileo并安装Odyssey Activation即可。或打开Fliza，找到根目录，删除.disable_tweakinject，然后点击/usr/bin/killall，在bash后面输入killall backboard，回车后自动注销。

### 卡在三分之一处

飞行模式不能下载数据，漏洞出现利用循环BUG。安装Odyssey以后，要重启设备后十五秒再点JailBreak。

### 卡在三分之三处

漏洞出现利用循环BUG，或执行刷新图标缓存即UICache时卡住。重启设备后十五秒再点JailBreak即可。

# Taurine

## 普通安装

下载链接如下。

```
https://taurine.app/
```

## 永不过期版本

需要用Taurine越狱后，才可以安装永不过期版本。添加以下源后，搜索taurine-permanent安装即可。

```
https://repo.theodyssey.dev/
```

也可下载以下deb包，然后通过Filza安装。

```
https://apporanges.lanzoub.com/b0cfc798j
https://www.aliyundrive.com/s/oG2XUjBbEFA
https://pan.baidu.com/s/1N9xFwIPtkVqx7G9AVGdHtQ?pwd=yzke（提取码 / yzke）
```

# Checkra1n

## 应用安装

官网如下。

```
https://checkra.in/
```

下载后连接手机，打开电脑上的应用，Start即可。如果提示iOS版本过高，则先让手机进入恢复模式，然后再越狱，也可点击Options，勾选Allow untested iOS/iPadOS/tvOS versions。

## 内核配置

越狱后从以下内核选择一种进行安装。

### OdysseyRa1n

#### 安装

手机端安装好Checkra1n后不要打开。确保电脑已安装usbmuxd，若未安装可在终端输入以下命令。

```
// MacOS
brew install usbmuxd

// Linux
sudo apt install libusbmuxd-tools
```

将手机连接到电脑后，在终端输入以下命令以执行安装脚本。

```
curl http://chimera1n.aaronc.cn/CoolStar/chimera1n-deploy-linux-macos.sh | bash

// 或
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/coolstar/Odyssey-bootstrap/master/procursus-deploy-linux-macos.sh)"
```

更新完APT和Sileo以后，安装libhooker。打开CheckRa1n，点击Start重新引导。

#### Cydia商店

在Sileo搜索Cydia Installer即可安装Cydia。若无此安装包，则添加以下源。

```
https://apt.bingner.com/
```

### Checkra1n

打开手机上的checkra1n软件，安装Cydia即可。

### Chimera1n

#### 安装

用爱思助手打开SSH通道，记住端口号。确保电脑已安装usbmuxd，安装方法同上。

手机端安装好Checkra1n后不要打开。电脑端下载以下安装包，解压后编辑chimera1n.sh，将倒数两行的-p4444改为刚才记住的端口号，然后双击chimera1n.command安装，有错误提示可忽略。

```
https://geekben.org/38.html
```

Sileo安装好后更新所有依赖，然后安装libhooker bootstrapper(checkm8)插件。插件安装好后打开checkra1n，点击Start重新引导。

完成后安装以下插件。安装前需在软件包界面点击右上角日期后面的小横线，开启开发者模式。

```
Applist
Preferenceloader
rocketbootstrap
substitute
substrate compatibility layer
tweak injector
conditionalwifi5
```

至此安装完成。注意，使用过程中不要更新除Sileo外的应用商店的插件依赖，否则将无法进入系统。

#### Cydia商店

安装Cydia会导致系统不稳定。

Cydia安装包如下，注意不要装第四个deb。完成后安装`连个锤子`插件以修复权限。

```
https://www.lanzous.com/i6nfksh
```

## 清除越狱

在手机上打开Checkra1n，点击Restore System即可。

## 常见问题

### -20错误

尝试勾选Options里面的Safe Mode，然后重新越狱。

若无效，则打开手机并按照正常流程用checkra1n进入DFU模式。软件显示进入DFU模式成功后，iPhone7/8/X按住音量上键和音量下键，其余老机型按住音量上键和Home键，持续按一段时间，手机进入诊断模式。

打开以下链接，下载minaUSB并安装。

```
https://appletech752.com/Downloads/minaUSB.pkg
```

打开终端输入以下命令，以使minaUSB能够打开。

```
xcode-select --install
codesign -f -s - --deep /Applications/
```

打开minaUSB并点击Patch USB Restrict，提示完成后重新用checkra1n越狱即可。

# Palera1n

可认为是Checkra1n的15.0及更新版本。使用Rootful类型即可。

```
https://palera.in/

# WinRa1n，Windows版
https://onejailbreak.com/blog/winra1n-jailbreak/
```

# Doubleh3lix

```
https://doubleh3lix.tihmstar.net/
```

若越狱后重启设备导致越狱环境失效，可使用以下网站激活。

```
https://totally.not.spyware.lol/
```

# Meridian

```
https://meridian.sparkes.zone/
```

若越狱后重启设备导致越狱环境失效，可使用以下网站激活。

```
https://totally.not.spyware.lol/
```

# Phoenix

```
https://phoenixpwn.com/
```

# kok3shi9

```
https://kok3shidoll.web.app/kok3shi9.html
```

# wtfis

```
https://github.com/therealclarity/wtfis
```

# 其它工具

```
# ap0110
# iOS 10，重启后可自动越狱
https://github.com/athenusdev/ap0110

# ayakurume
# iOS 15，A8-A11设备
https://github.com/dora2-iOS/ayakurume

# Betelguese
# 利用checkra1n越狱得到SSH，然后安装Sileo越狱商店
https://github.com/23Aaron/Betelguese

# ra1ncloud
# iOS 15
https://github.com/iarchiveml/ra1ncloud

# Th0r
https://github.com/pwned4ever/Th0r_Freya
https://github.com/pwned4ever/Th-r

# Blizzard
https://geosn0w.github.io/
https://github.com/GeoSn0w/Blizzard-Jailbreak-9
https://geosn0w.github.io/getblizzard/
https://github.com/GeoSn0w/Blizzard-Jailbreak

# ra1npoc
# 用iPhone/iPad进行checkra1n越狱
https://github.com/kok3shidoll/ra1npoc

# openra1n
# palera1n的Windows版本
https://github.com/wh1te4ever/openra1n

# Freya
# 支持A11及以下设备，支持iOS15.0-15.1系统
https://github.com/pwned4ever/Freya15

# kfund
# 支持iPhone 6s，iOS 15.1
https://github.com/wh1te4ever/kfund

# Houdini
# 支持iOS 10-10.3.2，11-11.1.2
# 非完整越狱
https://github.com/ihorlaitan/houdini-public
```

# 常见问题

## Chimera1n玩游戏闪退

删除OpenSSH即可。

## Taurine安装dash命令组后插件安装正常但卸载出错

主要表现为Sileo提示需要更新bash并强制安装dash。dash命令组不具备支持mv、cp等命令，因此需要切换回bash命令组。

打开Filza，进入`/bin`目录并点击sh文件右侧的叹号，若路径为/usr/bin/dash，则打开NewTerm并输入以下命令。

```
su
# 获取超级权限，密码为alpine
rm -f /bin/sh && ln -s /usr/bin/bash /bin/sh
ldrestart
```


# 参考教程

## Odyssey越狱工具常见问题/错误红字解决方案

```
https://mp.weixin.qq.com/s/mEvS3XOk323iimyRC-DDKQ
https://mp.weixin.qq.com/s/p-hJIVyrYLtX3Qzr1BP4cg
https://mp.weixin.qq.com/s/FqMaobhAI4iqaQ7bXj387Q
https://mp.weixin.qq.com/s/ODNZikQQgVnsks1YGBvWFQ
https://mp.weixin.qq.com/s/gVplvVFgLCCB0hZKCZJcSQ
```

## Taurine/Odyssey越狱工具常见问题/错误红字解决方案(六)

```
https://mp.weixin.qq.com/s/1iz5ZVHkj3-6yyJUxVP45Q
```

## Taurine，永不过期，免费安装，再也不用担心签名问题了，速度

```
https://mp.weixin.qq.com/s/OL2aJMRBApre3jkvEhKcQA
```

## unc0ver，永不过期，免费安装，再也不用担心签名问题了，速度

```
https://mp.weixin.qq.com/s/4UnkQ68bLQJFnVSJbXyHBg
```

## u0Launcher，永不过期版 unc0ver ，速度使用

```
https://mp.weixin.qq.com/s/ZsHpKOf_kTkImfJqQpv6Ow
```

## Fugu 15 Max (暂命名)越狱工具的正确引导方向以及常见问题解决方案(一)

```
https://mp.weixin.qq.com/s/4KHT_OJK8hcysjkb6y14CQ
```

## iOS 15.1.1 越狱，XinaA15 使用指南

```
https://mp.weixin.qq.com/s/bdOG9eX3IBiEL8FSDG-5OQ
```

## IOS9.1~9.3.5系统越狱方法

```
https://mp.weixin.qq.com/s/lWlrIbVWWNmPJLKURpDezA
```

## IOS10~10.3.3越狱方法

```
https://mp.weixin.qq.com/s/oqkAHqeMB-nTnaXg63-RrA
```

## 好消息！IOS10~10.3.3系统完美越狱

```
https://mp.weixin.qq.com/s/LFqg5FC7Ai0DVny__atxFw
```

## iOS 14.4.1 Betelguese 越狱发布，新系统出现

```
https://mp.weixin.qq.com/s/gQ2ZtdnWTIDiMS1S-95-WA
```

## iOS 14 ReProvision 续签进展，新越狱视频演示

```
https://mp.weixin.qq.com/s/TDOpIqIdmmcOyIwwa3zB_A
```

## iOS 14.3 越狱工具已出？降级工具再次更新

```
https://mp.weixin.qq.com/s/OT5Cuu-0Cl2wvkre5kqcag
```

## iOS 13.5 Blizzard 新越狱，操作官网已经出现

```
https://mp.weixin.qq.com/s/BNBOIfaUx8Fxuu7dzy9TnA
```

## 新发现！iOS16.5.1 越狱方法，支持Windows系统

```
https://mp.weixin.qq.com/s/adD12hcazfibHN4GZFTcZw
```

## iOS 8.4.1 终于可以越狱，长达8年时间

```
https://mp.weixin.qq.com/s/hcVjM3nTP0liIusBIVisiw
```

## iOS 15.1 XinaA15 越狱工具，更换字体仅需几秒

```
https://mp.weixin.qq.com/s/gL8vNR6RDb8F4LoUY-ttng
```

## 惊现！iOS 15.7.1 有根越狱，但会失去5G存储空间

```
https://mp.weixin.qq.com/s/SR4JnHA-W62ugkSA9EYy2Q
```

## iOS 14.5.1 越狱问题，百度网盘要提速？

```
https://mp.weixin.qq.com/s/cIDqspQ2TID9lY3APAx8_A
```

## 劲爆！iOS 15.4.1 ra1ncloud 已发布，确实能越狱

```
https://mp.weixin.qq.com/s/hWSzXxgj-HURTQ5jZM40fg
```

## 有点牛！iPhone为iPhone引导越狱，不依赖电脑

```
https://mp.weixin.qq.com/s/K_17t1HFCe7gB07z-gbaAQ
```

## How to Fix Checkra1n Error -20 (unlock iPhone to use accessories)

```
https://www.youtube.com/watch?v=VHWFVwztNwU
```

## 新的 iOS 16.5 越狱发布，支持这款机型

```
https://mp.weixin.qq.com/s/4yjcOhCeI-tNpSR7TEDSZQ
```

## iOS 15.0 - 15.7.6 越狱已发布，不用电脑

```
https://mp.weixin.qq.com/s/0MkrVtac2s2Yi2I2sOx3nQ
```

## 收藏啦！iOS15.1XinaA15越狱，PC访问系统文件

```
https://mp.weixin.qq.com/s/FsM3Ul-3KaDunLQF__xQ8Q
```

## 新工具疑似完美越狱 / 阿里云盘扩容 / 扫黑风暴惨遭泄漏

```
https://mp.weixin.qq.com/s/GwVnPIfZoeZVIRS0IKkufg
```

## iOS14.8.1系统越狱新解锁，戴耳机也能解锁设备

```
https://mp.weixin.qq.com/s/rTLe1KdfRQiZ54SaH4G8Hw
```

## iOS 系统，Dopamine 越狱，官方源作者源推荐（一）

```
https://mp.weixin.qq.com/s/9IVsWkRaCcfrnSr4Ebupxw
```

## 二个越狱消息：新版Cydia将来出炉/IOS11.3.1平刷插件

```
https://mp.weixin.qq.com/s/2hmmLyQaSpupc9D9PYsmRQ
```

## iOS12.4 越狱工具无法安装？绕过 ID 新方法

```
https://mp.weixin.qq.com/s/3AqUY1R0hVxWYxScBlfNwA
```

## iOS 15.4.1 越狱，Dopamine 将支持简体中文

```
https://mp.weixin.qq.com/s/8rVGhvCy3jwpN_c3Hiwksg
```

## 如何在不同基板(框架)/APT(规范)下施展Cydia和Sileo的轮回骚操作

```
https://mp.weixin.qq.com/s/9oMQVCdudK_jYvTVw5zQTw
```

## Cydia for iOS7-iOS13 内各种红字、黄字常见错误解决方法全收录

```
https://mrmad.com.tw/cydia-red-yellow-error
```

## coolstar/Odyssey-bootstrap

```
https://github.com/coolstar/Odyssey-bootstrap
```

## XinaA15 新官网上线，请收藏

```
https://mp.weixin.qq.com/s/0MRRIOghnihTA-TSmv7s5g
```

## iOS 17 系统，也能越狱？

```
https://mp.weixin.qq.com/s/B2EE4Tq6lFYmQH1koJucCQ
```

## 惊现！iOS 完美越狱已发布，仅支持这些系统

```
https://mp.weixin.qq.com/s/InUtCLs6zDVy3XJs2Vwp7A
```

## iOS 14.6 - 14.8 系统越狱来了，你需要知道这些

```
https://mp.weixin.qq.com/s/zf3cJqinVIE4G8w7lJQLXQ
```

## unc0ver 更新 7.0.1 版本，真的是完美越狱吗？

```
https://mp.weixin.qq.com/s/dGvP2l4JwwkAMj-kevzoNw
```
