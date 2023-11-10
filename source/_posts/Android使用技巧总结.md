---
title: Android使用技巧总结
categories: Android
abbrlink: Android-Skills
date: 2020-04-22 22:54:29
tags:
---

![](topic.jpg)

安卓使用技巧。

<!-- more -->

```
APK安装器——SAI
https://github.com/Aefyr/SAI




termux
https://github.com/termux/termux-app



API Levels
https://apilevels.com/


Shizuku
https://sspai.com/post/73294
https://shizuku.rikka.app/zh-hans/
https://www.xda-developers.com/shizuku/
https://forum.xda-developers.com/t/exploit-shizuku-support-smt-shell-v2-0-get-a-system-shell-uid-1000-within-the-app-itself-and-write-your-own-system-app-with-an-api.4561879/
https://www.reddit.com/r/fossdroid/comments/y8ewgf/a_list_of_apps_that_utilize_shizuku_for_elevated/
https://github.com/BLuFeNiX/SMTShell
https://forum.xda-developers.com/t/exploit-shizuku-support-smt-shell-v2-0-get-a-system-shell-uid-1000-within-the-app-itself-and-write-your-own-system-app-with-an-api.4561879/



Fdroid
https://depau.github.io/fdroid_shizuku_privileged_extension/fdroid/repo/
https://f-droid.org/packages/de.marmaro.krt.ffupdater/
https://gitlab.com/sunilpaulmathew/izzyondroid
https://github.com/redsolver/skydroid




双系统
https://www.jianshu.com/p/0045d820f586
https://zhuanlan.zhihu.com/p/48520404




解压/打包boot.img
https://www.whitewinterwolf.com/posts/2016/08/11/how-to-unpack-and-edit-android-boot-img/
https://forum.xda-developers.com/t/guide-unpack-repack-boot-recovery-img-without-kitchen.2839670/
https://github.com/cfig/Android_boot_image_editor
https://github.com/GameTheory-/mktool
https://github.com/pbatard/bootimg-tools
https://stackoverflow.com/questions/57485080/stock-boot-img-not-booting-after-re-packing
https://forum.xda-developers.com/t/tool-boot-img-tools-unpack-repack-ramdisk.2319018/



临时 root？
https://forum.xda-developers.com/t/how-do-i-temporarily-root-a-samsung-device-running-android-9-without-twrp-recovery.4421255/

Magisk Root
另外一个可能的方法：先刷入twrp
可以用supersu来看是不是真的被root了
Magisk修改的是boot分区
进不了boot分区会卡在android的界面，进不了system分区会卡在开机logo界面

救砖模块
https://sspai.com/post/57320
https://www.52pojie.cn/thread-1600094-1-1.html
刷了救砖模块手机反而启动不了了。。。
补救：
https://topjohnwu.github.io/Magisk/faq.html
ADB输命令，或者进Safe Mode（其实就是fastboot或者recovery）


magisk不适用于android 9是一个普遍现象
https://forum.xda-developers.com/t/why-magisk-doesnt-work-with-pie-roms.3848025/page-3
https://forum.xda-developers.com/t/solved-i-do-have-root-access-but-magisk-is-not-installed.3861626/



root的另外一个可能—kernelSU


Mac上的odin——heimdall
https://glassechidna.com.au/heimdall/#downloads
https://bitbucket.org/benjamin_dobell/heimdall/downloads/heimdall-suite-1.4.0-mac.dmg
可用homebrew安装，但在mac12上失败，因为它试图安装到系统盘
https://formulae.brew.sh/cask/heimdall-suite

Jodin基于heimdall，所以也不行








fastboot命令大全
https://www.jkmeng.cn/219.html







Heimdall的用法
https://forum.xda-developers.com/t/guide-how-to-install-use-heimdall-and-flash-back-to-stock.1113195/

两个exe文件，heimhall是命令行，heimhall-frontend是用户界面方式
替换driver的时候，如果没有samsung开头的设备，就选MSM8953（反正选错了驱动也装不上去的）
可能需要用1.42版本修复ERROR: Failed to Send Data! 问题：
https://www.reddit.com/r/LineageOS/comments/aymmy2/heimdall_error_failed_to_send_data/
1.42 版本 windows：
https://github.com/tothphu/heimdall_build

步骤：
1. 下载1.40和1.42的heimdall
2. 用1.40下面的Drivers/zadig，更换MSM8953的驱动
3. 命令行切换到1.42所在目录，运行以下命令：

.\heimdall.exe detect
.\heimdall.exe download-pit --output pit.pit --no-reboot
.\heimdall.exe print-pit --no-reboot

然后就可以将输出的分区表复制出来了，可以搜索一下相关分区
主要分区是BOOT RECOVERY SYSTEM
flash命令示例如下：

https://forum.xda-developers.com/t/tool-heimdall-1-4-rc1.2071724/

.\heimdall.exe flash --resume --BOOT boot.img











组合固件（新bootloader+旧系统，可以在新bootloader跑旧系统）
https://combinationfirmware.com/downloads/j3300zcu1aqh1/



TWRP
twrp不能挂载分区，应该是没有解密模块



Google Play版微信
https://www.coolapk.com/feed/45383246?shareKey=YThmNjhlNzAwMzZiNjQ0MzY5MTg~&shareFrom=com.coolapk.market_12.5.0




Samsung J3（armv7架构）要用微信8.0.2
可以在这里下载（在官网下的armv7装不了）
https://www.apkmirror.com/apk/wechat-tencent/wechat/wechat-7-0-10-release/wechat-7-0-10-android-apk-download/





Google Camera
https://www.bilibili.com/read/cv8122208
https://tecnoandroid.net/zh-TW/gcam-google-camera-xiaomi-huawei-samsung-oneplus-realme-redmi/
https://forum.xda-developers.com/t/downgrade-android-8-to-7-binary-3.3891802/
armv7可以用的版本（不过会闪退，闪退的话要更换维护版本）
https://www.apkmirror.com/apk/google-inc/camera/camera-4-2-035-141213305-release/google-camera-4-2-035-141213305-2-android-apk-download/download/?key=2997286a3ea65b09c97e3be9755b361bde73e4e8


降级bootloader（基本不可能，bootloader禁止从高SW REV刷到低SW REV）
https://forum.xda-developers.com/t/downgrading-bootloader-in-samsung.4382225/
https://www.reddit.com/r/AndroidQuestions/comments/nnlg6f/sw_rev_check_fail_bootloader_device_7_binary_2/
https://forum.gsmhosting.com/vbb/f777/how-downgrade-binaryu8-binary-u1-2963482/
https://forum.xda-developers.com/t/guide-skipping-kg-state-prenormal-on-oneui-android-pie-9-0.3911862/
https://forum.xda-developers.com/t/is-it-possible-to-downgrade-s8-from-pie-to-oreo-sw-rev-error.4135167/
https://forum.xda-developers.com/t/method-for-everyone-who-wants-to-downgrade-galaxy-s8-to-oreo-nougat.3946240/
https://forum.xda-developers.com/t/can-i-downgrade-the-samsung-bootloader-from-sw-rev-3-to-2.3650483/
从这里可以看到SW REV
https://samfw.com/firmware/SM-J3300
BIN/BINARY是什么
https://samfw.com/blog/what-is-bit-binary-value
只能刷BINARY VALUE比当前高或相同的固件，否则会报错
假设value是4，那么可以这样来搜索root文件：j3300 u4 root



Android看系统启动log需要root权限
https://android.stackexchange.com/questions/26080/does-android-keep-a-log-of-when-it-boots
https://www.xda-developers.com/how-to-take-logs-android/



LineageOS
需要适配的设备，才能源码编译



解压lz4
https://github.com/lz4/lz4





J3300 Root

用最新的magisk 26.1也不行

这个也不行
操作是先刷原版固件包，再刷它的AP包
https://support.halabtech.com/index.php?a=downloads&b=file&id=318127
https://drive.google.com/file/d/1uZRLjK4I2--h69aKJ51WS40vtqINJJ4k/view

j3300 android 9 SAR 是 No，要装magisk 23 才能看到
所以应该只传 boot.img 而不是整个 AP 包，但也还是不行，搞不懂



尝试安装时保留AVB 2.0/dm-verity 和 保持强制加密
https://zhuanlan.zhihu.com/p/143181151

用AP包：
都取消勾选：可启动，无Root
都勾选：可启动，无Root
只勾选保留AVB 2.0/dm-verity：可启动，无Root
只勾选保持强制加密：可启动，无Root

用img：
都取消勾选：可启动，无Root
都勾选：可启动，无Root
只勾选保留AVB 2.0/dm-verity：可启动，无Root
只勾选保持强制加密：可启动，无Root

看日志可以看到
E opjohnwu.magis: Not starting debugger since process cannot load the jdwp agent

https://forum.xda-developers.com/t/magisk-general-support-discussion.3432382/page-1565

I doubt that error has anything to do with it (it's got to do with debugging Java), but like I said earlier my log reading skills are limited. I can't see anything special in there, but it isn't certain that the problem manifests as an error. A full logcat without the heavy filtering might be necessary.

https://forum.xda-developers.com/t/magisk-general-support-discussion.3432382/page-1566


——原因很可能是升级了bootloader的版本，导致magisk的boot修补失效了
——可能可以尝试magisk in recovery

——更新（2022-10-6）
magisk 25.2+boot.img或recovery.img都不行
安装后用recovery的方式进入也不行

https://github.com/topjohnwu/Magisk/issues/4495
用这个也不行

尝试22-25.2的版本均不行

（如果想用22以下的，注意一进去的时候要马上按安装
不然检测到有更新的话，就按不了了
——但其实就算进去了也不行，会下载失败的）

尝试以下方法，还是不行：
https://trendyport.com/how-to-extract-boot-img-from-boot-img-iz4-and-root-samsung/?amp

尝试刷入后取消自动重启，自行进入Recovery并重置系统，还是不行
```

# 资料

```
https://source.android.com/setup/build/building-kernels-deprecated?hl=zh-cn
https://androidfilehost.com/
https://forum.renegade-project.org/t/845-windows/36
https://forum.xda-developers.com/t/halium-9-0-halium-boot-ubports-guide.4093517/
https://docs.halium.org/en/latest/
https://www.jianshu.com/p/9b85dce0dcb7
```



# 软件与框架

## Root专用

### Busybox

提供Android上缺失的Linux命令行实现，如dirname等。Root后通过以下APP安装即可。

```
https://github.com/meefik/busybox
```

### 文件管理

可用Root Explorer、MT文件管理器。

### 系统工具

#### Scene

用于锁CPU频率、超频等。

```
https://www.coolapk.com/apk/com.omarea.vtools
```

#### 澜系统工具箱

实现删温控等功能。

```
https://www.coolapk.com/apk/xzr.La.systemtoolbox
```

#### Devcheck Pro

查看机器硬件配置。

```
https://www.yxssp.com/23173.html
```

#### Termux

手机上使用终端。可以没有Root权限。

```
https://f-droid.org/zh_Hans/packages/com.termux/
```

### 游戏修改

#### GameGuardian

游戏修改器。

## Xposed

### 安装

#### 已ROOT方法

Xposed Installer下载链接如下。注意资源需要翻墙才能下载。

```
https://forum.xda-developers.com/t/official-xposed-for-lollipop-marshmallow-nougat-oreo-v90-beta3-2018-01-29.3034811/
```

由于APP资源地址由http更改为https，而Xposed Installer中并未更改，因此安装后会出现`could not load available zip files`错误，刷新下载页面时会出现`下载http://dl.xposed.info/repo.xml.gz时出错`的提示。这需要在安装前修改APP包的资源地址解决。

打开MT文件管理器，选择Xposed Installer的APK安装包，点击查看。选择classes.dex，用Arsc编辑器打开，在字符串页面搜索`http`，将所有搜索结果的`http://`都改为`https://`，保存并退出编辑器。选择刚才修改好的APK，点击自动签名，完成后点击安装即可。

若仍出现`could not load available zip files`错误，可提前下载Xposed的SDK包，然后放置到Android/data/de.robv.android.xposed.installer/cache/downloads/framework下，手机对应的SDK版本可在Xposed Installer主界面看到。下载链接如下。

```
https://dl-xda.xposed.info/framework/
```

如果仍未出现，需要点击右上角的横杠符号，勾选Show outdated version，才会出现Xposed版本。

找到下载包后，点击云朵符号，选择Install即可。

#### 免ROOT方法

可使用模拟框架。

```
# 太极
https://taichi.cool/zh/download.html

# Virtual Xposed
https://vxposed.com/

# 应用转生
https://lanzoui.com/b05xrjlbc
```

也可使用虚拟机VMOS Pro，打开并安装一个支持Xposed的安卓ROM，然后点击系统-设置，打开Xposed即可。

也可使用将Xposed模块嵌入APP的方式。

```
# LSPatch
# 得到的APP已经嵌入模块，直接使用即可
https://github.com/LSPosed/LSPatch

# Xpatch
# 需要安装得到的APP和Xposed模块
https://github.com/WindySha/Xpatch
```

### 模块使用

在下载页面即可下载模块。也可通过以下网站下载。

```
https://repo.xposed.info/module-overview
```

## 自动化操作

### Hamibot

Hamibot需要保持在线才能运行，因此需要给Hamibot自启动权限，同时关闭省电策略，防止APP在后台被清理。

```
https://hamibot.com/
```

对于自身不能自动运行的脚本，可通过创建自动任务的方式实现自动运行。

### 一触即发

```
https://www.coolapk.com/apk/com.yicu.yichujifa
```

可自行编辑脚本。示例脚本中模拟点击的屏幕坐标在不同手机上可能会不同，因此需要自行修改。保持在脚本的编辑页面，切换要需要点击的APP，将准心移动到需要点击的位置，然后点一下准心，选择相应的动作即可。

### 自动精灵

通过录制创建脚本。

```
https://www.coolapk.com/apk/com.zdanjian.zdanjian
```

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

部分手机如Sumsung国行版只是将Google Play隐藏了起来，重新安装相关APP即可。Google Play下载地址如下。

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

### 国际版APP应用市场

可用`9Apps`、`V-Appstore`、`F-Droid`。F-Droid可用以下镜像加速。

```
https://mirrors.tuna.tsinghua.edu.cn/help/fdroid/
```

## 常用工具

### Gboard

好用的输入法，在Google Play即可下载。

由于下载简体中文语言包需要一段时间，一开始使用拼音时会出现无法弹出中文选字的情况，挂翻墙并等待下载完成即可。

# 虚拟机

在安卓手机上通过安装相应的APP，可以运行电脑系统。

## Bochs

通过以下链接下载Bochs安装包和适用于Bochs的XP镜像。

```
https://pan.baidu.com/s/1i4FDLGT
http://pan.baidu.com/s/1gfj4a2Z
```

将apk格式的Bochs安装包和img格式的Windows系统镜像传送到手机，放置于任何目录下均可。安装Bochs，将`ata0-master`前面的复选框选中，然后点击`select`。选择img格式的系统镜像文件Windows.img，其他项无需更改。点击顶部的`HARDWARE`选项卡，按照以下设置。

|      选项     |             设置            |
|---------------|-----------------------------|
| CPU Model     | /                           |
| Chipset       | i440fx                      |
| 内存          | 517MB（可根据手机配置调整） |
| VGA Card      | cirrus_5446                 |
| Sound Card    | sb16                        |
| Ethernet Card | rtl8029                     |
| PCI/Slot1     | cirrus                      |
| PCI/Slot2     | ne2k                        |
| PCI/Slot3     | es1370                      |
| PCI/Slot4     | voodoo                      |
| PCI/Slot5     | none                        |

点击顶部的MISC选项卡，将Full screen前面的复选框选中以使Windows可以全屏运行。点击右上角绿色的Start按钮以启动Windows。

## Limbo

通过以下链接下载Limbo安装包和适用于Limbo的XP镜像。

```
http://pan.baidu.com/s/1pK7ua3D
http://pan.baidu.com/s/1miRtjJU
```

将apk格式的Limbo安装包和qcow2格式的Windows系统镜像传送到手机，放置于任何目录下均可。

安装Limbo并打开，加载虚拟机下选择新建，在弹出对话框中输入虚拟机名称，然后按照以下设置。

|       选项       |                   设置                  |
|------------------|-----------------------------------------|
| 用户界面项       | SDL                                     |
| CPU型号项        | athlon                                  |
| CPU核心数        | 2（可根据手机配置调整）                 |
| 运存项           | 64（可根据手机配置调整）                |
| 光驱/软盘A/软盘B | /                                       |
| 硬盘A            | 点击后选择打开，选择qcow2格式的镜像文件 |
| 硬盘B            | /                                       |
| 引导设备         | 硬盘                                    |
| 网络配置         | User                                    |
| 网卡             | rtl8139                                 |
| 显卡配置         | cirrus                                  |
| 声卡配置         | sb16                                    |
| 高级设置         | /                                       |
| 多线程AIO        | 勾选右侧复选框（警告可以无视）          |

全部设置完毕后，回到顶部，点击运行以启动Windows。

## APQ

通过以下链接下载APQ安装包、Zarchiver安装包和适用于APQ的XP镜像。

```
https://www.cr173.com/soft/692055.html
https://dl.pconline.com.cn/download/1815731.html
https://pan.baidu.com/s/1_aoxWWi0mfN1cXyS3V4I6g
```

获取root权限后打开APQ，点击data分区，等待文件处理完成。

打开zarchiver，打开APQ/kolibri.conf，将以下位置的文件名改为下载好的镜像文件即可。

```
#第一块硬盘，硬盘镜像文件为xp.qcow2，应位于SD卡上的APQ主目录下或写绝对路径
-fda kolibri.img
```

## Termux
 
无需root，打开即可在Android上运行Linux。

### 更换软件源

输入以下命令以打开配置文件。

```
vi $PREFIX/etc/apt/sources.list
```

修改安装源如下，保存并退出。

```
deb https://mirrors.tuna.tsinghua.edu.cn/termux stable main
```

运行以下命令完成更新。

```
pkg update
pkg upgrade
```

### SSH连接

输入以下命令。

```
// 安装SSH服务
pkg install openssh

// 设置密码
passwd

// 查询手机IP
ifconfig

// 查询当前用户
whoami

// 开启SSH
sshd -p 9000

// 确认SSH服务的监听端口
netstat -ntlp | grep sshd
```

完成后即可根据信息进行连接。

### 运行程序

SD卡目录的文件不具有可执行权限，需要拷贝到系统的/home文件夹并修改权限才可运行，示例如下。

```
cp Demo /home/Demo
cd /home
chmod 777 Demo

// 运行
./Demo
```

### 优化终端

可用zsh代替bash作为默认shell。通过以下命令即可。

```
sh -c "$(curl -fsSL https://github.com/Cabbagec/termux-ohmyzsh/raw/master/install.sh)"
```

可通过修改以下文件更改配色。

```
vi ~/termux-ohmyzsh/install.sh
```

### 软件库

#### waifu2x

用于图片放大。下载并直接运行即可。

```
https://github.com/tanyiok1234/waifu2x_srmd-ncnn-vulkan-termux-binary
```

### 常用命令

#### 访问SD卡目录

```
termux-setup-storage
```

### 完整Linux

Termux不是完整Linux，但可配合其它APP实现完整Linux的安装。

#### 通过AnLinux

##### 安装

安装AnLinux后打开，选择需要安装的Linux发行版。复制指令并在Termux的终端中运行即可。以Debian为例，完成后在Termux可通过以下指令启动系统。

```
./start-debian.sh
```

修改以上脚本，将以下行的注释取消即可访问手机自身的存储文件系统。

```
command+=" -b /sdcard"
```

##### 桌面环境配置

安装前需先启动系统。在AnLinux中按照步骤操作即可。

完成安装后需要安装VNC Viewer，然后在Termux中运行以下命令以开启VNC。

```
vncserver-start

// 若出现错误，需要先输入以下指令停止VNC
vncserver-stop
```

复制输出的地址到VNC Viewer，此处以`localhost:1`为例，密码为开启VNC时所设置的密码。完成后连接即可。

#### 通过atilo

##### 安装

输入以下命令。

```
pkg install proot
termux-chroot
echo "deb [trusted=yes] https://yadominjinta.github.io/files/ termux extras" >> $PREFIX/etc/apt/sources.list
pkg in atilo-cn
```

通过以下命令查看能够安装的版本。

```
atilo list
```

以安装Debian为例，命令如下。

```
// 安装
atilo install debian

// 卸载
atilo remove debian
```

注意，如果下载失败，需要先通过以下命令在手机的tmp目录中删除安装包，否则该脚本会直接跳过下载。

```
cd ~/.atilo
ls
rm -r tmp
```

完成安装后运行以下命令启动系统。

```
termux-chroot
startdebian
```

##### 环境配置

###### JAVA

```
apt search openjdk
apt install default-jdk
```

###### python

通过以下命令安装。

```
apt install python3 python3-pip
```

打开~/.pip/pip.conf，修改为以下内容以更换pip的软件源。

```
[global]
index-url =http://pypi.douban.com/simple  
[install]
trusted-host=pypi.douban.com
```

### 环境配置

#### Java

若未安装完整Linux，可通过以下命令。

```
pkg install ecj
pkg install dx
```

#### Fortran

需要安装完整Linux。输入以下命令以修改软件源。

```
vim $PREFIX/etc/apt/sources.list
```

在文件末尾添加以下语句，保存并退出。

```
deb https://its-pointless.github.io/files/24 termux extras
```

输入以下命令安装。

```
wget https://its-pointless.github.io/pointless.gpg
apt-get install gnupg
apt-key add pointless.gpg
apt-get update
```

通过以下命令检查版本号，选择两者均有的版本。

```
apt search gcc
apt search gfortran
```

以gcc-8为例，安装及检验命令如下。

```
apt install gcc-8
gfortran-8 -v
gcc-8 -v
```

#### OpenCL

需要安装完整Linux。运行以下命令以安装编译器。

```
apt install clang
apt install cmake
```

从以下链接下载编译所必须的头文件。

```
https://github.com/KhronosGroup/OpenCL-Headers
```

寻找手机中名称类似`libOpenCL.so`的文件，复制到主程序目录。主程序目录结构如下。

```
├── CL
│   ├── libOpenCL.so
│   └── [头文件]
├── CMakeLists.txt
├── [cl核文件，以HelloWorld.cl为例]
└── [主程序，以test.cpp为例]
```

CMakeLists.txt按照以下格式填写，通过cmake和make即可生成相应文件。

```
# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8.11)
 
# 项目信息
project (Test)
 
set(CMAKE_INCLUDE_CURRENT_DIR ON)
 
include_directories ("${PROJECT_SOURCE_DIR}/CL") 
 
# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)
 
#首先申明动态库的位置
LINK_DIRECTORIES("${PROJECT_SOURCE_DIR}")
 
SET(HELLO_SRC libOpenCL.so )
 
# 指定生成目标
add_executable (testopencldemo ${DIR_SRCS})
 
#制定具体的动态库的名称 
target_link_libraries (testopencldemo ${HELLO_SRC})
```

#### Jupyter

需要安装完整Linux。通过以下命令安装。

```
pip3 install numpy
pip3 install pandas
apt install libzmq
pip3 install jupyter
apt install freetype libpng pkg-config
pip3 install matplotlib
```

完成安装后通过以下指令启动。

```
jupyter notebook
```

可通过配置文件，避免输入`http://localhost:8888/`后面的一串密匙。

运行以下命令以生成配置文件和密匙。

```
jupyter notebook --generate-config
ipython
from notebook.auth import passwd
passwd()
```

完成密码输入后复制所得内容，修改data/data/com.termux/files/home/.jupyter目录下的jupyter_notebook_config.py，如下。

```
c.NotebookApp.password = 'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed' # 修改为前面所得的内容
c.NotebookApp.ip = '*' # 允许任何ip访问
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8888 # 可自行指定端口，访问时使用
```

完成后保存，启动后即可直接通过以下网站访问。

```
http://localhost:8888/

// 电脑端利用SSH登录手机后可输入以下网站
[手机IP地址].8888
```

可运行以下命令以安装python扩展包，使编写python时有提示符。安装完成后在Jupyter notebook中出现Nbextensions，选择Hinterland即可。

```
apt install libxml2
apt install libxslt
pip3 install jupyter-contrib-nbextensions
jupyter contrib nbextension install --user
pip3 install jupyter_nbextensions_configurator
```

#### VS Code

需要安装完整Linux。

输入以下命令安装node、npm和cnpm。

```
apt install nodejs
apt install npm
npm install -g cnpm --registry=https://registry.npm.taobao.org
cnpm install -g npm
```

用cnpm安装wcode。

```
cnpm install -g wcode
```

使用时通过以下命令启动。

```
wcode -p 9999 /home
```

通过浏览器访问即可。

```
// 手机浏览器
localhost:9999

// 内网电脑
[手机IP地址]:9999
```

也可通过code-server。下载arm64版本的安装包，复制到手机并解压。

```
https://github.com/cdr/code-server/releases
```

解压完成后切换到该目录，并运行以下命令开启VS Code。通过浏览器访问即可，同上。

```
export PASSWORD=123456 
./code-server --auth password --port 9999
```


## Aid Learning

Termux的国内高仿版。无需安装Termux，直接下载即可使用。Aid Learning已自带VS Code。

### 环境配置

#### gfortran

Aid Learning已自带gcc/gfortran。若没有可通过以下命令安装。

```
apt install gcc
apt install gfortran
```

#### JAVA

以Java 8为例，通过以下命令安装。

```
apt install openjdk-8-jdk
apt install openjdk-8-jre
```

输入以下命令查看是否正确安装。

```
java -version
```

若出现错误，则需要修改环境变量。打开/etc/profile，添加以下内容后保存。

```
export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-arm64"
export CLASSPATH=.$JAVA_HOME/lib/
export JRE_HOME=$JAVA_HOME/jre
export PATH=$JAVA_HOME/bin:$PAT
```

#### Jupyter

Aid Learning已自带Jupyter。

##### 基本使用

可使用Jupyter Lab，通过以下命令安装和启动即可。

```
// 安装
pip3 install jupyterlab

// 启动
jupyter lab
```

也可使用Jupyter Notebook。直接运行以下命令即可。

```
jupyter notebook
```

##### Java内核

注意，Jupyter支持的JDK版本为9.0及以上，而Aid Learning内核支持的JDK为JDK8，因此在安装Java时需要留意版本。

可安装IJava支持套件。通过以下链接下载，然后复制到手机。

```
https://github.com/SpencerPark/IJava/
```

切换到文件所在位置并运行以下命令。

```
unzip ijava.zip -d ijava // 文件名为ijava.zip
cd ijava
python3 install.py --sys-prefix
```

安装完成后查看Jupyter内核，此时应当有Java。

```
jupyter kernelspec list
```

# 参考教程

## 极致安卓之—Termux安装完整版Linux

```
https://zhuanlan.zhihu.com/p/95865982
```

## 把安卓手机性能发挥到极致之-Termux

```
https://zhuanlan.zhihu.com/p/92664273
```

## 把安卓手机性能发挥到极致之-Aid Learning运行Java及性能测试

```
https://zhuanlan.zhihu.com/p/92489740
```

## 把安卓手机性能发挥到极致之-Termux安装Python及Jupyter

```
https://zhuanlan.zhihu.com/p/94203587
```

## 安卓手机C/C++开发平台

```
https://zhuanlan.zhihu.com/p/97882309
```

## 把安卓手机性能发挥到极致之-Termux/Aid Learning使用Fortran

```
https://zhuanlan.zhihu.com/p/92280533
```

## 极致安卓之—Aid Learning基于Jupyter开发Java和Python

```
https://zhuanlan.zhihu.com/p/101147592
```

## 把安卓手机性能发挥到极致之-Termux运行Java及性能测试

```
https://zhuanlan.zhihu.com/p/92471681
```

## Termux安装Node.JS及网页版文档查看与修改器wcode

```
https://zhuanlan.zhihu.com/p/106298311
```

## 极致安卓—Termux/Aid Learning安装宇宙最强VS Code

```
https://zhuanlan.zhihu.com/p/106593146
```

## Xposed下载模块89 repo.xml.gz失败的解决方法

```
https://www.52pojie.cn/forum.php?mod=viewthread&tid=1461477
```

## 教你如何简单高效的对软件(apk文件)进行修改

```
https://www.juyifx.cn/article/291723297.html?ivk_sa=1024320u
```

## 自动微信好友检测、收能量、签到打卡，不过是这款APP的冰山一角！

```
https://mp.weixin.qq.com/s/nX9oY6abpKLVk1IA_r8G2g
```

## daltonfury42 - Github gist

```
https://gist.githubusercontent.com/daltonfury42/c33fdfa7a44f261018a5d35dea7eb245/raw/5fc372ec0d36117fa3e7698d8de1952c1bac6b6a/platina.xml
```

## 抱歉，安卓有了Xposed真的可以为所欲为！

```
https://mp.weixin.qq.com/s/n7Rqn6e8zQyvghvSRJ61XA
```

## Downgrade Bootloader check fail

```
https://forum.xda-developers.com/t/downgrade-bootloader-check-fail.4111575/
```

## 一个APP安全实现微信防撤回，iOS也行？这也太强了吧！

```
https://mp.weixin.qq.com/s/w1oxB0BnhQkyhI1tW_2f6g
```

## 安卓神器的天花板Xposed，这次真的能让你用上！

```
https://mp.weixin.qq.com/s/V7mK6ahKN8rLOkvHExUpCA
```

## 大厂出品却冷门10年，免翻不限速下载国际版APP的绝佳选择！

```
https://mp.weixin.qq.com/s/8xc2uRZP2EboZz3wnR74Zw
```
