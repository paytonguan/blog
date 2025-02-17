---
title: iOS伪越狱玩法
categories: iOS
abbrlink: iOS-Fake-Jaibreak-Usage
date: 2023-11-10 13:18:29
tags:

---

![](topic.jpg)

Trollstore等。

<!-- more -->

# TODOS

```
# schemeshare
https://mp.weixin.qq.com/s/xe25A0horQpOzNykY494Rw

# TrollStore 小技巧二：解决删除不干净
https://mp.weixin.qq.com/s/cn_C33xMiyWcCt-jJa0Ejg

# 国行开启Apple Intelligence
https://mp.weixin.qq.com/s/PiU95w6Ic1kNKnfWN1Jtfw




FilzaEscaped修改X手势

1.打开FilzaEscaped 
2.找路径：
/var/containers/Shared/SystemGroup/systemgroup.com.apple.mobilegestaltcache/Library/Caches/com.apple.MobileGestalt.plist 
3.打开com.apple.MobileGestalt.plist
4.选择CacheExtra-再找oPeik/9e8lQWMszEjbPzng 
5.ArtworkDeviceSubType 点击右侧（i） 
6.注意这个值一定要记得，预防你想恢复原来的样子，想恢复改为原来值就行。 
7.值修改为：2436
8.点击右上角存储，打开设置-显示与亮度-视图-改为放大，再改为标准即可实现X手势。





PlankFilza修改为iPhone X手势

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




PlankFilza修改运营商名称

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






PlankFilza修改关于本机信息

这个就是可以修改关于本机型号及版本号等，直接找路径：
/var/containers/Shared/SystemGroup/systemgroup.com.apple.mobilegestaltcache/Library/Caches/com.apple.MobileGestalt.plist
然后看看，你想修改什么内容，右上角保存，然后重启手机即可修改成功。
```

# 安装与使用

## Trollstore

TrollStore可以在部分系统上利用漏洞，不越狱即可安装永不过期的IPA。利用该漏洞，可对系统Var目录进行读写。

打开以下链接并按照步骤操作即可。

```
https://github.com/opa334/TrollStore
```

安装后需要安装ldid以允许安装未签名的应用。建议安装持久性助手，以避免重建图标缓存时TrollStore无法打开。如果安装时出错，可翻墙后重试。

完成后，将需要安装的IPA分享到TrollStore即可安装。

可使用以下项目对Trollstore进行汉化。

```
https://github.com/sbwml/TrollStore_zh_Hans
```

### 操作技巧

#### 屏蔽系统更新

用Filza打开/usr/bin/vm_stat，单击文件运行，键盘切换为英文，长按空白界面粘贴以下指令并运行即可。

```
# 禁止更新
rm -rf /var/MobileSoftwareUpdate/MobileAsset/AssetsV2/* && chflags schg,schange,simmutable /var/MobileSoftwareUpdate/MobileAsset/AssetsV2

# 恢复更新
chflags noschg,noschange,nosimmutable /var/MobileSoftwareUpdate/MobileAsset/AssetsV2
```

#### 清理手机占用空间

可用Filza清空`/var/db/diagnostics/Persist/`和`/var/db/diagnostics/Special/`目录。

注意TrollStore无法安装iCleaner，因无法获取到iCleaner所需要的权限。

### 轻松签+

与TrollStore功能类似，但可以自定义IPA。利用TrollStore的漏洞可以安装永不掉签的轻松签版本，也可以选择已有的企业证书自签安装。

```
https://esign.yyyue.xyz/
```

通过修改标识符，可实现双开。通过移除库，可能可以去除广告弹窗，注意修改完后选择`仅修改配置，不签名`。

### TrollFools

可往App注入插件。

#### 可用插件

```
# 插件库
https://www.123pan.com/s/vgn0Vv-ZTaFv.html
https://pan.quark.cn/s/4d23a616c1b2

# Lucky Speeder，游戏或广告加速
https://github.com/kekeimiku/LuckySpeeder

# DisableAccelerometer，禁用加速度传感器
https://github.com/DevelopCubeLab/DisableAccelerometer

# ShakeShield，屏蔽加速度计
https://github.com/huami1314/ShakeShield
```

## MacDirtyCow

MacDirtyCow利用CVE-2022-46689漏洞，支持iOS 14.0-15.7.1，16.0-16.1.2，建议15.0及以上系统使用。

利用该漏洞，可通过交换同等大小文件的方式覆盖文件实现，从而实现一些特权操作。注意覆盖的文件不能大于原文件，覆盖后会一直存在并占用内存，重启后得到释放。当占用内存过大时，可能会导致手机变卡、小组件无法载入、第三方输入法无法使用、手机自动注销等。

MacDirtyCowDemo仓库如下。直接使用利用MacDirtyCowDemo的App即可。

```
https://github.com/zhuowei/MacDirtyCowDemo
```
### 操作技巧

#### Altstore突破三个签名限制

更新至1.6.1后，在Settings界面的CREDITS上面三指向上划出开关，安装MacDirtyCow突破后即可签名第四个应用程序。

## KFD

该漏洞可用于iOS 15.0-15.7.6和iOS16.0-16.5&16.6b1。直接使用利用KFD的App即可。

# 插件商店

可用插件商店如下。

```
# Misaka
https://github.com/straight-tamago/misaka

# SimpleKFD
https://github.com/Lrdsnow/SimpleKFD

# PureKFD
https://github.com/Lrdsnow/PureKFD
```

## 可用Repo

```
# Picasso源
https://raw.githubusercontent.com/Lrdsnow/lrdsnows-repo/main/PureKFD/manifest.json
https://raw.githubusercontent.com/circularsprojects/circles-repo/main/manifest.json
https://raw.githubusercontent.com/sourcelocation/Picasso-test-repo/main/manifest.json
https://bomberfish.ca/PicassoRepos/Essentials/manifest.json

# Misaka源
https://raw.githubusercontent.com/shimajiron/Misaka_Network/main/repo.json
https://raw.githubusercontent.com/34306/iPA/main/repo.json
https://raw.githubusercontent.com/roeegh/Puck/main/repo.json
https://raw.githubusercontent.com/huligang/coolwcat/main/repo.json
https://yangjiii.tech/file/Repo/repo.json
https://raw.githubusercontent.com/leminlimez/leminrepo/main/repo.json
https://raw.githubusercontent.com/ichitaso/misaka/main/repo.json
https://raw.githubusercontent.com/dobabaophuc1706/misakarepo/main/Repo/repo.json
https://tweakrain.github.io/repos/misaka/misaka.json
https://www.iwishkem.tk/misaka.json
https://raw.githubusercontent.com/EPOS05/MisakaRepoEPOS/main/repo.json
https://raw.githubusercontent.com/tdquang266/MDC/main/repo.json
https://raw.githubusercontent.com/kloytofyexploiter/Misaka-repo_MRX/main/repo.json
https://raw.githubusercontent.com/HackZy01/misio/main/repo.json
https://raw.githubusercontent.com/p0s3id0n86/misakarepo/main/Repo/repo.json
https://raw.githubusercontent.com/tyler10290/MisakaRepoBackup/main/repo.json
https://gist.githubusercontent.com/c22dev/af8dd3a760330eb31da5f8751af1b487/raw/6eb744fabc6eb0eb3352ce41c9a08ce5c38c4e6a/index.json
https://dobabaophuc1706.github.io/repo/
https://puck.roeegh.com/repo.json
https://www.appfair.net/fairapps-ios.json
https://repo.starfiles.co/
https://raw.githubusercontent.com/swaggyP36000/TrollStore-IPAs/main/apps.json
```

# Cowabunga/Cluckabunga

超强工具箱，下载链接如下。

```
https://cowabun.ga/

# Cowabunga
https://github.com/leminlimez/Cowabunga
# Cluckabunga
https://github.com/leminlimez/Cluckabunga
```

## 自定义锁头

选择工具-锁定，点击右上角导入锁头素材文件夹，选择素材即可。锁头素材下载链接如下。

```
https://www.123pan.com/s/vgn0Vv-xl3Fv.html
```

## 自定义运行模式

该漏洞可实现交换同等大小的文件，通过自定义运行模式可自定义需要交换的系统文件。

点击工具-自定义运行模式，选择+号创建操作即可。创建操作完成后注销即可应用效果。

### 自定义蜂窝网络4G/5G显示

创建操作，类型选择plist，路径为Localizable.strings的路径，如下。

```
# iOS 15
/System/Library/PrivateFrameworks/UIKitCore.framework/zh_CN.lproj/Localizable.strings

# iOS 16
/System/Library/PrivateFrameworks/UIKitCore.framework/Localizable.loctable
```

plist类型选择binary，PLIST数据下点击添加属性，编辑新添加的属性。Key一栏输入`4G`，Value一栏输入需要自定义的内容，如`8G`。具体需要修改的项可直接用Filza查看该plist文件。同理也可将`5G`修改为`9G`等。

修改后文件体积会增大，点击`计算大小`会出现红色的叉。此时需要删除一些项，使文件体积小于或等于原文件。点击添加属性，编辑新添加的属性。点击String，弹出菜单，选择Deleting。参考文件内的项目，可删掉部分无用Key，如5GUC、5GUW、5GE等。删除完成后重新点击`计算大小`，出现绿色的勾时视为成功。

完成后注销手机即可应用操作。

### 自定义输入密码/滑动关机文字显示

步骤与上面的类似，需要修改的plist如下。

```
# iOS 15
/System/Library/PrivateFrameworks/SpringBoardUIServices.framework/zh_CN.lproj/SpringBoardUIServices.strings
```

可自定义的属性如下。

```
# 锁屏输入密码显示
PASSCODE_MESA_ENTRY_PROMPT

# 滑动关机显示
POWER_DOWN_LOCK_LABEL
```

可删除的属性如下。

```
PHONE_BT_AND_WIFI_OFF_PROMPT
```

### 自定义设置个人帐户文字显示

需要修改的plist如下。

```
# iOS 15
/System/Library/PrivateFrameworks/PreferencesUI.framework/zh_CN.lproj/Settings~iphone.strings
```

可自定义的属性如下。

```
# 设置个人帐户文字
CASTLE_SUBTITLE

# 设置顶部文字
Settings

# 设置飞行模式文字
Airplane Mode

# 设置通用项目文字
General
```

可删除的属性如下。

```
# 监督模式提示
SUPERVISION_IPHONE
SUPERVISION_IPOD
SUPERVISION_WITH_ORG_IPHONE
SUPERVISION_WITH_ORG_IPOD
```

### 自定义没有更早的通知文字显示

需要修改的plist如下。

```
# iOS 15
/System/Library/PrivateFrameworks/UserNotificationsUIKit.framework/zh_CN.lproj/Localizable.strings
```

可自定义的属性如下。

```
# 没有更早的通知文字
NO_NOTIFICATION_HISTORY
```

可删除的属性如下。

```
NOTIFICATION_OPTIONS_TURN_OFF_APPLICATION_DURING_MINDFULNESS
```

### 自定义电池健康最大容量值

```
/var/MobileSoftwareUpdate/Hardware/Battery/Library/Preferences/com.apple.batteryhealthdata.plist
```

需要修改的属性如下。不建议修改为80以下。

```
Maximum Capacity Percent
```

### 自定义锁屏手电筒和相机图标

创建操作，类型选择替换，路径如下。

```
/System/Library/PrivateFrameworks/CoverSheet.framework/Assets.car
```

替换数据一栏点击上传文件。文件下载链接如下。

```
# iOS 15
https://github.com/34306/iPA/releases/download/MacDirtyCow_iPAs/ios15.car

# iOS 16
https://github.com/34306/iPA/releases/download/MacDirtyCow_iPAs/ios16.car
```

car文件可通过以下软件编辑。

```
https://github.com/SerenaKit/Samra
```

### 自定义控制中心连接按钮

创建操作，类型选择替换，路径如下。

```
/System/Library/ControlCenter/Bundles/ConnectivityModule.bundle/Assets.car
```

替换数据一栏点击上传文件。文件下载链接如下。

```
https://github.com/34306/iPA/releases/download/MacDirtyCow_iPAs/CC.car
```

### 自定义时钟图标

与上述操作类似，替换路径如下。

```
/System/Library/PrivateFrameworks/SpringBoardHome.framework/Assets.car
```

文件下载链接如下。

```
# iOS 15/16均可用
# Anime
https://github.com/34306/iPA/releases/download/MacDirtyCow_iPAs/clock_anime.car

# checkra1n
https://github.com/34306/iPA/releases/download/MacDirtyCow_iPAs/clock_checkra1n.car
```

### 自定义通知横幅阴影

替换路径如下。

```
# 浅色模式
/System/Library/PrivateFrameworks/PlatterKit.framework/platterVibrantShadowLight.visualstyleset

# 深色模式
/System/Library/PrivateFrameworks/PlatterKit.framework/platterVibrantShadowDark.visualstyleset
```

用于替换的文件下载链接如下，下载Notification.Shadow.zip即可。

```
https://github.com/roeegh/Mise/releases
```

### 自定义系统默认英文键盘空格键

替换路径如下。

```
/System/Library/TextInput/TextInput_en.bundle/Keyboard-en.plist
```

用于替换的文件下载链接如下，下载Custom.Spacebar.zip即可。

```
https://github.com/roeegh/Mise/releases
```

### 自定义控制中心

替换路径如下。

```
/System/Library/PrivateFrameworks/ControlCenterUI.framework/DefaultModuleSettings~iphone.plist
```

用于替换的文件下载链接如下，下载Better.CC.zip即可。

```
https://github.com/roeegh/Mise/releases
```

也可手动定义各组件的大小。用Filza打开上面的DefaultModuleSettings~iphone.plist文件，复制该文件至可编辑的地方，如Documents。打开复制后的文件，点击希望修改的项，修改其Size项中的width和height即可。

```
# 音乐模块
com.apple.mediaremote.controlcenter.nowplaying

# 亮度模块
com.apple.control-center.DisplayModule

# 音量模块
com.apple.mediaremote.controlcenter.audio
```

修改完成后，将该文件作为替换文件即可。

### 自定义字体

需要替换的文件路径如下。

```
# 锁屏时钟字体
/System/Library/Fonts/CoreUI/SFUISoft.ttc

# 中文字体
/System/Library/Fonts/LanguageSupport/PingFang.ttc

# 英文字体
/System/Library/Fonts/CoreUI/SFUI.ttf

# 系统键盘字体
/System/Library/Fonts/CoreAddition/Keycaps.ttc  

# 系统拨号键盘字体
/System/Library/Fonts/CoreAddition/PhoneKeyCaps.ttf

# 表情字体
/System/Library/Fonts/CoreAddition/AppleColorEmoji-160px.ttc
```

# FileSwitcherPro

和Cowabunga的自定义运行模式基本相同，下载链接如下。

```
https://github.com/straight-tamago/FileSwitcherPro
```

打开后点击左上角设置，开启Replace Plist Auto Padding以在替换plist时自动调整大小。点击下面的+号，选择新建Folder，完成后进入文件夹，点击+号选择Bundle Item，可选择希望添加的项目，添加后开启，注销即可生效。

也可自行替换文件。创建Original Item，ACTION的Type选择Replace File即为替换文件。

## 更换App提示音

用Filza进入/var/containers/Bundle/Application/，然后找到需要替换修改提示音的App。以微信为例，为com.tencent.xin文件夹，进入后搜索in.caf，复制该文件路径。

打开FileSwitcherPro，点击右侧+号，选择Original Item选项，在TARGET中填入上述路径。在Type中选择Replace File，然后点击File Select，选中要用于覆盖的音频文件。点击Add，然后Apply和Restart即可。

## 更换App UI

同上，替换Assets.car即可。

# Santander

文件管理器。

```
https://github.com/mineek/SantanderMacDirtyCow
```

## 修改分辨率

可通过修改分辨率以在旧设备上实现灵动岛。打开Santander，点击右上角更多按钮，选择`Go to...`-`Other`，跳转到以下地址。

```
/var/containers/Shared/SystemGroup/systemgroup.com.apple.mobilegestaltcache/Library/Caches/com.apple.MobileGestalt.plist
```

点击 CacheExtra 进入，在搜索框搜索opeik，找到ArtworkDeviceSubType选项即为分辨率。记住当前分辨率，然后点击进入，修改为相应分辨率。

```
iPhone 14 Pro / 2556
iPhone 14 Pro Max / 2796
```

## 系统声音替换

目录如下。

```
/System/Library/Audio/UISounds/
```

长按对应文件并选择Replace即可。

```
# 键盘音频
Tock.caf

# 键盘音频
key_press_click.caf

# 锁屏音频
lock.caf
```

## 透明文件夹

路径如下。

```
/System/Library/PrivateFrameworks/SpringBoardHome.framework/
```

长按对应文件并选择Replace即可。

```
folderDark.materialrecipe
folderLight.materialrecipe
```

# 主题美化

可用APP如下。

```
# Mugunghwa
# 将主题用Filza放置于var/mobile/Themes/mugunghwa/Themes即可
# 主题图标的规律为，App标识符-large.png
https://github.com/s8ngyu/Mugunghwa/releases

# TrollTools
https://github.com/sourcelocation/TrollTools/releases
```

主题包可在以下链接下载。

```
# 普通
https://pan.baidu.com/s/1Ygj6RpzIFQYn0aGzGB5nFg?pwd=9nns（提取码 / 9nns）
https://www.123pan.com/s/vgn0Vv-5J3Fv
https://www.123pan.com/s/vgn0Vv-jEaFv.html
https://havoc.app/

# 锁屏密码美化
https://pan.baidu.com/s/1E4MDnPbJ9fdFx-kHqo68og?pwd=7wcu（提取码 / 7wcu）
https://www.123pan.com/s/vgn0Vv-gw3Fv
```

# 工具

## 通用工具

```
# FilzaEscaped，免越狱文件管理器，可用于iOS 15及以下系统
https://basvtdevelopments.com/filzaescaped

# PlankFilza，免越狱文件管理器
https://github.com/brandonplank/PlankFilza

# PostBox，非越狱手机查看插件库
https://www.postbox.news/

# Filza Deco
https://github.com/dobabaophuc1706/FilzaMod

# ControlConfig，控制中心添加模块
# 在App设定好模块后，在控制中心添加该模块即可
https://github.com/f1shy-dev/ControlConfig

# FileSwitcherX，工具箱
https://github.com/straight-tamago/FileSwitcherX

# 图标排序
https://github.com/Avangelista/Appabetical

# 字体更换
https://github.com/ginsudev/WDBFontOverwrite

# 修复企业证书失效
https://github.com/BomberFish/Whitelist
https://www.123pan.com/s/vgn0Vv-YO3Fv

# 移除自签三个App限制
https://github.com/zhuowei/WDBRemoveThreeAppLimit

# 自定义状态栏
https://github.com/Avangelista/StatusMagic

# 修改分辨率，iOS 16老设备激活灵动岛
https://github.com/matteozappia/DynamicCow

# 自定义分辨率
https://github.com/sourcelocation/ResSet16

# 隐藏主页条
https://github.com/straight-tamago/NoHomeBar

# Dock栏透明
https://github.com/straight-tamago/DockTransparent

# 隐藏主页条+Dock栏透明
https://github.com/leminlimez/DockHider

# 一键注销
# 将放大器添加到控制中心，然后打开一次App，就可以在控制中心一键注销
https://github.com/straight-tamago/RespringCC

# 状态栏显示秒数
https://mp.weixin.qq.com/s/GNzXfiTchse4KeAD7iHrxQ

# AppCommander，App备份
https://github.com/BomberFish/AppCommander

# DirtyCow，工具箱
https://github.com/mineek/dirtycowapp

# FileManager
https://github.com/mineek/FileManager

# CyMusic，音乐软件
https://github.com/gyc-12/music-player-master
# 音源
https://www.123684.com/s/vgn0Vv-a8MFv
```

## Trollstore专用

```
# 应用库
# 推荐
https://github.com/Peng1029/trollrepo
https://ipa.cypwn.xyz/
# 其它
https://ipa.store/
https://pan.ios98.com/
https://pan.qxnav.com/
https://www.123pan.com/s/vgn0Vv-Qo3Fv
https://pan.baidu.com/s/1f7qiHzFutQ3zN1-SQkFXSg?pwd=791g（提取码 / 791g）

# TrollSpeed，网速悬浮窗工具
https://github.com/Lessica/TrollSpeed

# TrollRecorder，通话录音工具
https://github.com/Lessica/TrollRecorder

# 爱啪思道，App Store辅助工具
https://github.com/Lakr233/Asspp

# DowngradeApp
https://pan.baidu.com/s/17YQgjtw2Mf2W36iTG63w6Q?pwd=fxpf（提取码 / fxpf）
https://share.weiyun.com/t0Lvr5ms

# AppStore++
# 原版包含广告，网盘包含去广告版本
https://github.com/CokePokes/AppStorePlus-TrollStore
https://www.123pan.com/s/vgn0Vv-9r3Fv

# deb转换为IPA
https://github.com/sourcelocation/DebToIPA/releases

# Unc0ver
# 若安装8.0.0-8.0.2版本，则还需要安装u0Launcher，安装完成后打开u0Launcher，它会自动跳转到unc0ver，正常越狱即可
https://github.com/opa334/u0Launcher/releases

# Filza，包含原版和无Url Scheme版本
# 部分App会通过检查Url Scheme查看是否安装Filza以判断当前是否越狱
# 此时可使用无Url Scheme版本
https://www.tigisoftware.com/default/?p=439

# Apps Manager
https://www.tigisoftware.com/default/?p=435

# ResolutionSetter
# 各个设备的分辨率可在该网站查看：https://www.apple.com.cn/iphone/compare/
# 若修改分辨率后屏幕黑屏，重启手机即可
https://github.com/Halo-Michael/ipas/blob/main/resolutionsetter.ipa

# ResolutionSetterSwift
https://github.com/haoict/haoict.github.io/tree/master/cydia/ipa

# BlizzBoards
https://appinstallerios.com/TrollStoreIPAs/BlizzardBoard.ipa

# H5GG
https://github.com/H5GG/H5GG

# uYou+
https://github.com/qnblackcat/uYouPlus/releases

# OldOS
https://github.com/zzanehip/The-OldOS-Project/releases

# RingTonesManager
https://share.initnil.com/With_TorllStore

# NiceBattery
https://www.niceios.com/download.php

# 电池助手
https://www.123pan.com/s/vgn0Vv-Ex3Fv
https://pan.baidu.com/s/1YBIuBzHONOTwYZ0YTYZUVg?pwd=ik6g（提取码 / ik6g）

# PostBox
https://www.postbox.news/

# 静音模块
# 将文件用Filza放置到/var/Managed Preferences/mobile中，注销后即可在控制中心添加静音按钮
https://www.123pan.com/s/vgn0Vv-5I3Fv
https://pan.baidu.com/s/1N_-DkwRejMQOz_dBu9GZwA?pwd=hufn（提取码 / hufn）

# ModMyIPA
# 安装后通过修改现有IPA包的标识符，可实现应用多开
https://github.com/powenn/ModMyIPA

# 电话助手
https://www.htv123.com/down/CallAssist_TrollStore.ipa

# TrollTools
# 多功能工具箱
https://github.com/sourcelocation/TrollTools

# WallpaperSetter，自定义浅色模式深色模式的墙纸
https://github.com/Skittyblock/WallpaperSetter

# CAPerfHUD，浮窗显示手机性能
https://github.com/khanhduytran0/CAPerfHUD

# IPA提取器
https://github.com/SmileZXLee/IpaDownloadTool

# AppRhyme，音乐播放器
https://github.com/canxin121/app_rhyme
# 音源
https://github.com/hhhackor/AppRhymeApi/raw/main/custom_api.evc

# Blossom，用视频作为壁纸
https://github.com/inyourwalls/Blossom

# AnimationSpeed，动画加速
https://www.123865.com/s/vgn0Vv-IowFv

# RebootTools，一键重启
https://github.com/DevelopCubeLab/RebootTools

# ExtractApp，提取App
https://github.com/DevelopCubeLab/ExtractApp

# BatteryInfo，电池信息
https://github.com/DevelopCubeLab/BatteryInfo

# UiharuX Pro，App 快捷启动、跑马灯、灵动岛、雪花飘落、控制面板等
https://github.com/straight-tamago/UiharuX

# ClearFilzaResidue，清理Filza残留
https://github.com/DevelopCubeLab/ClearFilzaResidue

# MuffinStore Jailed，安装指定旧版应用
https://github.com/mineek/MuffinStoreJailed-Public
```

## KFD专用

```
# posi0nKFD
https://github.com/GenericCoding/kfd

# KFDColors
# 修改角标颜色
https://wwqw.lanzouk.com/b0198pbsd
```

## TrollRestore专用

```
# misakaX
https://github.com/straight-tamago/misakaX

# Nugget
https://github.com/leminlimez/Nugget

# Cowabunga Lite
https://github.com/leminlimez/CowabungaLite
```

# 参考教程

## iOS 系统，真网速悬浮窗，无需越狱

```
https://mp.weixin.qq.com/s/j4I1rATrvKwF6rpQDf4YVg
```

## 超爽！iOS 16.1.2 更换 App 提示音，不用越狱

```
https://mp.weixin.qq.com/s/3Q-zpaIBDrAsfduWZLpYUg
```

## 真好玩，实现 iOS 16.1.2 透明文件夹，不用越狱

```
https://mp.weixin.qq.com/s/C8z-ozXvEsMw3rosKF_S4g
```

## iOS 16.5 Misaka 替代品，插件源通用

```
https://mp.weixin.qq.com/s/A2bwFZRAjviM6-9I5US1lw
```

## iOS 16.5 系统，PureKFD 插件商店，无需越狱，支持这些系统

```
https://mp.weixin.qq.com/s/hwycMbSa2O2TYhBddNxeaQ
```

## iOS 14.3 PlankFilza 来了，免越狱修改 X 手势

```
https://mp.weixin.qq.com/s/Fs3Qj3flAC2KgBw7OGAKzw
```


## 电池百分比显示，终于来了，无需越狱

```
https://mp.weixin.qq.com/s/zxIvLXdJIZ0zn_8JXhnLVQ
```

## 新款电池百分比显示，状态栏显示，功能丰富，无需越狱

```
https://mp.weixin.qq.com/s/Bifd41nNVdn5t5DIWZA4tg
```

## IPA 提取器，完全免费，无需越狱

```
https://mp.weixin.qq.com/s/jF8rUJpkxylM4TdoEHsDJA
```

## iOS 系统，手机性能 HUD，无需越狱

```
https://mp.weixin.qq.com/s/SyG9g0jGYW2Uk5cjxQLIRQ
```

## iOS 系统，自定义外观模式墙纸，无需越狱

```
https://mp.weixin.qq.com/s/SK-4YwXi1Gy9rsnmbcMMPg
```

## iOS 系统，通知发光，无需越狱

```
https://mp.weixin.qq.com/s/SmPKL6stDiZAxzEz2jUN5w
```

## iOS 系统，自定义控制中心，无需越狱

```
https://mp.weixin.qq.com/s/XGrnvicqsBQpn6szBBWpjQ
```
## iOS 16.1.2 DirtyCow 已发布，可以实现一键替换

```
https://mp.weixin.qq.com/s/MOrmpFG0SA8k62d0m9rQCA
```

## iOS 系统，文件管理器发布，无需越狱

```
https://mp.weixin.qq.com/s/SICZ0FI7yUPOVBe8e5nb7w
```

## 奶牛系列官方网站，正式上线，请收藏

```
https://mp.weixin.qq.com/s/KSFRwIA3TPEIBu6HxShzAQ
```

## iOS 系统，听歌自由，速度

```
https://mp.weixin.qq.com/s/yeDc6y-dK7JCcNlwBG_e8Q
```

## misakaX，更新1.7，支持 iOS 16

```
https://mp.weixin.qq.com/s/1gkgYk_K-UZLMh9syK6lsg
```

## 新工具，金块

```
https://mp.weixin.qq.com/s/UqASjZN5qAkPZT9s7McZFA
```

## 关于 MacDirtyCow，支持的系统范围，必看

```
https://mp.weixin.qq.com/s/h9R_veKuOd2ienAHce9KYw
```

## FilzaEscaped15 来了，iOS 系统文件管理器，无需越狱

```
https://mp.weixin.qq.com/s/cDmx4doiumVOBZuuE9jUjg
```

## iOS 系统，自定义4G/5G，无需越狱，非常简单

```
https://mp.weixin.qq.com/s/2bbId2CsBfQMt8OxdBlHkw
```

## 黑科技，老款 iPhone 激活灵动岛，无需越狱，支持这些系统

```
https://mp.weixin.qq.com/s/_yDmSWBKnv7wGjtm_F7UqA
```

## 轻松签+，无需证书，永久签名，永不过期，无需越狱

```
https://mp.weixin.qq.com/s/STdwUdba7Pc8klnfzfcmlw
```

## iOS 永久签名，永不过期，无需越狱，完全免费，速度

```
https://mp.weixin.qq.com/s/fjD7CDQFZ_BGvCsz8qdyog
```

## FileSwitcherPro，多功能修改器（一）

```
https://mp.weixin.qq.com/s/pfHhe7kyiUAkt7TUKmLJ7g
```

## FileSwitcherPro，多功能修改器（二）如何替换文件

```
https://mp.weixin.qq.com/s/p7J3YK9eHFYq7LxvSqZ-VQ
```

## Since we don’t have iCleaner Pro for TrollStore, it’s possible to delete some large cache files from Filza without harm to the system.

```
https://www.reddit.com/r/Trollstore/comments/yhw4kb/since_we_dont_have_icleaner_pro_for_trollstore/
```

## iOS 系统，一键更换字体，大神接手，新版下载地址，必看

```
https://mp.weixin.qq.com/s/Rf8WAaKGz26pAWYSkCo6Bg
```

## iOS 系统，自定义小元素，个性十足

```
https://mp.weixin.qq.com/s/dG4TWMeWlEfMmXyEKvYt2Q
```

## iOS 系统，自定义锁屏手电筒和相机，简单

```
https://mp.weixin.qq.com/s/ux2yU0tRg2w5_fQIAJ_FvQ
```

## iOS 系统，桌面 App 快速移动到指定文件夹

```
https://mp.weixin.qq.com/s/5ImIQevXIN071Jc9nG-A0w
```

## iOS 系统，全新全局手势神器，完全免费

```
https://mp.weixin.qq.com/s/O6Dug7Cj3tT5I1d_hICaeA
```

## iOS 16.5 系统，SimpleKFD 工具箱，一键监督，一键灵动岛

```
https://mp.weixin.qq.com/s/BxN8UzhyzcBC7oF59z5P0Q
```

## 好东西，iOS 14 制作永久版应用，仅需几秒搞定

```
https://mp.weixin.qq.com/s/L-vudctVvu99yqcDEqXKXg
```

## 好东西，iOS 16.1.2 Files 文件管理器，确实有效

```
https://mp.weixin.qq.com/s/yGbrTzI_NejCerf0KrJeKQ
```

## 牛批！iOS 16.5 修改角标已发布，可改颜色

```
https://mp.weixin.qq.com/s/seEwRkgkCC77_PaICdqrlg
```

## 新工具，Blossom ！

```
https://mp.weixin.qq.com/s/HhRDXervYkIAVXZPn0UdMw
```

## 让 iPhone 再战三年！

```
https://mp.weixin.qq.com/s/5ycNjO5_4zHZlwheR8bVbA
```

## 新玩法，加速

```
https://mp.weixin.qq.com/s/uO_QLNaMm9dR90DCkHq0Eg
```

## 一键重启 ！

```
https://mp.weixin.qq.com/s/_funZkt1EJfJj6z_XdeRDQ
```

## 禁止摇一摇！

```
https://mp.weixin.qq.com/s/m4dxYgTWV8Olml-QUgYCsw
```

## 汉化，简体中文

```
https://mp.weixin.qq.com/s/b3zLL1IIxIPmJ4YWFVMO4w
```

## iOS 系统，音乐，速度

```
https://mp.weixin.qq.com/s/s7gMo2w-0f0t53JT7_skyw
```

## 好东西！iOS 17.0 巨魔查电池，详细记录循环信息

```
https://mp.weixin.qq.com/s/fg6gnTBYJ8ktS6Id_aQpgQ
```

## 伪装，开启 CallKit！

```
https://mp.weixin.qq.com/s/Awu1ErsQi32UzNaLcpBUzA
```

## 清除残留 ！

```
https://mp.weixin.qq.com/s/wU_Tesa3OCZUMKWvK5WbRA
```

## 新工具，抢鲜

```
https://mp.weixin.qq.com/s/UFucgW1usFSvQX7BRJ0rZQ
```