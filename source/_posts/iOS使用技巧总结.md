---
title: iOS使用技巧总结
categories: iOS
abbrlink: iOS-Skills
date: 2020-03-20 19:06:29
tags:

---

![](topic.jpg)

共享应用、破解内购等。

<!-- more -->

# TODOS

```
iOS 13 超级有用的功能，可方便传输文件
https://mp.weixin.qq.com/s/UyNWytJ4hvoDpcaj6p-x1Q


PPL实现
https://paper.seebug.org/1768/

iOS一些漏洞
https://googleprojectzero.blogspot.com/2020/07/the-core-of-apple-is-ppl-breaking-xnu.html



iOS逆向
https://iosre.com/t/ios安全和逆向系列教程/22761






越狱
https://ios.cfw.guide/


重新登录appleID后会安装很多app

原因应该是之前下他们的时候下到一半就没下，然后重新登录后会自动完成没下完的部分

解决：


https://discussionschinese.apple.com/thread/253150810

让他们下完，在全部删掉

或者：

https://discussionschinese.apple.com/thread/253193718

我以前也是，后来没有了，你试试，你点开app，点击右上角头像，进去再点击账户头像，下拉有一个删除此设备（写了什么自动下载，我忘记了），然后来回切换ID都可以了



双引导
https://github.com/dualra1n/dualra1n

降级
https://github.com/mineek/sunst0rm
https://github.com/edwin170/downr1n

iOS马甲包
https://github.com/lightank/LTVestbagDemo


Charles Proxy抓IPA包
https://sspai.com/post/36122




JsBox ChatGPT Keyboard
https://neurogram.notion.site/ChatGPT-Keyboard-af8f7c74bc5c47989259393c953b8017




一些模拟器
https://mp.weixin.qq.com/s/XIdVlLJ19kZ-Jli_N9Ejvg
https://www.123pan.com/s/vgn0Vv-yTxFv.html
https://mp.weixin.qq.com/s/AO3GzaZSIdAyxusHgIfxOA
https://mp.weixin.qq.com/s/IDPVqhTZQXC0JF-xv1sw_w
https://www.123pan.com/s/vgn0Vv-o04Fv.html
https://mp.weixin.qq.com/s/v9QEjwVaBmrcvUMP2F59Cw
https://mp.weixin.qq.com/s/pTjy6CxJTIsoW285H5W82g
https://mp.weixin.qq.com/s/vmjBR0MoXhUmlAHrJglNOw
https://mp.weixin.qq.com/s/j-MyhIR2mXlBcI-BDzPgBg
https://mp.weixin.qq.com/s/f3okIyI3QrMvpENVaaKKOQ
https://mp.weixin.qq.com/s/wIPMP7VFhD1pVh8DVICZIg
https://mp.weixin.qq.com/s/qFf5d4bjNoUh2gsxcyvQBA
```

# 硬件技巧

## Home键失灵

越狱后可安装CCSettings插件在控制中心添加主屏幕按钮，或安装Activator插件实现手势操作。

## 电源键失灵

可通过爱思助手完成重启。

# 系统技巧

## 恢复模式

### 进入

#### 正常方法

彻底关闭设备，并且将设备与PC断开连接。长按Home键的同时将设备连接到计算机，不要松开Home键，直到在设备屏幕上看到`连接到iTunes`的图片。

#### 备选方法

可在按键失灵时使用。用爱思助手即可。

### 退出

按住电源键十秒即可。通过爱思助手进入的需要通过爱思助手退出。

## DFU模式

### 进入

#### 正常方法

把设备连接到PC后关闭设备，同时按住Home键和电源键十秒后松开电源键，继续按住Home键，直到在电脑上看到识别到DFU状态下的USB设备。

#### 按键失灵方法

##### 通过脚本

把设备连接到Windows后下载以下压缩包，解压后双击`DFU.bat`即可。

```
https://feng-bbs-att-1255531212.file.myqcloud.com/2011/10/15/2472983_DFU.rar
```

##### 通过固件

该方法适合早期的iPhone，主要是iOS7及以下的系统。安装较早版本的iTunes（10或11），并下载当前系统的固件。然后下载Redsn0w，链接如下。

```
http://www.iphonehacks.com/download-redsn0w
```

下载完成后以管理员身份运行Redsn0w，选择Extras-Even More-DFU IPSW，选取下载好的固件，等待制作完成。然后连接手机并打开iTunes，连接成功后按住Shift并点击恢复，选择制作好的DFU固件，刷机完成后即进入DFU模式。

### 退出

按住Home键和电源键十秒，设备关机之后重新启动设备即可。

# 操作技巧

## 添加日历订阅

点击设置-日历-账户-添加账户-其他-添加已订阅的日历，填入服务器地址即可。推荐订阅如下。

```
# 中国节假日补班日历订阅
# Github 仓库为 https://github.com/lanceliao/china-holiday-calender
https://www.shuyz.com/githubfiles/china-holiday-calender/master/holidayCal.ics

# 中国二十四节气日历订阅
# Github 仓库为 https://github.com/KaitoHH/24-jieqi-ics
https://cdn.jsdelivr.net/gh/KaitoHH/24-jieqi-ics@master/23_solar_terms_2015-01-01_2050-12-31.ics

# 电影、美剧、英剧公共日历
https://calendar.google.com/calendar/ical/291ig4cijjnirlc6krcopq5gj4%2540group.calendar.google.com/public/basic.ics
```

也可在以下网站寻找订阅。

```
http://icalshare.com/
https://sspai.com/post/43209
```


## 修改步数

下载`乐心健康`，登录并在设置中选择数据共享，开启需要修改步数的APP。打开以下网页，登录并修改步数即可。

```
http://step.xbmmw.top/
```

## 修改定位

可通过以下网站查询经纬度。

```
http://www.gpsspg.com/maps.htm
```

### 通过爱思助手

通过爱思助手的虚拟定位功能即可。

### 通过抓包

打开网络调试软件（如Thor）后，打开需要修改定位的App，点击定位相关按钮，查找关于location的数据包，进入经纬度进行修改即可。注意经纬度的小数点精确的位数。

## 查看电池循环次数

打开设置-隐私-分析与改进，保证`共享iPhone分析`处于打开状态，然后点击`分析数据`，下拉并在搜索框搜索`log-aggregated`，找到时间最近的文件，点击右上角并选择`存储到文件`，保存到手机中。

添加以下快捷指令，打开后选择刚才保存的文件即可。

```
https://www.icloud.com/shortcuts/df02957b73414724b94fa58e0a836461
```

## 日历添加法定节假日安排

打开日历APP，点击日历-添加日历-添加订阅日历，链接如下。

```
https://www.hurbai.com/ical.ics
```

## 将iPad作为电脑副屏

有线连接可使用Duet Display、Twomon、Splashtop Wired XDisplay。无线连接可使用Spacesdesk、Duet Air、Moonlight。将平板作为数位板则可考虑EasyCanvas、Astropad Studio。

也可使用Splashtop。在iPad安装Splashtop Personal，打开后点击`我想在封闭网路中使用Splashtop`，按照提示在电脑安装Splashtop Streamer并连接即可。如果希望将iPad作为扩展屏，则可购买HDMI欺骗器并接到电脑上，然后切换投影模式为扩展即可。

## 系统清理

在设置中将系统时间调后一个多月，等待片刻再恢复自动校准时间。

## iOS 13双网变全网通

iPhone 11以下设备（不含），港版、韩版、英版、美版等仅支持移动和联通，不支持电信。可通过开通电信VoLTE的方式添加支持。


## 密码与激活锁

当iOS打开了查找我的iPhone功能且无有效的激活记录时，将会导致激活锁定，需要输入开启了查找我的iPhone对应的iCloud账号和密码才可解锁。若忘记密码，则需要绕过激活锁。

若忘记了屏幕密码，则需要绕过屏幕密码锁。

工具可通过以下链接下载。

```
https://appletech752.com/icloudbypass.html
https://appletech752.com/downloads.html
```

```
绕过ID锁，具体操作方法

条件要求：
仅支持A5-A11设备
MAC系统或黑苹果系统
Checkra1n越狱工具（在艾锋降级-在线越狱获取）



操作步骤：
先将设备进入dfu模式
使用Checkra1n进行越狱
在端口44上使用ssh，同时删除或者重命名 Applications文件夹内的setup.app
执行 killall -9 SpringBoard.Boom，注销后即可跳过iCloud。



若是国行机，请往下看： 

执行命令有二个方法：
【方法1】
rm /Library/Preferences/com.apple.networkextension.plist 
killall CommCenter 

【方法2】
mv /Applications/Setup.app/Setup/Applications/Setup.app/Setup.bak touch /Application/Setup.app/Setup 
uicache --all 
killall backboardd 
killall -9 SpringBoard
```
## 切换AppStore账号后自动下载App的解决

进入AppStore中的账号详情，点击`删除此设备`即可。

# 参考教程

## 不按任何键即可进入DFU模式

```
https://www.feng.com/post/3028191
```

## 通过软件恢复进入DFU刷机模式教程

```
http://iphone.tgbus.com/tutorial/hacktutorial/201207/20120706173454.shtml
```

## 修改步数，免越狱，免root，wx&zfb&qq一键刷步数！！！

```
https://mp.weixin.qq.com/s/S6DhF6P8f68ejPz9xoP5bw
```

## iOS逆向工程初体验

```
https://medium.com/zrealm-ios-dev/ios-%E9%80%86%E5%90%91%E5%B7%A5%E7%A8%8B%E5%88%9D%E9%AB%94%E9%A9%97-7498e1ff93ce
```


## learn-regex/README-cn.md

```
https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md
```

## 无法 OTA 更新系统，解决办法来了

```
https://mp.weixin.qq.com/s/KzZ5ZX1xw_JBdgyCpTM5eg
```

## 小技巧｜查看 iPhone 电池循环次数

```
https://mp.weixin.qq.com/s/hBIanqg5-9jiHWppGCL1yg
```

## 神级应用！爱了！冲！

```
https://mp.weixin.qq.com/s/VjkXjLBKPCo1v5p0INDhsg
```

## 网页版小组件商店教程

```
https://www.notion.so/396b02ceeab04bb6b6e16492afbc4d3f?v=97d0676933f6462884643daa02320a2f
```

## 没用上这款系统内置神器APP，绝对是你的无谓损失！

```
https://mp.weixin.qq.com/s/dZWGfbTVG-78Qpe4yarKTA
```

## 让 iPhone 自带日历 App 显示国家法定节假日安排

```
https://mp.weixin.qq.com/s/ToZjqtRTgxPxswF02-H_oA
```

## 手机平板秒变电脑副屏，折腾一周后最流畅、最方便的方法

```
https://mp.weixin.qq.com/s/ZvLQQt1-FyToTtZA14Vifg
```

## 最出名的那些日历APP，结果一点都不够好用...

```
https://mp.weixin.qq.com/s/LSW7nB-5dLhEgSTqlOvy8g
```

## iOS 永久签名，支持 iOS 14.0 - 15.4.1，支持全系设备，官方在线安装，永不过期，完全免费

```
https://mp.weixin.qq.com/s/0f2My5VsQMSOoHXhwPo0aA
```

## [Tutorial] Save TONS of GB's on smaller storage iDevices!

```
https://www.reddit.com/r/jailbreak/comments/pmcj2s/tutorial_save_tons_of_gbs_on_smaller_storage/
```

## 切换id后会自动下载app

```
https://discussionschinese.apple.com/thread/253193718
```

## 美女更新，终于支持了

```
https://mp.weixin.qq.com/s/kPHfEvNLe85IuMZE3vP59g
```

## 大佬宣布离开，全部作品，完全免费

```
https://mp.weixin.qq.com/s/vgWr-J_Fy5z7ertnZ2ojNw
```

## 注意！iOS 15.0-15.1.1 电池容量可改，小心被骗

```
https://mp.weixin.qq.com/s/1jf8kFYwAq_Iasn8mL-7NQ
```

## iOS 13 双网秒变全网通方法，电信卡终于支持

```
https://mp.weixin.qq.com/s/FGDCsm5Po8j1MNRa2Aj8EQ
```

## DolphiniOS 模拟器已发布，掉签问题已修复

```
https://mp.weixin.qq.com/s/Htif1DpuPwTPOxHQYYaMnw
```

## iPhone 上玩真的 PSP 游戏，支持 iOS 13 系统

```
https://mp.weixin.qq.com/s/Shwv4N4lWlTVvDxGBrW23g
```

