---
title: iOS越狱指南
categories: iOS
abbrlink: iOS-Jailbreak-Guide
date: 2023-11-10 11:33:29
tags:

---

![](topic.jpg)

iOS越狱指南。

<!-- more -->

# TODOS

```
https://ios.cfw.guide/using-palen1x/


https://www.reddit.com/r/jailbreak/comments/1tmvh7/question_what_exactly_is_bootstrap/
https://onejailbreak.com/blog/iapstore-tweak/
https://leavez.xyz/2017/02/12/mario-tweak/
https://www.bilibili.com/read/cv12153485/
```

# 方法

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

## Dopamine

### 应用安装

越狱后需要在Sileo安装Ellekit deb、PreferenceLoader-Rootless、AltList、libSandy、CCSupport、uikittools。

```
https://ellekit.space/dopamine/
https://github.com/opa334/Dopamine
```

### 清除越狱

打开Filza，进入Private/Preboot，打开以jb为名的文件夹，确认内部有一个名为Proucursus的文件夹，且该文件夹内部有fugu15max的相关文件。回到上一层并删除jb为名的文件夹。返回根目录，删除var/jb。

完成后打开TrollStore，进入Settings，点击Rebuild Icon Cache即可。

### 常见问题

```
https://mp.weixin.qq.com/s/5yzh14U7xC3Y0yVtVwdbIg
```

#### 越狱后打开APP闪退

需要重启SpringBoard。打开TrollStore，进入Settings，点击Respring即可。

## XinaA15

### 应用安装

```
https://zhuxinlang.github.io/
https://xina.ss03.cn/
https://apt.xina.vip/
https://github.com/NotDarkn/XinaA15
```

### 应用商店

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

### 插件

#### Xina清除35条更新

安装XinaA15后Sileo会出现35个更新。可通过安装该插件删除Procurs自带源以消除提示。

```
https://apt.ss03.cn/
```

#### MilkyWay2

由于未添加iOS 15支持，需要安装MilkyWay2和MilkyWay2-iOS14Fix。或者安装lenglengyu源的版本即可。

```
http://www.lenglengyu.com/
```

#### 更换字体

添加以下源，搜索Xina即可。

```
https://apt.ss03.cn/
```

### 使用

#### 手动安装Deb插件包

打开XinaA15内置文件管理器，长按点击Deb安装即可。

### 常见问题

#### 电脑端无法访问手机文件

安装Apple File Conduit"2"插件后爱思助手可能仍然无法访问手机文件。可以打开爱思助手，选择工具箱-SSH通道-SCP客户端，即可访问。

若需要在电脑端给手机安装插件，则将插件通过SCP客户端复制到手机端后，点击SCP客户端左上角的终端，输入以下命令即可。

```
dpkg -i [deb插件名称]
uicache —all
killall SpringBoard
```

#### 安装应用级插件出现白图标

安装XinalConFix插件，然后重新安装XinaA15工具，激活越狱。

```
https://opa334.github.io/
```

#### Sileo刷新源出现522错误

在Sileo越狱商店精选页面上，点击右上角头像，下拉找到缓存，直接点击两下，然后点击语音，开启使用系统语言，再点应用即可。

## SaiGon

```
https://github.com/H0aHuynh/SaiGon15x/releases
```

## Def1nit3lyN0tAJa1lbr3akTool

```
https://github.com/KpwnZ/Def1nit3lyN0tAJa1lbr3akTool
```

## NekoJB

```
https://nekojb.hhls.xyz/
```

## Unc0ver

Unc0ver版本与机型和系统的对应表格如下。

|       系统      |  支持机型 | 可用unc0ver版本 |
|-----------------|-----------|-----------------|
| iOS 13.0-13.7   | A9-A13    | 5.0.0-6.0.0     |
| iOS 14.0-14.3   | A9-A14    | 6.0.0-6.2.0     |
| iOS 14.4-14.5.1 | A12及以上 | 7.0.0-7.0.2     |
| iOS 14.6-14.8   | A12-A13   | 8.0.0-8.0.2     |


### 应用安装

#### 普通安装

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

#### 永不过期版本

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

### Sileo商店

添加以下源并安装其中的Sileo Prep（checkra1n）。完成后打开Sileo刷新，找到Sileo源，找到Cydia Installer并安装，即可使Sileo和Cydia共存。

```
https://repo.getsileo.app
```

### 清除越狱

打开Unc0ver，在设置中打开Restore RootFS，回到主页点击按钮即可。

### 常见问题

#### 进度条卡2或9

换其他签名版本的unc0ver尝试。

#### 进度条卡25

unc0ver签名有问题，需重新进行安装unc0ver。

#### 进度条卡31

unc0ver的广告，直接往下拉即可，不要点进去广告详情。

## Odyssey

适用于iOS 13.0-13.5，A9-A13设备。下载链接如下。

```
https://theodyssey.dev/
```

### 常见问题

#### 第二次及以后出现无限黑屏/菊花并重启

部分插件的冲突问题，导致引导RSD的时候出现卡滞。

重启设备，在Odyssey关闭Enable Tweaks后进行激活越狱。

越狱后打开Sileo，添加嘻哈源，搜索Odyssey Activation并安装即可，该方法不会有权限错误。也可打开NewTerm并执行以下命令，但有可能会出现权限错误。

```
rm /.disable_tweakinject && killall backboardd
```

#### 提示ERR_JAILBREAK

打开Sileo并安装Odyssey Activation即可。或打开Fliza，找到根目录，删除.disable_tweakinject，然后点击/usr/bin/killall，在bash后面输入killall backboard，回车后自动注销。

#### 卡在三分之一处

飞行模式不能下载数据，漏洞出现利用循环BUG。安装Odyssey以后，要重启设备后十五秒再点JailBreak。

#### 卡在三分之三处

漏洞出现利用循环BUG，或执行刷新图标缓存即UICache时卡住。重启设备后十五秒再点JailBreak即可。

## Taurine

### 普通安装

下载链接如下。

```
https://taurine.app/
```

### 永不过期版本

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

## Checkra1n

### 应用安装

官网如下。

```
https://checkra.in/
```

下载后连接手机，打开电脑上的应用，Start即可。如果提示iOS版本过高，则先让手机进入恢复模式，然后再越狱，也可点击Options，勾选Allow untested iOS/iPadOS/tvOS versions。

### 内核配置

越狱后从以下内核选择一种进行安装。

#### OdysseyRa1n

##### 安装

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

##### Cydia商店

在Sileo搜索Cydia Installer即可安装Cydia。若无此安装包，则添加以下源。

```
https://apt.bingner.com/
```

#### Checkra1n

打开手机上的checkra1n软件，安装Cydia即可。

#### Chimera1n

##### 安装

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

##### Cydia商店

安装Cydia会导致系统不稳定。

Cydia安装包如下，注意不要装第四个deb。完成后安装`连个锤子`插件以修复权限。

```
https://www.lanzous.com/i6nfksh
```

### 清除越狱

在手机上打开Checkra1n，点击Restore System即可。

### 常见问题

#### -20错误

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

## Palera1n

可认为是Checkra1n的15.0及更新版本。使用Rootful类型即可。

```
https://palera.in/

# WinRa1n，Windows版
https://onejailbreak.com/blog/winra1n-jailbreak/
```

## Doubleh3lix

```
https://doubleh3lix.tihmstar.net/
```

若越狱后重启设备导致越狱环境失效，可使用以下网站激活。

```
https://totally.not.spyware.lol/
```

## Meridian

```
https://meridian.sparkes.zone/
```

若越狱后重启设备导致越狱环境失效，可使用以下网站激活。

```
https://totally.not.spyware.lol/
```

## Phoenix

```
https://phoenixpwn.com/
```

## kok3shi9

```
https://kok3shidoll.web.app/kok3shi9.html
```

## wtfis

```
https://github.com/therealclarity/wtfis
```

## 其它工具

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
```

# 插件

## 无根越狱插件

部分可用插件如下。

```
# Derootifier，转无根插件
https://github.com/haxi0/Derootifier

# SixteenMusic，iOS 16系统风格的锁屏音乐播放器
https://www.123pan.com/s/vgn0Vv-DkfFv.html

# Witcher，应用切换器
https://github.com/MatoiDev/Witcher

# OCD，角标个性化
https://www.123pan.com/s/vgn0Vv-zj4Fv.html

# ClassicFolders，iOS 6风格的经典文件夹插件
# ClassicFolders 3原版，支持iOS 13-14
https://github.com/coolstar/classicfolders3-oss/
# ClassicFolders 3编译版，支持iOS 15
https://github.com/Lizynz/classicfolders3-oss/
# ClassicFolders 2，支持 iOS 7-12
https://github.com/coolstar/classicfolders2

# No2Theft，保护手机
https://www.123pan.com/s/vgn0Vv-TF4Fv.html

# Bubble Apps，将常用应用像悬浮的泡泡一样显示
https://www.123pan.com/s/vgn0Vv-13fFv.html

# KuKu，横幅通知个性化类型
https://www.123pan.com/s/vgn0Vv-QpxFv.html

# Ventana，Windows 10风格的锁屏美化插件，支持iOS 8-14
https://github.com/coolstar/ventana

# ClassicLockScreen，经典锁屏插件，支持iOS 7-9
https://github.com/coolstar/classiclockscreen

# 3DSwitcher 2，3D 风格的应用切换器插件，支持iOS 9-10
https://github.com/coolstar/3dswitcher2

# Anemone，主题插件，支持iOS 11-13
https://github.com/coolstar/anemone3
https://github.com/coolstar/Anemone-app
https://github.com/coolstar/anemone3extensions

# Sleipnizer，Safari浏览器手势
https://www.123pan.com/s/vgn0Vv-HmmFv.html

# SafariX，Safari浏览器增强
https://www.123pan.com/s/vgn0Vv-scoFv.html

# TwistNTurn，智能旋转
https://www.123pan.com/s/vgn0Vv-3AmFv.html

# Ring，控制中心音量扩展
https://www.123pan.com/s/vgn0Vv-4mmFv.html

# SmartNotifications，智能通知
https://www.123pan.com/s/vgn0Vv-somFv.html

# Lynx 2，终极定制插件
https://www.123pan.com/s/vgn0Vv-XdoFv.html

# SimpleTime，修改锁屏时间和日期
https://www.123pan.com/s/vgn0Vv-CQfFv.html

# Vē，通知记录器
https://github.com/AlexandraAurora/Ve

# Pinnacle，App堆栈式插件
https://www.123pan.com/s/vgn0Vv-6L1Fv.html

# BigShot，一键长截图
https://github.com/jontelang/BigShotJbSnapper3Plugin/tree/main/packages

# Jade，控制中心美化
https://www.123pan.com/s/vgn0Vv-0K9Fv.html

# IPA Ranger，IPA下载器
https://github.com/0xkuj/IPARanger

# SmartNetwork，智能网络
https://www.123pan.com/s/vgn0Vv-03mFv.html

# Sleepizy 2，睡前音乐管理器
https://www.123pan.com/s/vgn0Vv-XGoFv.html

# Mineland，灵动岛下移
https://github.com/34306/mineland

# upsidedowned，颠倒显示
https://github.com/34306/upsidedowned

# Apple File Conduit "2"，允许计算机应用程序，通过 USB 访问设备完整的文件系统
https://github.com/Cannathea/afc2d-arm64

# WTVRBGLauncher，输入法语音转文字免跳转
https://github.com/Lessica/WTVRBGLauncher

# RemoveWidgetBackground，移除iOS系统小组件背景
https://github.com/Lessica/RemoveWidgetBackground

# Griddy，图标随意摆
https://www.123684.com/s/vgn0Vv-ZkMFv

# CC18，将控制中心模块变成圆形
https://github.com/dayanch96/CC18

# 禁字诀，禁止指定App摇一摇跳转
https://www.123865.com/s/vgn0Vv-uXBFv

# Colorful Wallpaper X，动态光影壁纸
https://www.123pan.com/s/vgn0Vv-PgaFv

# NotificationsGroupCount，计算锁屏分组通知数量
https://www.123684.com/s/vgn0Vv-LaMFv
```

兼容无根越狱的源如下。

```
# Karen源
# AppSync Unified，随意安装IPA
https://cydia.akemi.ai/

# ElleKit基板、PreferenceLoader设置显示依赖(rootless)、Ampere电池(rootless)
https://repo.acreson.cn

# Shadow屏蔽(rootless)
https://ios.jjolano.me

# Netskao源
# PullOver Pro，侧边分屏
# CCPower，控制中心
# QQ净化
# 微信净化
# PullOver Pro，侧边分屏
# DumpDecrypter，砸壳
# ShowTouch，显示触摸轨迹
# QQNotiAvatar，QQ通知显示联系人头像
# WechatNotiAvatar，微信通知显示联系人头像
# CCBadgeClear，控制中心一键清除角标
# CCLiftToWake，控制中心添加抬起唤醒按钮
# SmallBanners，迷你横幅
https://repo.initnil.com
https://jailrepo.initnil.com/

# 密友助手(rootless)、斗图助手(rootless)
https://apt.25mao.com

# Sileo Night作者源
https://repo.anamy.gay

# 电话助手作者源
# 电话助手
# 魔术师，一键锁屏
# 灵动通知，使用灵动岛展示系统通知
http://apt.htv123.com/

# opa334作者源
# Choicy，管理插件注入
https://opa334.github.io/

# Chariz源
# LastLook，多功能息屏显示
# Cylinder Reborn，给桌面翻页加上特效
# Atria，自定义主屏幕图标布局
# LastLook，熄屏显示
# betterAlarm，锁屏计时器和闹钟显示插件
# AlbumManager，照片相簿加锁
# Bolders Reborn，自定义文件夹
# SearchDots，模仿iOS 16系统桌面搜索按钮
# SwipeExtenderX，原生键盘增强
# Akara，控制中心自定义
# Glance，熄屏显示
# Oneko，添加动态小猫
# IAmLazy，插件备份
# Zetsu，系统分屏
# Mitsuha，音频可视化
# QuickActions，自定义锁屏快捷
# TweakReviewsDB，插件评分评论
# PopOutButtons，按下按键弹出按钮指示器
https://repo.chariz.com/

# Ginsu作者源
# Dodo，锁屏美化插件
https://repo.ginsu.dev/

# PoomSmart源
# RecordPause，录像暂停
# TapVideoConfig，视频录制弹窗选择分辨率
# CC On&Off，在控制中心真正关闭WiFi/蓝牙
# BlurryBadges，角标美化
# AppColorClose，删除按钮自适应App图标颜色
# Battery Health Enabler，开启电池健康
# Live Text Enabler，在不支持的设备上开启实况文本功能
https://poomsmart.github.io/repo/

# Spark源
# SnowBoard，主题插件
# CCMusicArtwork，控制中心音乐模块显示正在播放音乐封面
# AutoUnlockX，自动解锁
https://sparkdev.me/

# Sileo 官方源
https://repo.anamy.gay/

# TIGI Software 源
# Filza、Apps Manager
https://tigisoftware.com/cydia/

# P2KDev源
# SimpleBattery，状态栏电池增加百分比显示
# SugarCane，控制中心亮度和音量添加百分比显示
# ReachMore，便捷访问手势
# 3DBadgeClear，快速清除通知角标
# DeleteForeverXS，永久删除照片
# EZSwipe，主屏幕向下滑动手势插件
# TinyWidget14，锁屏音乐播放器
# ScreenshotActions，给截图添加类似于iOS 16系统的拷贝并删除选项
# LetMeKnow，电话接听振动
# NewTab，给Safari底部添加新建页面+号按钮
# ReachMore，便捷访问手势
# QuitAll，一键清理后台
# Sakal，锁屏添加天气等信息
# LetMeDecline，锁屏来电增加拒接按钮
# VideosUnmute，照片中播放视频时取消静音
# CCBorder，控制中心模块自定义
# iOS 15.0-15.4.1无根越狱
https://p2kdev.github.io/repo/
# iOS 14
https://repo.packix.com/

# Havoc源
# Snapper 2，区域截图插件
# CCCounters，控制中心显示倒计时
# Macaron，给Dock栏添加图片
# UnderDock，键盘底部添加快捷按钮
# Lynx 2
# Picarize，一键抠图
# UnderDock，键盘增强
# Stella，模拟雪花飘落
# Anouk，给隐藏照片加锁
# FloatingDockXVI，自定义Dock栏
# Translomatic，快捷翻译
# CopyLog，剪贴板插件
# Ampere，给全面屏设备电池加上百分比
# Hammer It，分词大爆炸
# Snapper 3，区域截图
# CCDNDTimer，控制中心勿扰模式增强
# MenuSupport，将选择菜单图标按钮显示
# DLEasy，国外社交App视频下载神器
# Alpine，让iPhone使用tvOS精美弹窗
# ENA，从照片中提取对象
# Cardculator，卡片样式的计算器插件
# InstaLauncher 2，应用快速启动
# Uptime，控制中心显示开机后运行时间
# DNDMyRecording，录屏勿扰
# SquidGesture，全局手势
# Dynamic Stage，分屏
# Souvenir，锁屏自定义
# OneSettings，设置App自定义
# Minotaur，状态栏中查看通知
# cr4shed，崩溃报告记录
# Flow，锁屏音乐美化
# Zenith，App堆栈式插件
# SmallSiri，缩小Siri
# Centaur，重新设计通知中心
# PowerModule，控制中心模块插件
# Speedster，自定义动画速度
# ShowMyTouches，触摸轨迹
# Touch-Viz，触摸显示
# Verxina，Dock 栏扩展
# CopyVault，剪贴板
# CloseAll，一键清除后台
# QuitAll，一键清除后台
# Mochi15，锁屏音乐
# Lock Master，自定义系统锁定动画和声音
# CircleApps15，添加App快捷启动
# Prysm，全新的控制中心
# Quart，通知和媒体播放器
# Watermelon，WatchOS风格的精美锁屏小部件
# Emerald，状态栏显示，药丸风格
# Siliqua 2，AirPod手势控制
# Siliqua Pro，AirPod手势控制
# Noctis12，适合iOS 12系统的深色模式
# Noctis Neo，外观模式伴侣
# SmartRotate，智能方向锁
# Maple，充电动画
# NoctisXI，适合iOS 11系统的全局深色模式
# AirpodsCompanion，音量滑块增加AirPods控件
# Tinge，角标个性化颜色
# BadgeSync，删除锁屏通知同步删除对应App角标
# Remove Widget Background，透明小组件
# Icon Restore，备份或恢复主屏幕图标布局
# Single Mute，当开启静音时，时间旁边显示静音小图标
# Single VPN，开启VPN时，状态栏WiFi或数据信号以指定颜色显示
# Immortalizer，让应用无限制地在前台运行
https://havoc.app/

# Limneos源
# AudioRecorder XS，通话录音插件
# DynamicPeninsula，灵动半岛插件
# AquaBoard XS，添加水波纹特效
# AnsweringMachine XS，来电自动应答
# VoiceChanger XS，改变通话声音
# Glow，让屏幕发光
# BioProtect XS，程序保护插件
# LiveCallTranslator，通话翻译
https://limneos.net/repo/

# alias20源
# AdaptiveAirPodView，模仿iOS 17系统AirPods系列设备弹窗自适应深色模式
# bfdecryptor，砸壳插件
# CalcLog，计算器显示历史记录
# Speedy，自定义动画速度
# A-Shields，程序锁
# Axon，锁屏通知归纳
# Power4Options，电源增强插件
# FakeSignalBar，自定义运营商
# BlurryAlerts，使用tvOS风格的弹窗
# CoolCC Reborn，控制中心移除模块背景和添加边框
# Axon，锁屏通知归纳
https://alias20.gitlab.io/apt/

# level3tjg
# Installed，让Cydia源显示已安装和总插件数量
# BetterCCBrightness，控制中心亮度添加更多扩展
https://level3tjg.xyz/repo/

# iCraze源
# DarkPods，模仿iOS 17系统AirPods系列设备弹窗自适应深色模式
# NotiCopy15，复制通知
# HideQuickActions，去除锁屏快捷
https://repo.icrazeios.com/

# Creature源
# TweakSettings，独立的插件设置
# Shuffle，设置归类
# Tranquil，背景音
https://creaturecoding.com/repo/

# AnthoPak源
# AddToFolder，桌面App快速移动到指定文件夹
# VideoSwipes，给默认视频播放器添加手势
# PanCake，快速返回上一页
https://repo.anthopak.dev/

# Dcsyhi源
# Zetsu，分屏插件
# KBApp，在键盘底部添加快捷按钮或者App
# ADKeyboard，键盘添加颜色
https://dcsyhi1998.github.io/

# Sopppra源
# SettingsRevamp，设置自定义插件
# Cache Magic Rootless，控制中心清除系统缓存
# Cpu Magic Rootless，控制中心显示CPU实时信息
# Memory Magic Rootless，控制中心显示RAM实时信息
# Network Magic Rootless，控制中心显示网络信息
# StatusTime，自定义状态栏日期和时间
# TweakHub，快速呼出插件设置
# KillApps，多功能一键清理后台
https://sopppra.mooo.com/

# Nebbs源
# Edge 2，给屏幕边缘添加渐变色彩
https://repo.itznebbs.com/

# Lizynz源
# IconOrder，自定义文件夹
# FolderX，自定义文件夹插件
https://lizynz.github.io/

# SkyPian源
# CCLocation，控制中心添加定位开关
# CCRespring，控制中心一键注销
# CCSettrings，控制中心添加设置开关
# MiniLsp，锁屏迷你播放器
# Kayoko，剪贴板记录和管理插件
# CCScreenShot，控制中心截图
# CCAutoBrightness，控制中心亮度自动调节
https://skypain.github.io/repo/

# akusio源
# BackgrounderAction15 for CCSupport，应用程序最小化后保持后台运行
# MilkyWay4，分屏插件
https://akusio.github.io/

# NoisyFlake开源项目
# Velvet 2，自定义通知
# AlbumManager，整理好保护照片
# betterAlarm，漂亮的计时器和闹钟
https://github.com/NoisyFlake

# b4db1r3源
# OCD，角标和名称自适应图标颜色
# tappy，单击或双击状态栏时间显示自定义日期
# Tako，锁屏通知聚合
# CozyBadges，角标个性化颜色
https://b4db1r3.github.io/d3vr3p0/

# maxiwee源
# noquickies，去除锁屏手电筒和相机快速按钮
# NoDock，Dock栏透明背景
# nobar，去除小横条
# FiveIcon Dock，Dock栏五个图标
https://maxiwee.de/

# JunesiPhone源
# HideDockBG，Dock栏增强
https://junesiphone.com/supersecret/

# Ginsu源
# Dodo，锁屏插件
# DualClock 2，为锁屏显示双时钟
https://ginsu.dev/repo/

# Fouad Raheb源
# AppData，快速查看和管理应用程序详细数据
https://apt.fouadraheb.com/

# uz-ra源
# medusa，在iPhone上启用iPad多任务功能
https://uz-ra.github.io/

# ZX02源
# Frame，视频壁纸
# CCDND，控制中心勿扰模式
# CCDNDTimer，控制中心勿扰模式增强
https://zerui18.github.io/zx02/
https://zx02.yourepo.com/

# ETHN源
# ReachPlayer，便捷访问区域显示音乐播放器
# LoopVideos，照片视频循环播放
https://nahtedetihw.github.io/

# MERONA源
# A-Font，字体管理插件
# A-Shields，程序锁
https://repo.co.kr/

# ichitaso源
# NoUpdateMark，去除设置更新红点
# HideKBSettings，隐藏键盘设置
# PrimalFolder 2，将文件夹第一个图标作为文件夹封面
# HideSerialNumber，自定义关于本机页面选项
# RingerToggle，控制中心添加静音按钮
# LocationService，控制中心定位开关
# AutoBrightnessToggle，控制中心自动亮度调节开关
https://cydia.ichitaso.com/

# BigBoss源
# App Library Disabler，去除App资源库
# CCVibration，控制中心添加振动模块
# WiFiQR，将WiFi网络生成二维码
# HotspotQR，将个人热点网络生成二维码
# CustomLPM，自动开启低电量模式
https://apt.thebigboss.org/repofiles/cydia/

# 刀刀源
# 插件汉化包
# CCLess++，控制中心模块大集合
https://xiangfeidexiaohuo.github.io/

# 老牌猫源
https://apt.25mao.com/

# 赵楠源
# Give me deb，自动保存deb安装包
https://invalidunit.github.io/repo/

# rob311源
https://cydia.rob311.com/repo/

# alexia源
https://repo.alexia.lol/

# MirO92源
http://miro92.com/repo

# iKarwan源
# PMP，照片保护插件
https://repo.ikghd.me/

# Dhinak源
https://dhinakg.github.io/repo/

# DEB 备份，已安装的插件，提取和备份
# 百度网盘（提取码6rsu）
https://pan.baidu.com/s/1AHqn6osPVHSxtSuPQaabSA?pwd=6rsu 
# 123云盘
https://www.123pan.com/s/vgn0Vv-XchFv.html

# CydiaGeek源
# DoubleTapToLock，双击锁屏
https://cydiageek.yourepo.com/

# i0s_tweak3r源
# ByeByeAppLibrary，禁用App资源库
# LSTimeSeconds，锁屏显示秒数
https://www.yourepo.com/

# 0xkuj源
# 3DAppVersionSpoofer，自定义App版本
https://0xkuj.yourepo.com/

# ugly-soul源
# split-view-pro，分屏插件
# dynamic-notifier，通知上岛
# native-island-notifier，通知上岛
https://ugly-soul.github.io/repo/

# Nixuge源
# NetworkManagerReborn，网络管理
https://cydia.nixuge.me/

# 乌贼力量源
# LowPowersleep，锁屏自动低电量
# DockX，在键盘底部添加快捷按钮
# VPNIndicator，适合全面屏使用的VPN指示器
# StatusBarSpeed，状态栏网速
# RecordAnywhere，锁屏录屏
https://lclrc.github.io/repo/

# Brend0n源
# NoPassAfterRespring（Safe），注销后无需锁屏密码，直接进入桌面
# FPSIndicator，游戏帧数显示
# DesktopLyrics，桌面歌词
http://brendonjkding.github.io/

# p0358源
# PrideBars，自定义状态栏WiFi和信号颜色
https://repo.p0358.net/

# Skitty源
# Six（LS），锁屏界面变成iOS 6样式
# QuietDown，App单独静音
# Sushi，歌曲变化时横幅提醒
# Ersatz，全局范围内的替换文本
# MoreComplications，iOS 16锁屏小组件增加行数
# LightsOut，根据环境光传感器来切换系统深色模式
https://skitty.xyz/repo/

# MTAC源
# Hydra，三维触控添加App快捷菜单
# CCPowerMenu，控制中心电源增强
https://mtac.app/repo/
https://repo.mtac.app/

# khanhduytran0源
# TrollPad，后台分屏
https://khanhduytran0.github.io/repo/

# 艾锋源
# April，省电插件
# BypassRestrictionsPwd，抹去访问限制密码
http://apt.ss03.cn

# ethxnn88源
# VisibleIsland，为老设备开启灵动岛
https://ethxnn88.github.io/repo/

# Sugiuta源
# Nanobanners，迷你通知横幅
# FrendaCC，控制中心图标美化
# PlampyCC，控制中心图标美化
# LPMNotification，移除低于20%电量弹窗
https://sugiuta.github.io/

# udevs源
# DockX，在键盘底部增加更多实用按钮
# DebHoarder，自动备份deb安装包
https://udevsharold.github.io/repo

# kingpuffdaddi源
# CCBalance，控制中心添加音频平衡
# CCMono，控制中心添加单声道模块
# AudioMix，同时播放两个或多个音源
# AlwaysDark，自定义App使用浅色或深色模式打开
https://kingpuffdaddi.github.io/

# dixtdf源
# CCSupportShortcut，控制中心添加快速打开 URL 模块
https://dixtdf.github.io/repo/

# 0xkuj源
# StoreSwitcher 2，App Store 快速切换账号
https://0xkuj.github.io/kujman/

# Nathan源
# InternalStorageSettings，查看iPhone储存空间中系统数据
https://nathan4s.lol/repo/

# 34306源
# replaced，删除手机设置关于本机页面里未知部件
https://34306.github.io/

# CreatureSurvive源
# TweakSettings，独立的插件设置
https://creaturecoding.com/repo/

# Install Package Files源
# MobileMeadow Reborn，添加草地效果
https://pkgfiles.github.io/
```

## 非越狱手机查看插件库

可使用PostBox。

```
https://www.postbox.news/
```

## 插件库

### 越狱插件搜索

```
https://www.ios-repo-updates.com/
https://jlippold.github.io/tweakCompatible/
```

### Packix付费插件合集

```
链接 / https://pan.baidu.com/s/1OnBL77Tqn1og7iw3K90w2A
密码 / zeg8
```

## 越狱源

### 自用越狱源

```
// 必加
http://repo.acreson.cn/
https://invalidunit.github.io/netskao-archive/

// 其它
https://repo.packix.com/
https://rpetri.ch/repo/
http://tateu.net/repo/
https://rejail.ru/
http://repo.feng.com/
https://apt.bingner.com/
http://apt.thebigboss.org/repofiles/cydia/
https://repo.hackyouriphone.org/
http://apt.modmyi.com/
https://creaturecoding.com/repo/
https://cydia.ichitaso.com/
http://cydia.zodttd.com/repo/cydia/
https://repo.dynastic.co/
http://repo.qqtlr.com/
https://cydia.akemi.ai/
https://repo.rpgfarm.com/
http://mrmadtw.github.io/repo/
https://repo.initnil.com/
https://apt.initnil.com/
http://repo.pixelomer.com/
https://ibreak.yourepo.com/
https://repo.twickd.com/
https://skitty.xyz/repo/
https://apt.iphoneba.cn/
https://repo.cydiabc.top/
http://apt.coolstar.xyz/
http://ibreak.yourepo.com/
https://repo.icrazeios.com/
https://kanam.me/repo/
https://apt.25mao.com/
http://limneos.net/repo/
https://junesiphone.com/supersecret/
https://evynw.github.io/
https://soda-ldz.yourepo.com/
https://Poomsmart.github.io/repo/
https://sileo.top/
https://cydia.kiiimo.org/

// 以下源不能用Cydown
https://apt.cydiakk.com/
https://apt.abcydia.com/
https://apt.cydia.love/
https://apt.cydiavip.com/
https://apt.wxhbts.com/
https://apt.cydiami.com/
https://apt.Fcydia.com/
https://apt.cydiaba.cn/
```

### 其它越狱源

```
http://zlxdike.github.io/repo/
https://apt.fouadraheb.com/
https://apt.geometricsoftware.se/
https://henrikssonbrothers.com/cydia/repo/
http://apt.iarrays.com/
https://kanam.me/repo/
https://repo.openpack.io/
https://repo.niceios.com/
https://repo.menushka.ca/
https://creaturesurvive.github.io/
https://repo.anthopak.dev/
https://captinc.me/
https://jb365.github.io/
https://repo.1conan.com/
https://tweak.mario.net.in/
https://repo.xyaman.xyz/
https://nni43-repo.herokuapp.com/
https://www.icaughtuapp.com/repo/
https://midkin.eu/repo/
https://repoiz.github.io/repoiz/
https://denialpan.github.io/
https://repo.chr1s.dev/
https://repo.titand3v.com/
https://cydia.alexbeals.com/
https://foxfort.yourepo.com/
https://galacticdev.me/
https://repo.krit.me/
http://beta.unlimapps.com/
http://repo.qwertyuiop1379.com:7777/
https://shepgoba.github.io/
https://repo.tr1fecta.co/
https://repo.basepack.co/
https://repo.festival.tf/
https://myxxdev.github.io/
https://xia0z.github.io/
https://halo-michael.github.io/repo/
https://repo.yfyf.fun/
https://lzsxcl.github.io/repo/
http://apt.clezz.com/
```

## 插件集合

```
PermasigneriOS
iOS 14.0 - 15.4.1生成永久版绕过签名App
https://github.com/powenn/permasigneriOS

CopyLog
剪贴板管理神器

Crane
为每个应用程序创建多个容器，分身神器，一键切换帐户

PullOver Pro
侧边分屏
官方版本，c1d3r（很久没有更新）
https://c1d3r.com/repo/

WeChatCallk
微信添加Callkit功能

ShortLook
优雅通知

AFC2 For IOS11
在电脑助手开通系统文件访问权限，能访问越狱文件，可以进行查看路径，在电脑修改文件等操作

CallBar X
来电通话显示插件

vWallpaper 2
来电可设置视频
来电视频放置路径: /User/Media/vWallpaper2/VideosRingtones/  
动态桌面和锁屏是放置路径： /User/Media/vWallpaper2/Videos/ 
视频格式一定要mp4

AnyWhere
虚拟定位插件
apt.abcydia.com


IntelligentPass 4
带特定蓝牙耳机或连接特定wifi时自动解锁
apt.geometricsoftware.se


PPSSPP，PSP模拟器
cydia.ppsspp.org



软件源
http://apt.abcydia.com（雷锋源）
http://apt.cydiaba.cn（贴吧源）
http://apt.ss03.cn（艾锋源）
https://repo.packix.com（packix源）
https://repo.lonestarx.net
http://repo.auxiliudev.com
http://beta.sparkservers.co.uk
http://repo.auxiliumdev.com
https://repo.xarold.com/
https://cydia.angelxwind.net/
http://limneos.net/repo
http://julio.xarold.com/
http://getdelta.co/
http://beta.sparkservers.co.uk/
https://repo.lonestarx.net/
http://apt.thebigboss.org/repofiles/cydia




软件插件
Cylinder 桌面翻页特效
BetterCCXI 控制中心增强
WiFi Passwd 密码备忘录
FloatingDockXI 悬浮DocK
batteryNotch 电池刘海
App Admin 应用降级
NoMoreSApps x全屏应用
AnyWhere 虚拟定位+修改机型
ToneEnabler 手动拖入铃声
BatteryLife 电池信息
CarBridge CarPlay汽车互联
Xen HTML 锁屏与桌面挂件的搭载平台
Boxy 3 强大的桌面图标布局自定义工具
BeGreen 自定义电池符号
FullSafari 让 iPhone 的 Safari 浏览器实现 iPad 的标签页浏览模式



系统工具
FakeClockUp 系统加速
Fiza File 文件管理器
Netfix Cydia 修复联网问题
NGXPlay11 CarPlay 解锁
Liberty Lite 屏蔽越狱检测
SemiRestore11 /Rollectra IOS11.3~11.4B3平刷系统
BioProtect X 指纹/面部加密
PreferenceOrganizer 设置分类
AppSync Unified 关闭签名
NoBetaNag 屏蔽beta提示
iCleaner Pro 系统清理
Extender X 签名续签
NoMoreSApps x全屏应用
April 极限省电
Icon Renamer 自定义APP名称
DoubleTapLock[Public] 双击桌面屏幕锁屏
ForceInPicture 视频画中画
Apps Manager 应用数据清除/备份/恢复/传输等管理工具
BioProtect X 应用与控制中心开关等指纹或面部解锁加密
BetterCCXI 更新添加 iOS 11 控制中心天气显示
PreviewBegone 去除截图左下角预览
WeatherVane 一款可以在控制中心快速切换WIFI 和蓝牙设备的插件
DoubleTapLock 一款快速锁屏插件
LetMeknow 电话接通震动提醒插件
RealCC 可以在控制中心真正的关闭WiFi 和蓝牙
Snapper 2  截图增强插件
EzCC 款为控制中心添加更多快捷按钮的插件
ShowTouch  一款显示屏幕触控操作的插件
ShieldXI 一款程序锁插件
MagSafe 连接充电时手机弹出MagSafe充电画面
Titan iOS 13画中画插件
Vesta iOS 13右下角上滑呼出资源库
Pinnie iOS 13把信息中常联系人置顶
Ares 把Siri样式变成卡片或者横幅
Quorra iOS 13仿 iOS 14 摄录提示
Scorpion 设置来电通知窗口
Velox Reloaded 桌面应用可展开类似于iOS 14小组件




破解工具
Flex3 程序修改器
LocallAPStore  内购破解
抖音不能停 


主题美化
LockPlusPro 锁屏主题平台
Anemone 主题平台
Zeppelin 运营商个性化
ColorBadges 彩色角标
ModernXI 不一样的信息通知风格
ColorBanners 2 信息通知彩色风格



DynamicPeninsula 灵动半岛
limneos.net/repo


SemiRestore11平刷插件，支持IOS11.3~11.4B3系统
repo.packix.com


IconCert 14，检测签名后应用显示剩余时间，显示在图标上
apt.ss03.cn




Houdini美化软件(iOS 10-10.3.2 and iOS 11-11.1.2)
https://github.com/ihorlaitan/houdini-public




仿iOS 14插件
Titan，画中画插件
Scorpion，通话窗口插件
Velox Reloaded，桌面小组件插件
Back Tap，轻点背面插件
```

### 破解插件

```
# 土拨鼠综合助手
https://share.weiyun.com/3l8BCWg0
```

### 插件备份

```
# Deb备份
https://pan.baidu.com/s/18Kr9bvLt6_7X2zkjJgXVxg?pwd=i2r9
https://www.123pan.com/s/vgn0Vv-ZThFv.html
```

### 必备插件

#### Apple File Conduit “2”

即AFC2，可以允许电脑第三方助手直接访问设备越狱系统文件。

```
// 作者源地址
https://cydia.ichitaso.com/

// halo_michael打包源地址
https://mrmadtw.github.io/repo

// CDSQ打包源地址
http://repo.feng.com
```

#### AppSync

配合AFC2使用。

#### OpenSSH

电脑与手机连接的SSH通道。

#### NewTerm 2

终端工具。

```
https://cydia.hbang.ws
https://repo.chariz.com
```

### IPA签名

#### ReProvision

给IPA自签名。

```
https://repo.incendo.ws/
```

#### NoPlsOCSP

阻止苹果证书被注销。

```
http://xnu.science/repo/
```

#### AppSync Unified

屏蔽苹果对APP签名的检测，即可以直接安装未签名的破解版APP。

```
https://cydia.akemi.ai/
```

### 越狱屏蔽

#### A-Bypass

越狱屏蔽插件。

```
https://repo.rpgfarm.com/
```

#### Liberty Lite

越狱屏蔽插件。

```
http://ryleyangus.com/repo/
```

#### WeChatJailbreakHook

屏蔽微信越狱检测。

#### Shadow

越狱屏蔽插件。

```
https://ios.jjolano.me/
```

#### UnSub

屏蔽越狱检测。

#### FlyJB

越狱屏蔽插件。

```
https://repo.xsf1re.kr/
```

#### Choicy

禁止注入插件。

```
https://opa334.github.io/
```

#### KernBypass

从内核层面屏蔽越狱。若通过官方源安装插件，需要再安装NewTerm插件，并在NewTerm中执行以下命令。

```
su
changerootfs &
disown %1
```

若设备重启后插件失效，需要打开Filza，创建/var/MobileSoftwareUpdate/mnt1文件夹，然后打开NewTerm，输入su，密码为alpine，获取root权限，如此插件即可恢复正常。

安装本插件后，用iCleaner Pro清理垃圾时可能会卡在清理OTA软件更新。且本插件对硬件损伤较大。

```
// 官方源
https://akusio.github.io/

// 嗨客源
http://repo.qqtlr.com/
```

### 虚拟定位与导航

#### GPSmaster-GPS定位大师

虚拟定位、虚拟导航等。

#### Anywhere!--虚拟定位

更改手机定位与手机型号。




#### Fake GPS Pro

虚拟定位。

#### LoactionFakerX

虚拟定位。

### 系统美化

#### SnowBoard

主题插件。

```
https://sparkdev.me/
# 测试源
http://beta.sparkservers.co.uk
```

#### ReachIt

让iPhone单手操作模式下，原先上方空白处变成音乐播放器控制。

```
https://repo.nepeta.me/
```

#### Carrierizer

自定义运营商名称。

#### Anemone

美化主题的平台。

#### Cylinder

主屏幕翻页动画。

```
http://cydia.r333d.com/
```

#### Edge

屏幕圆角加渐变彩条。

```
https://apt.securarepo.io/
```

#### Xeon

运营商自定义。

```
https://nexusrepo.kro.kr/
```

#### LatchKey

自定义X设备锁屏图标。

```
https://repo.daus.ch/
```

#### PerfectTimeXS

修改状态栏时间样式。

```
https://kingmehu.yourepo.com/
```

#### Genesis 2

美化插件。

```
https://repo.packix.com/
https://repo.dynastic.co/
```

#### Riveria

美化插件。

#### Twister

美化插件。

```
https://repo.twickd.com/
```

#### PencilChargingIndicator

拥有和Apple Pencil一样的充电动画。在设置中的Customize Notifications打开Replace Banners，即可让信息推送获得相同效果。

```
https://shiftcmdk.github.io/repo/
```

#### Bolders

美观放大的文件夹扩展。

```
https://ndoizo.ca/
```

#### Shortlook

漂亮的动画通知。

#### Moveable9

自定义状态栏各个符号的位置或直接隐藏。

```
http://tateu.net/repo/
```

#### Scorpion

来电窗口迷你化。

#### CallBar XS

窗口模式来电通话，可通过CrackTool3的破解补丁破解内购。

```
// 作者源地址
http://limneos.net/repo

// Callbar X破解补丁(基于CrackTool3)源地址
https://apt.cydiaba.cn/

// 免费源地址
https://pulandres.me/repo/
```

#### Atria

自定义桌面布局。

```
https://repo.chariz.com/
```

#### Orion

功能强大的系统调节插件。

```
# 插件源
https://repo.chariz.com/

# 依赖源
https://limneos.net/repo/
https://creaturecoding.com/repo/
https://sparkdev.me/
```

#### Uranium

界面元素自定义。

```
https://repo.packix.com/
```

#### WaveAway

界面元素自定义。

```
https://repo.twickd.com/
```

### 键盘

#### EmojiPort

更新系统的Emoji。

```
https://poomsmart.github.io/repo/
```

#### EmojiPort Fonts

让低版本的iOS系统使用最新的Emoji表情。

```
https://vxbakerxv.github.io/repo/
```

#### Barmoji

为iPhone X/XS/Max/XR键盘底部添加一排常用Emoji表情。

```
https://beta.cpdigitaldarkroom.com/
```

### 控制中心

#### FlipConvert

控制中心更多快捷方式。

```
https://julioverne.github.io/
```

#### RealCC

控制中心直接关闭Wi-Fi与蓝牙而不是之前的断开当前连接。

#### MissionControl

控制中心自定义插件。

```
https://dylanduff.com/repo/
```

#### PowerModule

增强iOS11/12控制中心。

```
https://repo.packix.com/
```

#### Modulus

为控制中心添加模块。若安装后无效，则需手动安装CCsupport插件。

```
// 官方源地址
https://repo.packix.com/

// 免费源地址
http://repo.hackyouriphone.org/
```

#### CCUptime

控制中心查看设备运行时间的插件。

```
https://repo.packix.com/
```

### 通知

#### Goodges 2

应用通知数显示在应用名称上。

```
https://apt.noisyflake.com/
```

#### LocationService (CCSupport)

控制中心添加定位开关。

#### ClearBadges3DTouch10

通过3D Touch功能呼出Clear Badge Notifications按钮，清理应用角标通知。

#### Evelyn's Collection

锁屏挂件。

```
https://evynw.github.io/
```

#### Lisa

锁屏通知聚合。

```
https://esquillidev.github.io/
```

#### Priority Hub

通知按App分类显示。

```
https://kunderscore.gitlab.io/repo/
```

#### Tinybar

缩小通知横幅。

```
http://capt.dreamcode.it
```

#### Priority Hub

锁屏通知小图标显示插件。

```
https://kunderscore.gitlab.io/repo/
```

#### NotifierDots

iPhone X右上角通知图标。

```
http://apt.mumiantech.com/
```

#### Eyeplugs

隐藏特定应用的通知。

```
https://repo.dynastic.co/
```

#### KeepItSimple

快速清除锁屏通知。

```
https://repo.packix.com/
```

### 设置页面

#### ModernSettings

为设置页提供更多选项。

#### System Info

显示更多系统信息。

```
https://apt.xninja.xyz/
https://apt.arx8x.net/
```

#### Shuffle

设置页面归类。

```
https://creaturecoding.com/repo/
```

#### SettingsWidgets

在设置页面显示小组件。

```
https://shepgobarepo.github.io/
```

#### SettingsButtons

为设置界面增加按钮。

```
https://repo.twickd.com/
```

### 系统清理与加速

#### AnimationsBeFast

iOS系统动画加速。

#### iCleaner Pro

清理手机缓存垃圾。

```
# 正式源
https://ib-soft.net/repo

# 测试源
https://ib-soft.net/repo/beta
```

#### CacheClearerX

清理缓存插件。

```
https://alexpng.github.io/
```

#### QuitAll

一键清理后台。

```
https://repo.chariz.com/
```

### 越狱配置

#### ConditionalWiFi5

解决越狱之后新下载的APP没有联网权限的问题。

#### CrashReporter

记录插件问题，便于排查。

```
https://revulate.dev/
```

#### FLEXall

状态栏手势调用FLEX。

```
https://DGh0st.github.io/
```

#### DenyPhotoAlbums

隐藏照片应用里没必要的分类相簿。

#### Bakgrunnur

保持程序在后台运行。

#### Succession

平刷工具。

```
https://samgisaninja.github.io/
```

#### SnapBack

创建系统快照。其中Orig-fs是越狱工具创建的快照备份，不可删除，可用于恢复到原始状态。一般只对Root分区创建快照，不要对Var分区操作。

#### Cr4shed

崩溃报告记录，有助于寻找导致崩溃的插件。

#### Cydown

免费下载Cydia里的收费插件，提取插件的DEB安装包等。

```
https://julio.hackyouriphone.org/
```

#### Sileo Remover

移除Sileo。

```
https://repo.x4-applegate.com/
```

#### Batchomatic

插件备份。

#### EZTweakList

Cydia平台导出源地址以及插件列表。

```
https://legitcomputerwhisperer.github.io/
```

#### ModernDepictions

让Cydia实现Sileo主题风格。

```
http://repo.pixelomer.com/
```

#### CydiaCounter

统计Cydia所安装插件数量。

```
https://fidele007.github.io/
```

#### WishDia

Cydia愿望清单。

```
https://julioverne.github.io/
```

#### addsource

Cydia源管理增强。

#### DismissProgress

在插件安装界面增加关闭按钮。

```
https://poomsmart.github.io/repo/
```

### 相机/照片与截屏

#### Camera Tools

相机增强工具。

#### TapVideoConfig

直接在相机视频/慢动作里切换拍摄大小。

#### SilentScreenshot

关闭截图咔嚓声、去除截图后停留在左下角的预览、自定义截图闪屏颜色等。

```
https://repo.packix.com/
```

#### DeleteForever

为照片删除添加`Permanently`按钮，永久删除照片。

#### QuickMarkup

将标记按钮添加到照片编辑工具栏中。

#### PhotoSize

查看照片应用图片大小。

#### Aperturize

让iPhone 7/8 Plus和iPhone X双摄设备实现人像模式景深控制。

```
// 官方源地址
https://repo.packix.com/

// 免费源地址
https://repo.xarold.com/
```

#### Shutter Depth Control

与Aperturize插件一样。

```
https://jbrownllama.yourepo.com/
```

#### Snapper 2

强大的截图插件。

#### SneakyCam

息屏拍照或录像。

```
https://haoict.github.io/cydia/
```

### 关机与重启

#### Sentinel

防止手机关机后需重新越狱。

#### PowerSelector

快速注销、关机、软重启。

### 应用增强

#### AppData

查看APP数据。

#### AppTweak13

在应用上滑显示应用详情。

#### Apps Manager

强大的应用数据清除/备份/恢复/传输等管理工具，可通过CrackTool3免费激活内购。

#### LocalIAPStore13

破解应用内购。

```
https://paxcex.github.io/
```

#### Autotouch

录制手指操作，然后自动重复操作。

```
// 官方源
http://apt.autotouch.net/

// 破解插件
链接 / https://pan.baidu.com/s/1CeEpOs3aM-bSsZ7yLONHDw
提取码 / 0fwD
```

#### WeChatCallKit

重回微信与系统电话一样的接听显示界面。

```
http://apt.cydiaba.cn/
https://apt.abcydia.com/
```

#### 微信助手密友

使微信能够转发语音。

```
https://sileo.top
```

#### Aesthyrica

Spotify增强。

```
https://aesthyrica.com/repo/
```

#### Cercube for Youtube

Youtube扩展。

```
https://apt.alfhaily.me/
```

#### Youtube Tools

同上。

```
https://jpet26.yourepo.com/
```

### 应用降级

#### AppStore++

Appstore扩展，可用于应用降级。

```
https://cokepokes.github.io
```

若降级时发现无可用版本，可打开链接以查询特定应用版本的版本号ID。复制要降级到的版本ID，降级时点击Manual Install，将ID填写到对话框即可。

```
https://tools.lancely.tech/
```

#### App Admin

App Store应用降级随意旧版。

```
http://beta.unlimapps.com
```

### 电话与录音

#### xybp888

电话助手。

```
http://apt.htv123.com/
```

#### Call Recorder/SuperRecorder

通话录音插件。

```
http://hacx.org/repo/
```

#### AudioRecorder 2

通话录音插件。

```
http://limneos.net/repo/
```

### Safari

#### Safari Features

让iPhone上的Safari显示iPad样式的标签。

#### FullSafari

同Safari Features。

```
https://repo.lonestarx.net/
```

#### Tabsa

同Safari Features。

```
https://r0wdrunner.github.io/repo/
```

#### Safari Plus

增强iOS设备自带Safari浏览器功能。

```
https://opa334.github.io/
```

#### Selector

实现Safari浏览器单句翻译功能。

```
https://repo.nepeta.me/
```

#### ForceInPicture

开启自带浏览器视频画中画功能。

#### WebShade

网页深色模式。

```
https://wilsonthewolf.github.io/repo
```

### 仿X手势

#### HomeX

旧机型使用iPhone X手势。

```
https://abxyap.github.io/repo/
```

#### HomeGesture

老机型实现iPhone X手势界面操作，可自定义功能设置。若在安装过程中遇到com.spark.libsparkapplist依赖缺失的情况，添加以下源地址即可。

```
https://repo.packix.com
```

#### GestureXS

仿iPhone X手势操作。

### 任务管理

#### KillBackground

一键清理后台插件。

```
http://cydia.ichitaso.com/
```

#### KillX

一键清理多任务后台的插件。

```
https://cemck.github.io/repo
```

#### KillX Pro

一键清理后台。

#### Kill All Apps

一键清理后台。

```
https://haoict.github.io/cydia/
```

### 系统配置

#### CCTomizer

系统调整插件。

#### FakeModel

伪装设备机型。

#### MilkyWay 2

分屏插件。

#### iNoSleep

锁屏不断开Wi-Fi。

#### Filza File Manager

文件管理器。

```
http://tigisoftware.com/cydia/
```

#### NotesCreationDate13

备忘录显示创建和修改的日期。

#### BatteryRamp

加速充电插件。

#### ScreenRecording Time

显示录屏时长。

#### EasyAppOrientation

锁定应用程序方向。

```
http://m156nrkvv.g2.xrea.com/repo/
```

#### ShowTouch

录屏圆点。

```
https://repo.lonestarx.net/
```

#### LockPlus Pro

锁屏增强。

```
https://junesiphone.com/supersecret/
```

#### Xeninfo

```
http://junesiphone.com/supersecret/
```

#### Xen HTML

向主页面和锁屏页面添加组件。

```
https://xenpublic.incendo.ws/
```

#### CarBridge

解除CarPlay限制，让第三方APP运行于CarPlay。

```
https://repo.chariz.io/
```

#### SwipeToDeleteContact

直接可在iPhone通讯录里左滑删除名片。

#### BetterRotate

在屏幕锁定旋转的情况下允许视频横向旋转。

```
// 官方源地址
https://repo.packix.com/

// 免费源地址
http://repo.hackyouriphone.org/
```

#### WiFi Passwords

查看连接过的Wi-Fi密码。

#### PerfectNetworkSpeedInfo

网速悬浮窗。

```
https://johnzaro.github.io/cydia/
```

#### Copypasta

剪贴板管理器。

```
https://repo.litten.love/
```

#### AV Orientation Lock

视频播放器中添加方向锁定按钮。

```
https://repo.packix.com/
```

### 闹钟

#### NextAlarm14

显示下一个闹钟信息。

```
http://tateu.net/repo/
```

#### NappyTime14

显示闹钟倒计时。

```
# 插件源
https://arya1106.github.io/repo/

# 汉化源
http://repo.qqtlr.com/
```

### 电话

#### CallFavorites

电话三维触控菜单显示收藏联系人。

```
https://repo.pixelomer.com/
```

### 电量

#### DrainCheck

记录DND/LPM时间内的电量消耗情况。

```
# 插件源
https://ginsudev.github.io/repo/

# 汉化源
http://repo.qqtlr.com/
```

#### LowLock

锁屏低电量。

```
https://miro92.com/repo/
```

### 暂不可用插件

#### KillMyApps

锁屏不杀后台插件。

#### PreferenceOrganizer 2

对设置里的`系统设置`、`插件设置`、`App Store`进行归类整理。

```
https://cydia.angelxwind.net
```

#### FingerTouch

类似`Touchr`，可以让Home键实现触控操作，比如单触返回桌面，双触打开后台，长触锁屏等，无需按下去，起到保护Home键的作用。

#### BetterShutdown

替关机键增加重启和注销选项。

#### CrackTool3

一键免费激活插件的内购项目。 

#### SafeShutdown

软关机。

```
https://kurrt.com/repo/
```

#### VideoAdsSpeed

视频软件广告跳过。

## 插件操作

### 重置插件设置

用Filza删除/var/mobile/Library/Preferences中的对应plist文件即可。 

## 插件出错排查

若安装的插件不兼容或者冲突，一般会进入安全模式。此时打开Sileo卸载有问题的插件即可。

可使用PowerSelector一键进入安全模式。

# 补丁软件

补丁软件可以修改APP，达到破解效果。

## Flex 3

### 安装和破解

源地址如下。

```
https://getdelta.co/
```

登录以下账号即可永久使用会员。

```
账号 / sundasheng521@qq.com
密码 / 7758521a
```

### 手动添加补丁

打开Filza，进入路径/var/mobile/Library/Application Support/Flex3，使用文本编辑器打开patches.plist文件，拉到最下方，在末尾的`<dict>`后复制要添加的补丁代码，保存即可。

### 补丁制作

下载FlexTool插件，在设置中选择要修改的APP。打开后可通过FlexTool定位到需要修改的函数位置。打开Flex，点击+号，选择要修改的APP，点击Add Units，点击APP名称以进行初次编译。编译完成后搜索刚才找到的函数，勾选并返回到补丁主界面。点击刚才选择的函数，修改其返回值即可。

### 补丁列表

#### 支付宝十万步数

```
<dict>
    <key>UUID</key>
    <string>0A3D50C5-F2C6-4933-B997-C1647F3E109A</string>
    <key>apiVersion</key>
    <integer>3</integer>
    <key>appIdentifier</key>
    <string>com.alipay.iphoneclient</string>
    <key>author</key>
    <string>hyczby5218</string>
    <key>cloudDescription</key>
    <string>支付宝修改步数</string>
    <key>cloudID</key>
    <integer>46823</integer>
    <key>downloadDate</key>
    <integer>1551604711</integer>
    <key>name</key>
    <string>支付宝修改步数</string>
    <key>switchedOn</key>
    <true/>
    <key>units</key>
    <array>
        <dict>
            <key>methodObjc</key>
            <dict>
                <key>className</key>
                <string>APStepInfo</string>
                <key>displayName</key>
                <string>-(long long) numberOfSteps</string>
                <key>prefix</key>
                <string>-</string>
                <key>selector</key>
                <string>numberOfSteps</string>
                <key>typeEncoding</key>
                <string>q16@0:8</string>
            </dict>
            <key>name</key>
            <string>修改步数</string>
            <key>overrides</key>
            <array>
                <dict>
                    <key>argument</key>
                    <integer>0</integer>
                    <key>type</key>
                    <dict>
                        <key>subtype</key>
                        <integer>0</integer>
                        <key>type</key>
                        <integer>9</integer>
                    </dict>
                    <key>value</key>
                    <dict>
                        <key>type</key>
                        <integer>9</integer>
                        <key>value</key>
                        <integer>100000</integer>
                    </dict>
                </dict>
            </array>
        </dict>
    </array>
</dict>
```

#### Thor移除验证设备验证

```
<dict>
    <key>UUID</key>
    <string>AE872A89-5EB7-483D-B50A-88C29F6E20C9</string>
    <key>apiVersion</key>
    <integer>3</integer>
    <key>appIdentifier</key>
    <string>com.pixelcyber.dake.thor</string>
    <key>author</key>
    <string>zjh521901</string>
    <key>cloudDescription</key>
    <string>thor移除验证设备验证补丁</string>
    <key>cloudID</key>
    <integer>46126</integer>
    <key>downloadDate</key>
    <integer>1552100989</integer>
    <key>name</key>
    <string>Thor ·Fz夜</string>
    <key>switchedOn</key>
    <false/>
    <key>units</key>
    <array>
        <dict>
            <key>methodObjc</key>
            <dict>
                <key>className</key>
                <string>AppReceiptValidator</string>
                <key>displayName</key>
                <string>-(void) prepareVerify</string>
                <key>prefix</key>
                <string>-</string>
                <key>selector</key>
                <string>prepareVerify</string>
                <key>typeEncoding</key>
                <string>v16@0:8</string>
            </dict>
            <key>name</key>
            <string>🈲贩卖🈲谋利🈲</string>
            <key>overrides</key>
            <array/>
        </dict>
    </array>
</dict>
```

## SuperCharge

SuperCharge脚本的后缀名为st。将st脚本发送到手机，并用SuperCharge打开即可。源地址如下。

```
https://repo.supercharge.app/
```

# 免越狱安装越狱软件

## FilzaEscaped

可用于iOS 15及以下系统，下载链接如下。

```
https://basvtdevelopments.com/filzaescaped
```

### 使用

#### 修改X手势

```
1.打开FilzaEscaped 
2.找路径：
/var/containers/Shared/SystemGroup/systemgroup.com.apple.mobilegestaltcache/Library/Caches/com.apple.MobileGestalt.plist 
3.打开com.apple.MobileGestalt.plist
4.选择CacheExtra-再找oPeik/9e8lQWMszEjbPzng 
5.ArtworkDeviceSubType 点击右侧（i） 
6.注意这个值一定要记得，预防你想恢复原来的样子，想恢复改为原来值就行。 
7.值修改为：2436
8.点击右上角存储，打开设置-显示与亮度-视图-改为放大，再改为标准即可实现X手势。
```

## PlankFilza

```
https://github.com/brandonplank/PlankFilza
```

### 使用

#### 修改为iPhone X手势

```
打开PlankFilza工具

找路径：

/var/containers/Shared/SystemGroup/systemgroup.com.apple.mobilegestaltcache/Library/Caches/com.apple.MobileGestalt.plist

找到：

CacheExtra↓

oPeik/9e8IQWMszEjbPzng↓

ArtworkDeviceSubType

点击右边的符号，把这个值记起来，因为你恢复时，需要这个值恢复，不然就很麻烦。

然后把这个值改成：2436

然后点“储存”

然后手机重启一下，这样就行了






恢复方法：
把 2436值改成原来数值，然后点右上角“储存”，重启一下手机即可恢复。
```

#### 修改运营商名称

```
修改方法：
打开PlankFilza工具

找路径：

var/mobile/Libray/Carrier Bundles/Overlay

找这里就会显示你所有运营商文件，打开设置-通用-关于本机，看看运营商版本号多少，然后修改此版本号的文件。

移动：46000 联通：46001 电信：46007

点击进去，往下拉找到“StatusBarlmages”文件

找到 ltem 0 和ltem 1 或 ltem 2

把中国移动/中国联通/中国电信修改你喜欢名字

然后右上角保存，返回Overlay点击右侧符号

把访问权限所有者隐藏改成0555，右上角存储

然后重启手机即可修改成功
```

#### 修改关于本机信息

```
这个就是可以修改关于本机型号及版本号等，直接找路径：
/var/containers/Shared/SystemGroup/systemgroup.com.apple.mobilegestaltcache/Library/Caches/com.apple.MobileGestalt.plist
然后看看，你想修改什么内容，右上角保存，然后重启手机即可修改成功。
```

# 使用

## 建立SSH连接

将手机连接到电脑后，在电脑端打开爱思助手，在工具箱中选择打开SSH连接，记下IP和端口，其中127.0.0.1可以用localhost代替。

打开终端并输入以下命令，即可建立SSH连接。

```
// 2222为端口，localhost为IP
ssh -p 2222 root@localhost
```

## 屏蔽系统更新

若为Unc0ver越狱，打开Unc0ver软件即有相关设置。其它平台可安装OTADisabler插件，官方源如下。

```
https://cydia.ichitaso.com
```

也可用iCleaner Pro屏蔽，在启动项中关闭OTA Update即可。

也可用OTADisabler插件屏蔽。相反，OTAEnabler可启用系统更新。

## 国行Apple Watch开通ECG

系统要求为Watch OS 6.25+，iOS 13.5+。

手机越狱后安装ECG Enabler插件，安装完成后重新配对手表即可。成功开启后可以清除越狱，只要不删除iPhone与Apple Watch之间的配对即可。

## 应用多开

### 方法

通过Slice 3插件即可。

### 添加插件支持

打开Cydia，找到需要生效的插件，点击显示软件包内容，展开到DynamicLibraries，应当有xx.dylib和xx.plist两个文件。用Filza定位到以上位置，点开xx.plist，点击Bundles后面的`i`，点击`+`号后填写要添加插件支持的应用唯一标志符即可。该标志符可通过Filza的应用程序管理功能得到。

## 应用砸壳

AppStore下载的应用进行了数字版权加密处理，加密后的App无法被反编译。砸壳可以将已经安装的App解密，导出为ipa格式的文件。

砸壳后才能自定义使用，如修改标识符实现双开、注入插件实现增强等，砸壳后可以跳过ID验证。

砸壳后的App在越狱手机上可直接用Filza安装。

### 通过frida-ios-dump

该方法适用于Mac，需要APP能够正常打开。

#### 电脑端配置

确保电脑已安装frida，Mac可通过Homebrew安装。安装完成后运行以下命令，若出现`Waiting for USB device to appear...`，则成功。

```
frida-ps -U
```

运行以下命令以克隆仓库。

```
git clone https://github.com/AloneMonkey/frida-ios-dump.git
```

连接手机到电脑，执行以下命令，查看手机是否已经被识别。

```
frida-ls-devices
frida-ps -U
```

#### 手机端配置

完成电脑端配置后，需要在手机上安装frida-server，版本需要和电脑端匹配。

打开以下链接下载与电脑的frida对应的frida-server，并用Filza复制到手机的/usr/bin中，重命名为frida-server。在手机上用Filza找到刚刚复制进去的文件，调整其属性，将权限全部开放。

然后在手机打开NewTerm，运行以下命令完成安装。

```
cd /usr/bin
frida-server --version
frida-server -D
```

frida-server也可用Cydia安装。打开Cydia并添加以下源安装即可，若无法下载可换用嘻哈源。

```
https://build.frida.re
```

#### 砸壳操作

将手机和电脑进行连接，并确保两者连接到同一个Wi-Fi。

用爱思助手打开手机的SSH通道，然后打开克隆好的仓库，修改dump.py中端口为连接手机SSH的端口。打开终端，执行以下命令以与手机进行SSH连接。

```
ssh root@127.0.0.1 -p1025
```

再新建一个终端，执行以下命令以列出手机所有安装的应用。

```
cd frida-ios-dump
python3 dump.py -l
```

输入以下命令以进行砸壳。若提示`unable to access process with pid 1 from the current user account`，则需先在手机上打开APP。砸壳完成的APP将放到电脑的终端当前路径。

```
python3 dump.py [APP名称]（或BundleID）
```

### 通过插件

该方法需要APP能够正常打开。

#### CrackerXI+

安装CrackerXI+插件，源地址如下。

```
https://cydia.iphonecake.com/
```

打开插件，在设置中打开CrackerXI Hook、Remove UISupportedDevices、Set Minimum iOS version 10.0和Remove Watch App。在插件首页点击要砸壳的APP即可，砸壳后的APP放在手机的/var/mobile/Documents/CrackerXI/中。

#### DumpDecrypter

源地址如下。

```
https://repo.initnil.com/
```

#### AppsDump

```
https://pan.baidu.com/s/1AwXfV5BsMj4Pf_bTaQUXUw?pwd=42ni
https://www.123pan.com/s/vgn0Vv-vfHFv.html
```

## 游戏修改

### 必要插件

一般需要IGG和Filza。

IGG全称为iGameGuardian，用于修改游戏。安装用嘻哈源即可，官方源如下。

```
https://aquawu.github.io/igg/
```

进入插件配置，打开使用储存空间，下限设为0x00000000，上限设为0x150000000，并打开移除未匹配项，打开有号数，完成配置。

### 王者荣耀

#### 修改快捷信息/语音库

在语音库中清除任意数量的信息（比如4个），然后用`小心草丛`填入，不要点确定。切到IGG，选择smoba，I32搜索1。

切回王者荣耀，删除4个`小心草丛`，选择4个`敌人消失`填入，不要点确定，再次切到IGG，I32搜索2。

切回王者荣耀，删除4个`敌人消失`，选择4个`回防高地`填入，不要点确定，再次切到IGG，I32搜索3。

切回王者荣耀，删除4个`回防高地`，选择4个`请求支援`填入，不要点确定，再次切到IGG，I32搜索4。

切回王者荣耀，删除4个`请求支援`，分别选择`小心草丛`、`敌人消失`、`回防高地`、`请求支援`，不要点确定。再次切到IGG，上次得到的4-5个结果分别变成了1、2、3、4、5，把这几个数字换成所需要的消息，点确定即可。

详细的快捷消息代码如下。

| 代码 | 含义           |
| ---- | -------------- |
| 1    | 小心草丛       |
| 2    | 敌人消失       |
| 3    | 回防高地       |
| 4    | 请求支援       |
| 5    | 猥琐发育，别浪 |
| 6    | 注意野区       |
| 7    | 保护我方输出   |
| 8    | 小心对方偷塔   |
| 9    | 稳住，我们能赢 |
| 10   | 大家打开语音   |
| 11   | 跟着我         |
| 12   | 注意分散站位   |
| 13   | 注意保护后排   |
| 14   | 等等我，马上到 |
| 15   | 技能不全，撤退 |
| 16   | 状态不好，撤退 |
| 17   | 撤退，别打主宰 |
| 18   | 撤退，别打暴君 |
| 19   | 我回防高地     |
| 20   | 集合埋伏       |
| 21   | 清理兵线       |
| 22   | 我拿buff，谢谢 |
| 23   | 集合准备团战   |
| 24   | 拖住，我偷塔   |
| 25   | 集合进攻暴君   |
| 26   | 集合进攻主宰   |
| 27   | 全体进攻对抗路 |
| 28   | 全体进攻中路   |
| 29   | 全体进攻发育路 |
| 30   | 优先攻击输出   |
| 31   | 优先攻击突进   |
| 32   | 优先推塔       |
| 33   | 准备越塔强攻   |
| 34   | 正在前往支援   |
| 35   | 上去开团       |
| 36   | 等我集合打团   |
| 37   | 我来抓人了     |
| 38   | 大招已经好了   |
| 39   | 我去抓对抗路   |
| 40   | 我去抓中路     |
| 41   | 我去抓法语撸   |
| 42   | 干得漂亮       |
| 43   | 让我独享经验   |
| 44   | [全部]抱歉     |
| 45   | [全部]谢谢你   |
| 46   | [全部]挑衅     |
| 47   | 我拿BUFF再团   |
| 48   | 别团，等人齐   |
| 49   | 收到           |
| 50   | 多观察小地图   |
| 51   | 团不过先发育   |
| 52   | 清完兵线再团   |
| 53   | 继续推我回防   |
| 54   | 当心敌方陷阱   |
| 55   | I'll carry you |
| 56   | 集合反蓝       |
| 57   | 集合反红       |
| 58   | 集合入侵野区   |
| 59   | 帮我看红，谢谢 |
| 60   | 帮我看蓝，谢谢 |
| 61   | 我单带，别接团 |
| 62   | 经济落后，别团 |
| 63   | 兵线没到，撤退 |
| 64   | 敌人偷塔，撤退 |
| 65   | 打团了，别单带 |
| 66   | 等我复活在团   |
| 67   | 注意敌方绕后   |
| 68   | 法师来拿蓝     |
| 69   | 射手来拿红     |
| 70   | 求让BUFF，谢谢 |
| 71   | 交个朋友       |
| 72   | 新年快乐       |
| 73   | 集合进攻龙王   |
| 74   | 撤退，别打龙王 |
| 79   | 阵营-长城      |
| 80   | 阵营-玄雍      |
| 81   | 阵营-稷下      |
| 86   | 裂开吧！敌人   |
| 91   | 打团请保护我   |
| 92   | 兄弟，我来了   |
| 93   | 别急，有我     |
| 94   | 大佬，救命     |

#### 修改超长名字

老账号使用改名卡，在输入框不要输入东西。新账号则在创建昵称界面不要输入文字。

打开IGG并选择smoba，设置临近范围为0x9，然后i32搜索12。打开联合，i32搜索86400，这时只有一个结果。

把12改成64，然后清除。再次打开设置，设置临近范围为0x21，然后i32搜索42。打开联合，i32搜索12，把12改成88。

此时回到游戏界面修改即可，最长21个汉字或61个字母。

#### 设置三个相同的想玩英雄

在排位赛页面清除所有自定义英雄，选择第一个英雄（如赵云）。打开IGG并选择smoba，然后I32搜索107（赵云）。

去掉赵云，再次选择一个英雄（如小乔）。打开IGG，I32搜索106（小乔），此时结果只剩下一个。

点击内存，再点击右上角移动，此时会跳转到刚才的结果，且底部的两个数值为0。返回王者荣耀，随便选择三个英雄，再打开IGG，此时三个数字均发生变化。

修改该三个数字为所需要的英雄代码。返回王者荣耀，点击确定，出现三个一样的头像即可。

头像代码如下。

| 代码 | 含义                 |
| ---- | -------------------- |
| 105  | 廉颇                 |
| 106  | 小乔                 |
| 107  | 赵云                 |
| 108  | 墨子                 |
| 109  | 妲己                 |
| 110  | 嬴政                 |
| 111  | 孙尚香               |
| 112  | 鲁班七号             |
| 113  | 庄周                 |
| 114  | 刘婵                 |
| 115  | 高渐离               |
| 116  | 荆轲                 |
| 117  | 钟无艳               |
| 118  | 孙斌                 |
| 119  | 扁鹊                 |
| 120  | 白起                 |
| 121  | 芈月                 |
| 122  | 诸葛亮（旧）         |
| 123  | 吕布                 |
| 124  | 周瑜                 |
| 125  | 庞统                 |
| 126  | 夏侯淳               |
| 127  | 甄姬                 |
| 128  | 曹操                 |
| 129  | 典韦                 |
| 130  | 宫本武藏             |
| 131  | 李白                 |
| 132  | 马可波罗（旧）       |
| 133  | 狄仁杰               |
| 134  | 达摩                 |
| 135  | 项羽                 |
| 136  | 武则天               |
| 137  | 司马懿               |
| 138  | 孔夫子               |
| 140  | 关羽                 |
| 141  | 貂蝉                 |
| 142  | 安琪拉               |
| 143  | 安绿山               |
| 144  | 程咬金               |
| 146  | 露娜                 |
| 148  | 姜子牙               |
| 149  | 刘邦                 |
| 150  | 韩信                 |
| 152  | 王昭君               |
| 153  | 兰陵王               |
| 154  | 花木兰               |
| 155  | 艾琳                 |
| 156  | 张亮（旧）           |
| 157  | 不知火舞             |
| 158  | 八神庵               |
| 162  | 娜可露露             |
| 163  | 橘右京               |
| 166  | 亚瑟                 |
| 167  | 孙悟空               |
| 168  | 牛头                 |
| 169  | 后裔                 |
| 170  | 刘备                 |
| 171  | 张飞                 |
| 173  | 李元芳               |
| 174  | 虞姬                 |
| 175  | 钟馗                 |
| 176  | 杨玉环               |
| 177  | 成吉思汗             |
| 178  | 杨戬                 |
| 179  | 女娲                 |
| 180  | 哪吒                 |
| 182  | 干将（旧）           |
| 183  | 雅典娜               |
| 184  | 蔡文姬               |
| 186  | 太乙真人             |
| 187  | 东皇太一             |
| 189  | 鬼谷子               |
| 190  | 诸葛亮               |
| 191  | 大乔                 |
| 192  | 黄忠                 |
| 193  | 凯                   |
| 194  | 苏烈                 |
| 195  | 百里玄策             |
| 196  | 百里守约             |
| 197  | 异星                 |
| 198  | 梦琪                 |
| 199  | 公孙离               |
| 207  | 封网                 |
| 222  | 徐福                 |
| 225  | 庞统（傀儡）         |
| 237  | 司马懿               |
| 240  | 觉醒关羽             |
| 241  | 金龙                 |
| 242  | 黑龙                 |
| 243  | 红龙                 |
| 244  | 绿龙                 |
| 254  | 花木兰（重剑）       |
| 305  | 廉颇（改版）         |
| 310  | 嬴政                 |
| 312  | 沈梦溪               |
| 320  | 宫本武藏（强化）     |
| 332  | 马可波罗             |
| 333  | 马可波罗（加强）     |
| 346  | 露娜（改版）         |
| 355  | 艾琳（改版）         |
| 356  | 张亮                 |
| 366  | 亚瑟（改版）         |
| 378  | 杨戬（改版）         |
| 382  | 干将                 |
| 501  | 明世隐               |
| 502  | 裴擒虎               |
| 503  | 狂铁                 |
| 504  | 米莱狄               |
| 505  | 瑶                   |
| 506  | 云中君               |
| 507  | 李信                 |
| 508  | 伽罗                 |
| 509  | 盾山                 |
| 510  | 孙策                 |
| 511  | 猪八戒               |
| 512  | 囚徒                 |
| 513  | 上官婉儿             |
| 515  | 嫦娥                 |
| 516  | 舜                   |
| 518  | 马超                 |
| 520  | 少司命               |
| 522  | 东方曜               |
| 523  | 西施                 |
| 524  | 蒙犽                 |
| 525  | 鲁班大师             |
| 526  | 王翦                 |
| 527  | 蒙恬                 |
| 528  | 吕蒙                 |
| 529  | 盘古                 |
| 530  | 宫本武藏（改版）     |
| 531  | 镜                   |
| 532  | 镜（分身）           |
| 533  | Zombie               |
| 620  | 韩信                 |
| 621  | 庄周                 |
| 630  | 宫本武藏（改版强化） |
| 654  | 母僵尸               |
| 668  | Zombie               |
| 675  | Zombie               |
| 687  | Zombie               |
| 700  | 坦克轮子             |
| 701  | 坦克炮管             |
| 732  | 马可波罗（旧）       |

NPC皮肤代码如下。

| 代码 | 含义             |
| ---- | ---------------- |
| 770  | 暴君             |
| 771  | 主宰（英雄）     |
| 772  | 暴君3vs3（英雄） |
| 773  | 年兽（英雄）     |
| 2025 | 暴君3vs3         |
| 2145 | 主宰             |

技能代码如下。

| 代码  | 含义     |
| ----- | -------- |
| 80101 | 监视     |
| 80102 | 治疗     |
| 80103 | 晕眩     |
| 80104 | 惩戒     |
| 80105 | 干扰     |
| 80106 | 隐遁     |
| 80107 | 净化     |
| 80108 | 斩杀     |
| 80109 | 疾跑     |
| 80110 | 狂暴     |
| 80111 | 振奋     |
| 80112 | 月之守护 |
| 80115 | 闪现     |
| 80116 | 寒冰惩戒 |
| 80117 | 传送     |
| 80121 | 弱化     |
| 80122 | PVE闪现  |
| 81102 | 坚定意志 |
| 81107 | 不灭信仰 |
| 81109 | 奔狼号令 |
| 81110 | 幽梦之息 |
| 81112 | 时光凝滞 |

地图代码如下。

| 代码  | 含义                   |
| ----- | ---------------------- |
| 4010  | 变身大乱斗（地图）     |
| 41150 | 抢鲲大乱斗（娱乐地图） |
| 20001 | 经典1v1（竞技地图）    |
| 20002 | 经典3v3（竞技地图）    |
| 20009 | 火焰山（娱乐地图）     |
| 20011 | 经典5v5（竞技地图）    |
| 20012 | 无限乱斗（娱乐地图）   |
| 20013 | 无限火力（娱乐地图）   |
| 20015 | 迷雾模式（娱乐地图）   |
| 20016 | 荣耀峡谷（竞技地图）   |
| 20017 | 无限乱斗（娱乐地图）   |
| 20018 | 实战训练（地图）       |
| 20028 | 五军对决（娱乐地图）   |
| 20040 | 实战模拟（快速地图）   |
| 20041 | 实战模拟（标准地图）   |
| 20071 | 抢鲲模式（娱乐地图）   |
| 20072 | 强化模式（娱乐地图）   |
| 20080 | 快跑模式（娱乐地图）   |
| 20099 | 无塔1v1（竞技地图）    |
| 20699 | 无塔3v1（竞技地图）    |
| 20999 | 无塔5v5（竞技地图）    |
| 20111 | 征兆模式（竞技地图）   |
| 20151 | CS模式（竞技地图）     |
| 21000 | 峡谷大乱斗（地图）     |
| 21001 | 峡谷大乱斗（地图）     |

攻速上限代码如下。

```
163840000；32768000
```

TS视角如下。

```
1500000-165000  修改167888
Online-基址FD38  Beta-基址FD68
154444-174444  修改160001
```

3D视角如下。

```
1097285734
```

上帝视角如下。

```
1,081,006,571;
-1，25，
-1,125,398;
-1,082,130,432;
-1,088,838,298::37
改
-1,082,130,32
```

#### 体验隐藏角色

打开王者荣耀，进入单机模式，选择任意难度进入房间。任意选择英雄，此处以廉颇（105）为例。切到IGG，选择smoba，I32搜索105。

回到王者荣耀，选择小乔（106），切到IGG，I32搜索106。点右上角开关，再点左上角全改，此处测试将数值改成武则天（136）。回到王者荣耀，点击皮肤，变成武则天，开局。

退出本局游戏，再开一次房间，任意选个英雄，切到IGG，点全改，改为艾琳（155）或年兽（773）等其他特殊隐藏角色/NPC。回到王者荣耀，开局即可。

若读条中英雄图像是白色，并且游戏崩溃，则划掉后台，重新操作。如再次操作依然崩溃，说明该英雄资源不存在，不支持修改。

#### 享受定位特权

下载王者人生APP并打开，可查找到能享受特权的店铺。以KFC为例，记住其位置。下载Wifi万能钥匙，查找该特权店对应的Wi-Fi名称，此处为KFC FREE WIFI。

安装Fake GPS Pro插件，进入Wifi信息修改，设置成KFC FREE WIFI，MAC地址可以任意。安装Anywhere插件，寻找到特权店铺的位置，扎标，然后点击上面跳出的地址推荐，再选择应用到王者荣耀和王者人生。进入王者荣耀，若出现王者特权提示，则成功。否则需要在Anywhere中微调位置直至出现弹窗为止。

或者可以通过修改经纬度的方式实现修改定位。打开flex3，添加王者荣耀，搜索privilegeManager，进入后再搜setuserlatitude longitude。勾选后返回，输入特权店的经纬度即可。修改Wi-Fi和MAC地址则同上。

#### 修改荣耀战区

修改定位方法同上，修改定位完成后进入游戏即可修改战区。注意每周一才可修改。

#### 修改技能效果

下列步骤必须在游戏中进行修改，且必须落地后在购买装备点击技能的时候进行修改。注意一局只能修改一次。

打开王者荣耀后，IGG进入smoba，I32搜索`英雄代码+00`，如搜索墨子则为10800。打开联合开关，搜索100，关闭联合开关，搜索`英雄代码+00`，全部改0即可。

常见代码及对应技能如下。

| 代码 | 英雄名称 |        技能效果        |
|------|----------|------------------------|
|  108 | 墨子     | 无限击退（被动）       |
|  112 | 鲁班     | 无限biubiubiu（被动）  |
|  114 | 刘禅     | 无限锤（一技能）       |
|  129 | 典韦     | 无限乱舞（一技能）     |
|  133 | 狄仁杰   | 无限卡牌（强化被动）   |
|  134 | 达摩     | 无限回血（强化被动）   |
|  146 | 露娜     | 无限标记（强化被动）   |
|  149 | 刘邦     | 无限剑气               |
|  150 | 韩信     | 无限挑飞               |
|  152 | 王昭君   | 无限下雨               |
|  162 | 娜可露露 | 强化普攻               |
|  163 | 橘右京   | 无限拔刀斩（被动）     |
|  166 | 亚瑟     | 无限沉默（被动）       |
|  167 | 孙悟空   | 无限棍子（被动）       |
|  170 | 刘备     | 无限两次弹射           |
|  194 | 苏烈     | 无限击飞               |
|  198 | 梦琪     | 无限三爪击             |
|  305 | 廉颇     | 强化普攻               |
|  503 | 狂铁     | 无限击飞               |
|  506 | 云中君   | 飞行状态下无限撕裂状态 |
|  510 | 孙策     | 无限回血（被动）       |

### 激门峡谷

以下方法不适用于iOS13以下系统。

打开Filza，找到APP管理器，点击LOL右侧的`i`。点击主程序，进入wildrift.app目录，找到主程序wildrift，点击右侧的`i`。点击打开方式-十六进制编辑器，返回后点击主程序wildrift。

按照以下内容完成修改后保存，然后重新下载游戏以避免闪退。

#### 透视

选择左侧地址码并输入`01FD1FD0`，将`08 41 40 39`修改为`08 00 80 D2`。

#### 除雾

选择左侧地址码并输入`01FCF238`，将`49 71 5F B8`修改为`C0 03 5F D6`。

### 王牌战士

必须在游戏中进行修改，必须加载完之后修改，一局只能修改一次。

#### 游击型英雄无后

F32搜25，全部改0即可。

#### 赵海龙独立无后

F32搜25，联合F32搜35，全部改0即可。

#### 赵海龙隐身

F32搜0.0025，全部改99即可。

#### 自瞄

游戏中需要关闭辅助瞄准。

i64搜4816377120，联合i64搜3212836864，修改为4811533255104790528即可。


# 应用商店

应用商店可在Cydia/Sileo通过插件的方式安装。

## Zebra

作者源如下。

```
https://getzbra.com/repo/
https://getzbra.com/beta/
```

## Installer

作者源如下。

```
https://apptapp.me/repo/
```

## AltStore

添加以下源并按顺序安装AltDaemon、AppSync Unified、appinst (App Installer)、AltStore (ALPHA)即可。

```
https://pwnders.github.io/repo/
https://cydia.akemi.ai/
```

# 常见问题

## Cydia无法联网

可用爱思助手安装1.2.3版本的乐网，暂时让Cydia联网。然后安装`连个锤子-越狱联网一键修复`插件，官方源如下。打开插件，点击修复网络，等待修复完成后注销即可。

```
http://apt.tinyapps.cn/
```

## Cydia提示证书无效

修改系统时间到当前时间即可。

## Cydia出现红色Depend依赖

缺失依赖的越狱源失效，翻墙后重新刷新源即可。

## Sileo提示「unable to fetch some archives」

若在手机端已安装NewTerm插件，则打开NewTerm并执行以下命令即可。若未安装，则先通过SSH使电脑连接到手机，然后在电脑端执行以下命令。

```
apt-get update --fix-missing
killall SpringBoard
```

## Slieo提示Target Packages is configured multiple times

在Slieo和Cydia中添加的源地址与名字冲突，在两边均添加同一个源时可能会出现该情况。总体不影响使用，建议卸载Cydia，并在NewTerm输入以下命令以移除cydia.list文件。

```
/etc/apt/sources.list.d
```

## Slieo提示trying to overwrite 'xxx'，with is also in package xxx 1.0

安装冲突。可以先卸载提示中的安装包，再尝试安装。

## Slieo提示missing 'Description' field

插件包缺少描述等相关信息。不影响环境和插件的使用。

## Slieo提示'Version' field value '': version string is empty

部分插件或脚本破坏了越狱环境。打开Fliza，进入路径/var/lib/dpkg/status，备份status文件后点击，选择文本编辑器模式打开。搜索lib.corefoundation，在Version一栏填写1676.10，存储即可。

## Slieo提示Depends PreferenceLoader

切换到软件包选项卡，点击日期旁边的三条横线，然后点击开发者以切换到开发者模式，然后搜索PreferenceLoader，安装即可。

## 无法正常内购

此问题是itunesstored进程无法正常启动所导致。若安装了内购破解插件，如LocalIAPStore，先尝试关闭或卸载。

若无效，则可使用DaemonDisabler开启itunesstored进程，插件源如下。安装后在插件设置中打开com.apple.itunesstored.plist，然后软重启。

```
https://level3tjg.xyz/repo/
```

若仍无效，可尝试使用Choicy禁止注入itunesstored。安装Choicy插件并进入Choicy设置，选择进程配置-守护进程，拉到最底部，点击显示所有守护进程，找到itunesstored后点击进入，打开禁用插件注入开关。注销并测试是否可以正常内购。

若依旧无法正常内购，可尝试重启或在安全模式下进行内购。

## Chimera1n玩游戏闪退

删除OpenSSH即可。

## 电脑连接SSH时提示「WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!」

打开用户目录下的.ssh/known_hosts文件，将有手机IP地址的记录删掉即可。

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

## iOS越狱教程-内存基址修改器的外番(二)

```
https://mp.weixin.qq.com/s/LJAhI2pGOlnmxne6TAnBCw
```

## iOS越狱教程-内存基址修改器的神奇之处(十二)

```
https://mp.weixin.qq.com/s/0Je9UhWjVTuK-WIDcP886A
```

## iOS越狱教程-内存基址修改器的外番(三)

```
https://mp.weixin.qq.com/s/Il5_Gsbf89McIBM70zdNJg
```

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

## iOS 越狱，官方源，作者源，推荐大集合

```
https://mp.weixin.qq.com/s/lSE0PSKA4PT09ChOrEwRRw
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

## 分享三款插件，你一定会喜欢

```
https://mp.weixin.qq.com/s/GkZDxPiU7dBJHmTCFP1q2g
```

## 分享二款实用插件，你一定会喜欢

```
https://mp.weixin.qq.com/s/dmPGwIi6K-hkM-KayaDvFQ
```

## 省电插件已经更新，越狱已经修复

```
https://mp.weixin.qq.com/s/7FFxbg62JpPv9SLtEK2xXQ
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

## iOS 13 也能享用 iOS 14 新功能，赶紧试试

```
https://mp.weixin.qq.com/s/q9yuPIOh0nl4hhoRo7j3AQ
```

## 新发现！iOS16.5.1 越狱方法，支持Windows系统

```
https://mp.weixin.qq.com/s/adD12hcazfibHN4GZFTcZw
```

## iOS 8.4.1 终于可以越狱，长达8年时间

```
https://mp.weixin.qq.com/s/hcVjM3nTP0liIusBIVisiw
```

## iOS 系统，tvOS 风格弹窗，完全免费

```
https://mp.weixin.qq.com/s/gd9MpAeWCgEGTLCaihX1ew
```

## iOS 系统，耳目一新

```
https://mp.weixin.qq.com/s/TQAxvrUhZYKPy-X7XaygOQ
```

## iOS 系统，自定义锁屏，iOS 16 样式

```
https://mp.weixin.qq.com/s/p3iVl7A6byCKIlip4wyyVA
```

## iOS 系统，归类，顶图，新样式

```
https://mp.weixin.qq.com/s/nWj5k6kep8F07tuABfNh-g
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


## 大神宣布，全部免费

```
https://mp.weixin.qq.com/s/SSYXT7gAnHCWU2-Nx7sKjg
```

## iOS 系统，一键更改 App 版本，完全免费

```
https://mp.weixin.qq.com/s/fue2BvyCnxJwiyDYyYp1oQ
```

## 新的 iOS 16.5 越狱发布，支持这款机型

```
https://mp.weixin.qq.com/s/4yjcOhCeI-tNpSR7TEDSZQ
```

## iOS 15.0 - 15.7.6 越狱已发布，不用电脑

```
https://mp.weixin.qq.com/s/0MkrVtac2s2Yi2I2sOx3nQ
```

## iOS 系统，动画加速，飞速体验，免费

```
https://mp.weixin.qq.com/s/mZg28Cu6X_VUo_2ABSmk1w
```

## 好东西！iOS 16.5.1 新的分屏插件

```
https://mp.weixin.qq.com/s/Xu3zfZwp9lIYtxvcPA6NJQ
```

## iOS 系统，全新触摸，中文，免费

```
https://mp.weixin.qq.com/s/ligvymOcyvnOy-TYy_yVpg
```

## iOS 系统，Touch-Viz，漂亮，内有福利

```
https://mp.weixin.qq.com/s/A30VyA5M-RwlPG8VpdYkGQ
```

## iOS 系统，必备神器，功能丰富

```
https://mp.weixin.qq.com/s/L_-UQ4Eds90dZObqTh0WoQ
```

## iOS 系统，增强版，中文，免费

```
https://mp.weixin.qq.com/s/9H1SDdrU5BUZiCh_pvmLLQ
```

## iOS 系统，韦尔西纳，测试一下

```
https://mp.weixin.qq.com/s/ETJQ4RAEimW_yoO677vlTQ
```

## iOS 系统，CopyVault，好用

```
https://mp.weixin.qq.com/s/1e1zT3l82OpV787B4DCQ2g
```

## iOS 系统，iPhone 熄屏显示，新鲜

```
https://mp.weixin.qq.com/s/nRNoXhFtXZp7JUaKO52Brg
```

## 在 iPhone 上养一只可爱的小猫，有趣

```
https://mp.weixin.qq.com/s/yAwYNtCUej1RmVMI78Tj5Q
```

## iOS 系统，键盘增强，完全免费

```
https://mp.weixin.qq.com/s/Fh4nLBx5xXG7qBADVCoOKw
```

## iOS 系统，一键清除后台，两款，完全免费

```
https://mp.weixin.qq.com/s/buF4WXfLffbQXRBgwIEQ2w
```

## iOS 系统，iPhone 熄屏显示，炫酷

```
https://mp.weixin.qq.com/s/a52lAZOAU_0eUZussSil4A
```

## iOS 系统，轻量级剪贴板神器，完全免费

```
https://mp.weixin.qq.com/s/WZ6zOGvSO5JvCBxh1tGqEQ
```

## iOS 系统，iPhone 分屏神器，现已免费

```
https://mp.weixin.qq.com/s/3aokPB-iMBQ-18QlBzLksg
```

## iOS 系统，一键隐藏，保护隐私

```
https://mp.weixin.qq.com/s/djagf9zvDL2c0e--wh1sZg
```

## iOS 系统，照片加锁，保护隐私，现已免费

```
https://mp.weixin.qq.com/s/9eSKpU4UcaC8MTn-RhyAlA
```

## iOS 系统，卡片计算器，前端悬浮显示

```
https://mp.weixin.qq.com/s/i_N7eFDM1f-uZIqZpOSfEg
```

## iOS 系统，像 iPad 一样分屏，终于来了，免费

```
https://mp.weixin.qq.com/s/gew66tNDWV0fQXxKBb-vmg
```


## iOS 系统，电池、亮度、音量百分比，清除角标，快速控制中心，完全免费

```
https://mp.weixin.qq.com/s/tXfKj3_HgZB4frYFQ4dPww
```

## iOS 系统，截图神器，详细使用

```
https://mp.weixin.qq.com/s/F8_flUzRjaQhyUg6fT9tNQ
```

## iOS 系统，真正的 iPhone 通话录音，就这么简单

```
https://mp.weixin.qq.com/s/WUWIdWcWB1lwKMfvUr4CtQ
```

## iOS 系统，老设备立刻拥有灵动岛，来了

```
https://mp.weixin.qq.com/s/I1kZRanmJ-AJ0XAvlytSTg
```

## iOS 系统，弹窗自适应，独立设置，录像暂停，Dock 动图，完全免费

```
https://mp.weixin.qq.com/s/6JJ4kDy4r459QK3HLXyG2Q
```

## iOS 系统，真后台来了，完全免费

```
https://mp.weixin.qq.com/s/_O6ZMCZXneX56QzBosreug
```

## iOS 系统，终于来了，分词大爆炸，完全免费

```
https://mp.weixin.qq.com/s/dPqO2AgnIMbOFd7Bi46vOw
```

## iOS 系统，终于支持了，自定义桌面布局，完全免费

```
https://mp.weixin.qq.com/s/1X16PvIXr7I3sUCp4VhR7w
```
## iOS 系统，我的 iPhone 下雪啦，清凉

```
https://mp.weixin.qq.com/s/RvrDbrBdhT-Q_ut_ky-ZoQ
```

## iOS 系统，视频播放器手势，完全免费

```
https://mp.weixin.qq.com/s/MjA6oIQlPu54BJ7kaAvjIw
```

## iOS 系统，iPhone 来电自动应答，语音信箱

```
https://mp.weixin.qq.com/s/_kD65DEKCcCGtFh6WorJZw
```

## iOS 系统，给隐藏照片加锁，保护隐私

```
https://mp.weixin.qq.com/s/eDvNKbSDuvAuO8s3nhyrzQ
```

## iOS 系统，分屏神器，终于来了，完全免费

```
https://mp.weixin.qq.com/s/VxrfG_IMyJkOYxY9s7e5zA
```


## iOS 系统，接听振动，角标美化，状态栏日期和时间，完全免费

```
https://mp.weixin.qq.com/s/OUj0C7_-ssvjkJMXeTXPdQ
```

## iOS 16.2 AppSync 插件更新，可以任意安装IPA

```
https://mp.weixin.qq.com/s/szIA7ASYdH0VWrRjeQV7xw
```

## iOS 系统，让 iPhone 使用 tvOS 精美弹窗

```
https://mp.weixin.qq.com/s/RXv3JAiom4K1rnK3W_U8MA
```

## iOS 系统，漂亮锁屏，别具一格

```
https://mp.weixin.qq.com/s/lewysejGGbpwA87h0TTG5w
```
## iOS 系统，功能丰富，必备神器，完全免费

```
https://mp.weixin.qq.com/s/p0nLRsMwnku0oxJen3XprA
```

## 05.05 ｜ 近期插件更新，速看

```
https://mp.weixin.qq.com/s/AwNF8CjL2HmxH_dQ5-RJ7A
```

## 超爽！iOS 15.1 MilkyWay2 分屏，含安装教程

```
https://mp.weixin.qq.com/s/5dg5MCJ6QgVZnIU-4CL4Gg
```

## IOS11.2~11.3.1系统美化软件已发布，你会喜欢吗？

```
https://mp.weixin.qq.com/s/FabYhz1LNwgic41A13kQdA
```

## iOS 系统，deb 备份，支持一键备份全部

```
https://mp.weixin.qq.com/s/r6KxWQsuSBh72w6b4X21Sg
```

## iOS 系统，通知归纳，简洁美观

```
https://mp.weixin.qq.com/s/Jc7ou0mo4yB5_SMiXOw6KA
```

## iOS 系统，一键振动，禁用资源库，自定义运营商，自动解锁，完全免费

```
https://mp.weixin.qq.com/s/PbTZM37HmOShWP7QS7_B9g
```

## iOS 系统，漂亮的计时器和闹钟，现已免费

```
https://mp.weixin.qq.com/s/oyFHcmKanTsznQho6UNMfQ
```

## iOS 系统，iPhone 通话变声器

```
https://mp.weixin.qq.com/s/yFlqh8mhRNi2QWZ1F40X1Q
```

## iOS 系统，动画加速，纵享丝滑

```
https://mp.weixin.qq.com/s/j7Mr4tD4e_wjbyCUWg8KMQ
```

## iOS 系统，快速启动神器，丝滑

```
https://mp.weixin.qq.com/s/9HmfhMYfq_0KNNl1ct2t9A
```

## iOS 系统，超酷动态视频壁纸，完全免费

```
https://mp.weixin.qq.com/s/LdtVlkrCD7-NBBQ7qW-IsA
```

## iOS 系统，运行时间，一键截图，锁屏天气，完全免费

```
https://mp.weixin.qq.com/s/5P8bmTSbnkChQF3F3uJePQ
```

## iOS 系统，功能强大，保护隐私

```
https://mp.weixin.qq.com/s/I8yXnoW5gY0908uOUgFVYw
```

## iOS 系统，简洁好用的程序锁，免费

```
https://mp.weixin.qq.com/s/7atDcp31oCJZZVDgnhFtBg
```

## iOS 系统，去除红点，去除资源库，录屏勿扰，复制通知，图标文件夹

```
https://mp.weixin.qq.com/s/KecTqxZ-Unabb6ARu3toBQ
```

## iOS 系统，让 iPhone 快速返回，非常方便

```
https://mp.weixin.qq.com/s/m-UVmt0uggHyD7I6_8TYxA
```

## iOS 系统，已安装插件，提取和备份 deb

```
https://mp.weixin.qq.com/s/vQpyIPIyGY80DOJ2KOe1fg
```

## iOS 系统，设置归类，设置顶图，就这么简单

```
https://mp.weixin.qq.com/s/7q4zS2mpFX2DRGxrjwqbaA
```

## iOS 系统，炫酷跑马灯，完全免费

```
https://mp.weixin.qq.com/s/xpwCCukt4O5VWP3cnXBixA
```

## iOS 系统，自定义文件夹，支持批量拖放，中文菜单

```
https://mp.weixin.qq.com/s/_4eThM51DYngTEjxcsKRUA
```

## iOS 系统，控制中心一键定位，一键注销，迷你播放器，免费

```
https://mp.weixin.qq.com/s/U9ahH9U06lYYYa3yMv8xEg
```

## iOS 系统，炫酷翻页特效，重生

```
https://mp.weixin.qq.com/s/WGF1iK9QmUJQRgVDoUqLyg
```

## 电池百分比，iOS 16 样式，完全免费

```
https://mp.weixin.qq.com/s/LoeQYWJCKRAov-DXZ7wLHA
```

## 仿 iPhone 12 充电动画来了，旧款设备也能实现

```
https://mp.weixin.qq.com/s/qQpviEqnfhXfL-1tyBneqQ
```

## iOS 13 仿 14 插件，越狱设备可试

```
https://mp.weixin.qq.com/s/gyoD4gOr3Iwn6vnOgHreIQ
```

## 收藏啦！iOS15.1XinaA15越狱，PC访问系统文件

```
https://mp.weixin.qq.com/s/FsM3Ul-3KaDunLQF__xQ8Q
```

## 真的来了，iOS 15/16 灵动半岛插件，确实有灵动

```
https://mp.weixin.qq.com/s/4S3-4qV4SopGeTwA7lCCpw
```

## 新工具疑似完美越狱 / 阿里云盘扩容 / 扫黑风暴惨遭泄漏

```
https://mp.weixin.qq.com/s/GwVnPIfZoeZVIRS0IKkufg
```

## iOS14.8.1系统越狱新解锁，戴耳机也能解锁设备

```
https://mp.weixin.qq.com/s/rTLe1KdfRQiZ54SaH4G8Hw
```

## iOS 系统，一键抠图神器，无需升级 iOS 16

```
https://mp.weixin.qq.com/s/VadXLLO9iBKMnUN507VHxQ
```

## iOS 系统，带上皇冠，终于来了

```
https://mp.weixin.qq.com/s/jUTM2lVhOSLWBdr1HiMQQQ
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

## iOS 13.4.1 免越狱改X手势，Sileo 再次更新

```
https://mp.weixin.qq.com/s/qYrfrFd-kE5hRtr0thIdUg
```

## iOS 系统，主题美化，真主题，抢鲜使用，无需越狱，免费

```
https://mp.weixin.qq.com/s/eDl_Og4I-nFotAgNwp2AnA
```

## iOS 系统，控制中心换个图标，有趣

```
https://mp.weixin.qq.com/s/nKzvEM2fIscdp0Wdlqprug
```

## iOS 系统，iPhone 水特效，清凉

```
https://mp.weixin.qq.com/s/_ctkLwa00Vuh-IPbppx7DA
```

## 近期插件更新，看看

```
https://mp.weixin.qq.com/s/k81Vg0MjJJKnvfdQ9h9JUw
```

## iOS 系统，自定义 Dock 栏摆放数量

```
https://mp.weixin.qq.com/s/h3b5_npq65eCjks5y5LfOw
```

## iOS 系统，翻译神器，详细使用

```
https://mp.weixin.qq.com/s/qZ0Gj64bHNqfNmZpocSyBQ
```

## iOS 15.4.1 越狱，Dopamine 将支持简体中文

```
https://mp.weixin.qq.com/s/8rVGhvCy3jwpN_c3Hiwksg
```

## iOS 15.4.1 Derootifier 更新，转无根插件

```
https://mp.weixin.qq.com/s/WpSy6G7vIYFWS0PmbhDXgQ
```

## iOS 系统，新的 iPhone 分屏神器，众享丝滑

```
https://mp.weixin.qq.com/s/MeqS0proFrb5CHZ1LrNWAw
```

## iOS 系统，自定义锁头，漂亮

```
https://mp.weixin.qq.com/s/CWGHr0mO5YOcHvcPP0-DgA
```

## iOS 系统，新一代截图神器，来了

```
https://mp.weixin.qq.com/s/DZZxAcw8lLvczs-8rUcBMQ
```

## iOS 系统，iPhone 熄屏显示，精美

```
https://mp.weixin.qq.com/s/QQgXWmQiGO5kCLsMkH1plw
```

## iOS 系统，计算历史，角标，去除，勿扰增强，插件 Hub，完全免费

```
https://mp.weixin.qq.com/s/8HUs0mvFWZ5-ktek1YrxmA
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



## iOS 系统，通知上岛，终于来了，免费

```
https://mp.weixin.qq.com/s/hmhS82ldAPGz13SrvdDXlg
```

## iOS 系统，新版，通知上岛，免费

```
https://mp.weixin.qq.com/s/LKNRCtao1hJ904z7GBd3RA
```

## iOS 系统，网络管理，重生

```
https://mp.weixin.qq.com/s/IJAs_AWQGgFUllICKsMkMA
```

## 无需更新系统，iOS 16 锁屏音乐播放器

```
https://mp.weixin.qq.com/s/Qgg-YsbZ2qxRByP3BvLOwQ
```

## iOS 系统，巫师，神奇切换器

```
https://mp.weixin.qq.com/s/5bDN0I2yoVoEE3z_fNzEhw
https://mp.weixin.qq.com/s/kpO4zbZx_jf-eaAIwnTUwQ
```

## iOS 系统，新版 OCD，Dotto 重生

```
https://mp.weixin.qq.com/s/TgPBUlJ6hA6hB47LKbFYXA
```

## iOS 系统，经典，复活

```
https://mp.weixin.qq.com/s/jPYZSmac9A982G47oog4Bw
```

## iOS 系统，保护你的 iPhone

```
https://mp.weixin.qq.com/s/mPAYZHpTyowD6-cgkm-DrA
```

## iOS 系统，新玩具，泡泡应用

```
https://mp.weixin.qq.com/s/DPUNePAuUqr7wc15YCRyOA
```

## iOS 系统，IAmLazy 一键备份，就这么简单

```
https://mp.weixin.qq.com/s/9Fozz0htLjFJIMzZKalnuA
```

## iOS 系统，让 iPhone 续航更持久

```
https://mp.weixin.qq.com/s/qgDeqCa4bmIZO_Mb42ylZA
```

## iOS 系统，KuKu，重生

```
https://mp.weixin.qq.com/s/P-Tq4LH5w_RHe6ulSY3BAQ
```

## 强，分屏神器随意切换

```
https://mp.weixin.qq.com/s/fu4GwHb22wV9wTugeTS4kQ
```

## 大神被诏安，苹果给的实在是太多了

```
https://mp.weixin.qq.com/s/8aXpTlDo9o924uG1_4otkw
```

## iOS 15.x SaiGon 越狱更新，离有根接近

```
https://mp.weixin.qq.com/s/nMf9dsdkYUcc74h7sAsgZA
```

## iOS 系统，专业歌词，卡拉OK，中文免费

```
https://mp.weixin.qq.com/s/U4oQJVB-dOhNTfo2oCLooA
```

## iOS 系统，浏览器手势，便捷神器

```
https://mp.weixin.qq.com/s/dyslnxm8WUQlaYXhbnlbAQ
```

## iOS 系统，增强神器，终于来了，好用

```
https://mp.weixin.qq.com/s/TZegOgeQQ4yBz147n6iD-g
```

## iOS 系统，灵动通知，现已发布，丝滑

```
https://mp.weixin.qq.com/s/3_rDkLlOd4yoZfKYvDLyqw
```

## iOS 系统，智能旋转，好用

```
https://mp.weixin.qq.com/s/YZalAF-psZc05IksHbzevg
```

## iOS 系统，新玩意，Ring

```
https://mp.weixin.qq.com/s/N18_xeLW6ERG-8qTqEQyTw
```

## iOS 系统，彩虹信号

```
https://mp.weixin.qq.com/s/HQOalFFFihh2bsHCX92g-Q
```

## iOS 系统，一键清除后台，免费

```
https://mp.weixin.qq.com/s/cmhK9IgrbsYoknVPqoa-qA
```

## iOS 系统，智能通知，终于来了

```
https://mp.weixin.qq.com/s/skVXzhdfBzOheA16PJNSbA
```

## iOS 系统，酷CC，完全免费

```
https://mp.weixin.qq.com/s/0IVd_KbPOxc8HugXw4c1zw
```

## iOS 系统，通知聚合，漂亮，重生

```
https://mp.weixin.qq.com/s/MG4ilT2szAIuCx09xFrX0Q
```

## iOS 系统，通知归纳，复活

```
https://mp.weixin.qq.com/s/FAlH-OJY9pqVQE69HkM_iw
```

## iOS 系统，超强山猫，终极定制

```
https://mp.weixin.qq.com/s/x6o1CxrpmxZS_AKw6twWeQ
```

## iOS 系统，老牌分屏神器，复活，免费

```
https://mp.weixin.qq.com/s/KPnCZGIzGjwz4yzvyRm1AA
```

## iOS 系统，一键单独静音 App，快速方便

```
https://mp.weixin.qq.com/s/83NwvHGvrPnat2JfScHypQ
```

## iOS 系统，新玩意，寿司

```
https://mp.weixin.qq.com/s/FlIRiQsf7GMNSCipOX_Mzw
```

## iOS 系统，简单时间，效果不错

```
https://mp.weixin.qq.com/s/gmE_tbMuwVhOlIMUQfBH0A
```

## iOS 15 系统，Freya 越狱发布，手机端，无需电脑

```
https://mp.weixin.qq.com/s/lO4QUi5t0QOayFKEnJ_e0w
```

## iOS 系统，长蛇座，菜单 App 快捷

```
https://mp.weixin.qq.com/s/iumYTvAg59zTfw9CZ_OfWQ
```

## iOS 15 系统，新越狱工具发布

```
https://mp.weixin.qq.com/s/OnXArgrCST81xFdLL2HHhQ
```

## iOS 系统，Vē，通知记录，全部

```
https://mp.weixin.qq.com/s/2w9XBSYPo7iplrDIhacjZg
```

## iOS 系统，砸壳，完整权限

```
https://mp.weixin.qq.com/s/QazBuU0vS1h1xvAgbpdiwA
```

## iOS 16.6.1 TrollPad 后台分屏已发布

```
https://mp.weixin.qq.com/s/TnhjRLBJb07IqqFLq2bdZg
```

## iOS 系统，App 堆栈，终于来了

```
https://mp.weixin.qq.com/s/zSZTNRIGDGVfwYiXjkHmLQ
```

## iOS 系统，全局替换，神器？

```
https://mp.weixin.qq.com/s/AzWKXwpXyk5r-1UJewEcRw
```

## 更新了！iOS 16.x LCT 通话翻译，不怕听不懂

```
https://mp.weixin.qq.com/s/O0jYtJevqlvDD7ZV7CW64w
```

## iOS 系统，一键长截图，终于来了，独立使用，完全免费

```
https://mp.weixin.qq.com/s/pm4OONErIvjqIuVUw6-V6w
```

## iOS 系统，Mitsuha 现已复活

```
https://mp.weixin.qq.com/s/4W--zjn9KhVyuDFuMsiL8Q
```

## 无须刷机，破解iPhone访问限制密码方法

```
https://mp.weixin.qq.com/s/buJ1b3PzLmxn3-_ZIq51AA
```

## iOS 系统，全新控制中心，终于来了

```
https://mp.weixin.qq.com/s/ZB-RdZmZBbKp4LcCKC9Tow
```

## iOS 系统，IPA 下载器，完全免费

```
https://mp.weixin.qq.com/s/i0ViSJ2e8zbZ2ib4mpguwg
```

## iOS 系统，老设备一键灵动岛，完全免费

```
https://mp.weixin.qq.com/s/P8kuxCVVbko6SPegTH512g
```

## iOS 系统，Mochi15 锁屏音乐，已经免费

```
https://mp.weixin.qq.com/s/wbFuW40JrKVuINk0R0v75g
```

## iOS 系统，智能网络，增强 WiFi 信号

```
https://mp.weixin.qq.com/s/N55JDSXC_7CusZYzjcpebg
```

## iOS 系统，锁大师，现已上架 Havoc 源，免费

```
https://mp.weixin.qq.com/s/QOwYPFBHEgCgU9jxyK_FNw
```

## iOS 系统，新的精美控制中心，必看

```
https://mp.weixin.qq.com/s/gcsH0URHsh4z9awUIliP3Q
```

## iOS 16.6.1 新快捷插件，超级便捷

```
https://mp.weixin.qq.com/s/-tvVsysm4-IcfS908bZD2g
```

## XinaA15 新官网上线，请收藏

```
https://mp.weixin.qq.com/s/0MRRIOghnihTA-TSmv7s5g
```

## iOS 系统，伴着音乐入睡

```
https://mp.weixin.qq.com/s/beSu7v8Q_xXDY-yqHsYdGA
```

## iOS 17 系统，也能越狱？

```
https://mp.weixin.qq.com/s/B2EE4Tq6lFYmQH1koJucCQ
```

## 大神去世！

```
https://mp.weixin.qq.com/s/9eEgZCNwp2l2jGbixECdCA
```

## iOS 系统，增加小组件，音量控件，触摸轨迹，V指示器

```
https://mp.weixin.qq.com/s/QEJev2pngW595b2YalJZcw
```

## iOS 系统，让控制中心更好用

```
https://mp.weixin.qq.com/s/2qGFFYJTfQFO-3WhfWOv3w
```

## iOS 系统，电源增强，完全免费

```
https://mp.weixin.qq.com/s/d3rQlXgBi6yUHLYaR7dFxQ
```

## iOS 系统，下载 deb，提取 dylib 文件

```
https://mp.weixin.qq.com/s/bNU5OMIzpYcxWKz-plXy4Q
```

## 来啦！全新 iOS 16.6.1 启动灵动岛，可以下移显示

```
https://mp.weixin.qq.com/s/QtIyEdHQs0xaU-lwFzz2Gg
```

## iOS 16.6.1 颠倒显示，实现无刘海显示

```
https://mp.weixin.qq.com/s/wOdmdHV-c7dTBMvfafbCDw
```

## 大神更新，老牌神级，现已支持

```
https://mp.weixin.qq.com/s/HWtxgeob9SiuF-kiEeZwxA
```

## 无需升级 iOS 18，体验新功能，第一期

```
https://mp.weixin.qq.com/s/mNjjfCJT8i5-frfvxtw_JQ
```

## iOS 系统，精美控制中心，完全免费

```
https://mp.weixin.qq.com/s/6u4ijH_kdJObZ6D96kSPBg
```

## iOS 系统，继续，控制

```
https://mp.weixin.qq.com/s/7yPvxAXQo64eqb-ytT5HEw
```

## iOS 系统，角标，个性化角标

```
https://mp.weixin.qq.com/s/RbNw_DeCROx6nNFGLKVJdw
```

## iOS 系统，一键换号，来电拒接，取消静音，计算历史，备份安装包

```
https://mp.weixin.qq.com/s/B9VxGRXX_o803jwBnNJAlA
```

## iOS 系统，锁屏秒数，评论评分，状态栏网速，同时播放，自由深色

```
https://mp.weixin.qq.com/s/XcaRhBeFUiQloLsYMNxpjA
```


## 无需升级 iOS 18，体验新功能，第二期

```
https://mp.weixin.qq.com/s/4bmOb5UjTuPKAD4gHAtIWg
```

## 惊现！iOS 完美越狱已发布，仅支持这些系统

```
https://mp.weixin.qq.com/s/InUtCLs6zDVy3XJs2Vwp7A
```

## 系统数据，查看详细

```
https://mp.weixin.qq.com/s/4V9v4fkKp4-QdfSeK5HwBQ
```

## iOS 系统，语音免跳转，移除小组件背景，迷你横幅，环境光切换，独立设置

```
https://mp.weixin.qq.com/s/PPxn4cm_e9mZ_ZMSyJI1kQ
```

## 一键透明

```
https://mp.weixin.qq.com/s/h9k298Yf7XwYg5BkbkLkmQ
```

## 图标随意摆，终于来了

```
https://mp.weixin.qq.com/s/NqAHwQ3osTT554lkkOzehA
```

## 新玩法，无需升级 iOS 18 系统

```
https://mp.weixin.qq.com/s/4jc7XqYDaWjygjnrGHkqfw
```

## CC18

```
https://mp.weixin.qq.com/s/2IoMgsPUtw_KEWPBgpqokQ
```

## 小草，终于重生

```
https://mp.weixin.qq.com/s/AZXHdcDyIkyscLOZGZgw0Q
```

## 时光机

```
https://mp.weixin.qq.com/s/lzNyegaJs1c-2XMSWdQ5Ow
```

## 移除电量低于20%弹窗

```
https://mp.weixin.qq.com/s/-_4KYfb7cD92M-6Z3D7ATA
```

## 实用，免费！

```
https://mp.weixin.qq.com/s/2N1p-zeb8-to2OfDgaXbzQ
```

## 一大波！

```
https://mp.weixin.qq.com/s/6TOE5mzlksU2L464TekALA
```

## 速度，无需升级 iOS 18.1 系统

```
https://mp.weixin.qq.com/s/x13r70Cw5dNmfYhjxX5m6w
```

## CCBorder

```
https://mp.weixin.qq.com/s/b75d8mDoTN8SM37qqQ5z-w
```

## 神器，永生者！

```
https://mp.weixin.qq.com/s/4CXLdgvM6kUNeZqXLPYmnA
```

## iOS 14.6 - 14.8 系统越狱来了，你需要知道这些

```
https://mp.weixin.qq.com/s/zf3cJqinVIE4G8w7lJQLXQ
```

## unc0ver 更新 7.0.1 版本，真的是完美越狱吗？

```
https://mp.weixin.qq.com/s/dGvP2l4JwwkAMj-kevzoNw
```

## 王者荣耀修改教程

```
https://mp.weixin.qq.com/s/kawj3yg4lklM7BGrM4lXew
https://mp.weixin.qq.com/s/5g0pjMRXkZc0g1hvs9oqoQ
https://mp.weixin.qq.com/s/kAwToNnMArbyG9hk-nzalg
https://mp.weixin.qq.com/s/trKrmzkOg8qUx9LVGu3WCg
https://mp.weixin.qq.com/s/21gSod8io0L4ygYIK2C0vg
https://mp.weixin.qq.com/mp/appmsgalbum?action=getalbum&__biz=MzA5OTU0MTE1MQ==&scene=1&album_id=1318325233564172288#wechat_redirect
```

## 分享插件：55款插件一定有你喜欢的

```
https://mp.weixin.qq.com/s/VoZGTl5iwe98tlzrKThEfg
```

