---
title: Mac使用技巧总结
categories: Mac
abbrlink: Mac-Skills
date: 2019-11-25 14:12:29
tags:
---

![](topic.jpg)

总结了一些MacOS的使用技巧，不断更新。

<!-- more -->

# 查看系统版本

点击菜单栏中的关于本机即可。或在磁盘工具中也能看到磁盘对应的系统版本。

# 系统网络在线重装

在开机时按住以下快捷键之一即可。

|         快捷键         |                        效果                       |
|------------------------|---------------------------------------------------|
| Command+R              | 重新安装当前系统版本                              |
| Option+Command+R       | 升级到最新版macOS                                 |
| Shift+Option+Command+R | 安装出厂时的macOS或与其最接近的官方仍在提供的版本 |

# 访问Mac共享

在Mac打开设置，选择共享-文件共享，点击`选项`，勾选两个复选框，并勾选用于共享的用户。回到设置页面，打开`网络偏好设置`，选择活跃的连接，点击高级-WINS，输入Windows的工作组名称，一般为WORKGROUP。

在Windows端打开运行，输入`\\[Mac的IP地址]`即可访问。

# 旧款Mac安装Catalina

使用macOS Catalina Patcher即可。

```
http://dosdude1.com/catalina/
```

# 直接下载Boot Camp驱动程序

使用Brigadier即可。

```
https://github.com/timsutton/brigadier/releases
```

# 安全模式下读取EFI分区

注意，此法不能读取内置Windows分区和外接驱动器（非APFS/HFS）分区。

在正常模式的系统下执行以下命令以获取msdosfs.kext，默认在根目录的kexts文件夹下。

```
sudo mkdir /kexts
sudo cp -RX /System/Library/Extensions/msdosfs.kext /kexts
sudo /usr/libexec/PlistBuddy -c "Add :OSBundleRequired string" /kexts/msdosfs.kext/Contents/Info.plist
sudo /usr/libexec/PlistBuddy -c "Set :OSBundleRequired \"Safe Boot\"" /kexts/msdosfs.kext/Contents/Info.plist
sudo chmod -Rf 755 /kexts/msdosfs.kext
sudo chown -Rf 0:0 /kexts/msdosfs.kext
```

在安全模式下通过以下命令加载此kext，再挂载EFI分区即可。

```
sudo kextload /kexts/msdosfs.kext
```

# 连接ZJU有线网络

打开系统偏好设置-网络，点击+，接口选择vpn，类型选L2TP/IPSec，完成后填写下列信息。

```
服务器地址 / 10.5.1.7
账户名称 / 学号@a10（20/50元包月则为@a20/@a50）
```

点击鉴定设置-密码并输入，保存后点击`高级`。会话选项下三个选项均勾选，保存。打开终端输入以下代码。 

```
sudo vim /etc/ppp/options
```

按`i`键后输入下列语句。

```
plugin L2TP.ppp
l2tpnoipsec // 第一个字母为小写L
```

按Esc后输入`:wq`并回车，回到网络设置界面。连接上述设置好的VPN，正常情况下连接成功。

# 重置蓝牙配置

进入系统盘的/Library/Preference，删除`com.apple.Bluetooth.plist`文件后重启即可。

# 备份EFI分区脚本

脚本地址如下，运行EFI Backup-Restore.command即可。

```
https://github.com/corpnewt/EFI-Backup-Restore
```

# 系统精简

打开终端并输入以下命令。

```
// 获取Su权限
sudo su
passwd root
su root

// 解除系统应用
cd /
mkdir RemovedFiles
cd RemovedFiles
mkdir System-Library-LaunchAgents
FLA=/RemovedFiles/System-Library-LaunchAgents/
cd /System/Library/LaunchAgents/
mv com.apple.AddressBook*                      $FLA # 通讯录
mv com.apple.CalendarAgent.plist               $FLA # 日历
mv com.apple.iCloudUserNotifications.plist     $FLA # iCloud
mv com.apple.icbaccountsd.plist                $FLA # iCloud
mv com.apple.icloud.fmfd.plist                 $FLA # iCloud
mv com.apple.cloud*                            $FLA # iCloud
mv com.apple.imagent.plist                     $FLA # FaceTime
mv com.apple.IMLoggingAgent.plist              $FLA # FaceTime
mv com.apple.AirPlayUIAgent.plist              $FLA # Airplay
mv com.apple.AirPortBaseStationAgent.plist     $FLA
mv com.apple.bird.plist                        $FLA # iCloud 缓存
mv com.apple.findmymacmessenger.plist          $FLA
mv com.apple.gamed.plist                       $FLA # macOS 游戏中心
mv com.apple.parentalcontrols.check.plist      $FLA # 家长控制
mv com.apple.Maps.pushdaemon.plist             $FLA
mv com.apple.ScreenReaderUIServer.plist        $FLA
mv com.apple.speech.*                          $FLA

cd /System/Library/LaunchDaemons/
launchctl unload -wF com.apple.AirPlayXPCHelper.plist # AirPlay
launchctl unload -wF com.apple.apsd.plist # Apple push notification

# 应先在系统偏好设置中禁用位置服务，再执行该语句
launchctl unload -wF com.apple.locationd.plist

launchctl unload -wF com.apple.findmymac.plist
launchctl unload -wF com.apple.findmymacmessenger.plist

launchctl unload -wF com.apple.icloud.findmydeviced.plist
launchctl unload -wF com.apple.cloudfamilyrestrictionsd-mac.plist 
launchctl unload -wF com.apple.mbicloudsetupd.plist

launchctl unload -wF com.apple.dvdplayback.setregion.plist 

launchctl unload -wF com.apple.SubmitDiagInfo.plist 
launchctl unload -wF com.apple.CrashReporterSupportHelper.plist 
launchctl unload -wF com.apple.ReportCrash.Root.plist 
launchctl unload -wF com.apple.GameController.gamecontrollerd.plist

launchctl unload -wF com.apple.spindump.plist
launchctl unload -wF com.apple.metadata.mds.spindump.plist

# Disable cups (common unix printing service)
#launchctl unload -wF org.cups.cupsd.plist
#launchctl unload -wF org.cups.cups-lpd.plist
```

# 更换字体

打开/System/Library/Frameworks/ApplicationServices.framework/Frameworks/CoreText.framework/Resources/DefaultFontFallbacks.plist，将STHeitiSC-Light进行修改即可，如冬青黑体简体中文W3为HiraginoSansGB-W3。

# 远程控制

可通过iMessage，打开后选择或新建对方的信息会话，点击右上角后选择共享即可。

# 删除Launchpad的废图标

打开访达，选择前往-前往文件夹，输入`/private/var/folders`并回车。在本路径下搜索`com.apple.dock.launchpad`，双击进去可看到db。

打开终端并输入以下命令即可。

```
# 替换为刚才找到的db路径
cd /private/var/folders/[...]/db

sqlite3 db "delete from apps where title='[需要删除的应用名称]';"&&killall Dock
```

# Menu Bar操作

按住Command键并拖动菜单栏图标可调整其顺序，拖出菜单栏则清除此图标。

若菜单栏上没有图标，则转至`/System/Library/CoreServices/Menu Extras`，直接用鼠标将Eject.menu拖放至菜单栏即可。

# 常用快捷键

Mac键盘与普通键盘键位对应如下表。 

| Mac     | Windows             |
| ------- | ------------------- |
| Command | Windows徽标键       |
| Option  | Alt键               |
| Delete  | 空格（Backspace）键 |
| Return  | 回车（Enter）键     |

替换Command和Control键后，常用快捷键对应如下（Space键即空格键，Win键即Windows徽标键）。

| Mac                           | Windows                | 作用                       |
| ----------------------------- | ---------------------- | -------------------------- |
| **常用**                      |                        |                            |
| Option                        |                        | 显示菜单隐藏项             |
| Command+C                     | Ctrl+C                 | 复制                       |
| Command+V‍                     | Ctrl+V                 | 粘贴                       |
| Command+Option+V              | Ctrl+Alt+V             | （Ctrl+C后）剪切           |
| Command+Delete                | Ctrl+Backspace         | 删除                       |
| Command+Z                     | Ctrl+Z                 | 撤销                       |
| Command+A                     | Ctrl+A                 | 全选                       |
| Command+F                     | Ctrl+F                 | 全选                       |
| Command+S                     | Ctrl+S                 | 保存                       |
| **截屏**                      |                        |                            |
| Option                        |                        | 维持比例                   |
| Control                       |                        | 保存截图到剪切板           |
| Command+Shift+3               | Ctrl+Shift+3           | 截取全部屏幕到文件         |
| Command+Shift+Control+3       | Ctrl+Shift+Win+3       | 截取全部屏幕到剪切板       |
| Command+Shift+4               | Ctrl+Shift+4           | 截取所选屏幕区域到一个文件 |
| Command+Shift+Control+4       | Ctrl+Shift+Win+4       | 截取所选屏幕区域到剪贴板   |
| Command+Shift+Control+Space+4 | Ctrl+Shift+Win+Space+4 | 仅捕捉一个窗到剪贴板       |
| **文件**                      |                        |                            |
| Control+Command+I             |                        | 查看多个文件简介           |
| Command+I                     |                        | 查看单个文件简介           |
| Command+Shift+N               | Ctrl+Shift+N           | 新建文件夹                 |
| Command+Shift+G               | Ctrl+Shift+G           | 输入文件路径               |
| Command+Up                    | Ctrl+Up                | 回到上级目录               |
| Command+Down                  | Ctrl+Down              | 进入下级目录（打开文件）   |
| Space                         | Space                  | 预览                       |
| Command+O                     | Ctrl+O                 | 打开所选文件               |
| Command+Shift+Delete          | Ctrl+Shift+Backspace   | 清倒废纸篓                 |

# 临时禁止通知中心弹出通知

按住`Option`后点击右上角通知中心图标即可。

# 把PNG转成JPG

直接修改后缀名即可。

# 触控板设置三指拖移

进入系统偏好设置-辅助功能-鼠标与触控板-触控板选项…，勾选启用拖移并选择三指拖移即可。

# 归类文件

选中多个文件，右键选择`使用多个文件新建文件夹`。

# 显示隐藏文件

利用`command（⌘）+shift+.`即可。

也可打开终端，输入以下命令。

```
// 显示隐藏文件
defaults write com.apple.finder AppleShowAllFiles -bool true
// 或defaults write com.apple.finder AppleShowAllFiles YES

// 隐藏隐藏文件
defaults write com.apple.finder AppleShowAllFiles -bool false
// 或defaults write com.apple.finder AppleShowAllFiles NO
```

# 删除所有短信

设置短信为保留30天，然后设置日期，拨动往后的几个月份直至短信被删除即可。

# QQ功能扩展

下载以下仓库，解压后在终端运行Other文件夹中的`install.sh`即可。

```
https://github.com/TKkk-iOSer/QQPlugin-macOS/tree/master
```

# 微信功能扩展

该插件已造成封号，不推荐使用。

在终端输入以下命令即可。

```
curl -o- -L https://raw.githubusercontent.com/lmk123/oh-my-wechat/master/install.sh | bash -s
```

项目地址如下。

```
https://github.com/MustangYM/WeChatExtension-ForMac
```

安装扩展后可以多开微信，在命令行输入以下命令即可。

```
open -n /Applications/WeChat.app
```

# Quick Look插件

通过安装Quick Look插件，可以在点击空格预览的时候有更多功能，仓库如下。

```
https://github.com/sindresorhus/quick-look-plugins
```

在终端通过以下命令进行安装，常用插件有qlcolorcode、qlstephen、qlmarkdown、quicklook-json、qlimagesize、suspicious-package等。

```
brew cask install [插件名]
```


# Xcode SDK文件

用于为Xcode提供旧版SDK文件。

```
https://github.com/phracker/MacOSX-SDKs
```

# 常见问题

## 构建Xcode时提示「xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer directory」

在终端输入以下命令即可。

```
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer/ 
xcode-select --print-path
// 输出/Applications/Xcode.app/Contents/Developer
```

## App在Catalina下提示已损坏

在终端输入以下命令即可。

```
// /Applications/xxxx.app换为所需App路径，推荐直接将app文件拖入终端
sudo xattr -d com.apple.quarantine /Applications/xxxx.app
```

## 因AppleGFXHDADriver重启

将SLE下的AppleGFXHDA.kext删除或更改后缀即可。

## 旧Mac电脑无法开启高分辨率

可使用mac-pixel-clock-patch-V2补丁，仓库如下。

```
https://github.com/Floris497/mac-pixel-clock-patch-V2
```

# 参考教程

## `为什么创建点下划线._文件，如何避免使用它们？`

```
https://qastack.cn/apple/14980/why-are-dot-underscore-_-files-created-and-how-can-i-avoid-them
```

## ItChat 系列 0 - 初识 ItChat

```
https://www.playpi.org/2019020701.html
```

## 在Mac系统中如何显示和隐藏文件

```
https://www.jianshu.com/p/a1c2495b02aa
```

## 如何删除GIT中的.DS_Store

```
https://www.jianshu.com/p/fdaa8be7f6c3
```

## Mac 重装系统教程(二)：网络在线重装

```
https://m.sohu.com/a/325579861_654244
```

## OS X 默认字体的讨论与修改方法

```
https://blog.royli.dev/2018/os-x-mo-ren-zi-ti-de-tao-lun-yu-xiu-gai-fang-fa-2d38abb1
```

## 删除启动台（LaunchPad）残留的图标

```
https://zhuanlan.zhihu.com/p/55866195
```
