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

# 基本知识

## 恢复模式

即Recovery Mode，该模式将会启动recovery分区，可在该模式下执行擦除数据等操作。通过刷入第三方recovery可达到更高级的功能。

对于三星设备，在关机状态下同时按音量上键+Home键+电源键即可进入Recovery模式。

## 下载模式

即Download Mode，也被称为Bootloader/Fastboot Mode，用于刷机。在三星上也被称为Odin Mode。

对于三星设备，在关机状态下按音量下键+Home键+电源键，同时将手机连接到电脑，即可进入下载模式。或先进入恢复模式，再选择Reboot to bootloader进入到下载模式。

## ADB工具

Google提供的用电脑命令行对手机进行相关操作的工具，注意需要在手机上打开USB调试模式。注意应当用最新的ADB工具，否则可能会识别不到设备，运行命令行可能会提示device offline。

下载链接如下。

```
https://developer.android.com/studio/command-line/adb
https://developer.android.google.cn/studio/releases/platform-tools.html
```

对于Mac，可通过Homebrew安装。

```
brew cask install android-platform-tools
```

该工具包包含adb和fastboot两个工具，其中adb用于普通模式，fastboot用于fastboot模式。

注意，adb和fastboot需要不同的驱动。如果运行命令行时找不到设备，可能是驱动问题，可尝试下载以下驱动。

```
# 通用驱动
https://github.com/xushuan/google_latest_usb_driver_windows
https://adb.clockworkmod.com/

# Sony
# 下载Flashtool，使用drivers目录中的自动安装程序
http://www.flashtool.net/index.php
```

若安装后仍不行，可尝试手动安装。Windows下打开设备管理器，将fastboot模式下的手机与电脑连接。检查手机是否被识别为未知设备，若是则右键选择安装驱动程序，驱动程序路径位于SDK包中的sdk/extras/google/usb_driver，SDK包下载链接如下。

```
https://developer.android.com/studio
```

也可直接使用工具和驱动一键下载脚本，链接如下。

```
https://forum.xda-developers.com/t/tool-latest-adb-fastboot-and-usb-driver-installer-tool-for-windows.3999445/
```

命令行切换到工具目录后即可使用，注意需要以管理员身份运行。adb常用命令如下。

```
# 查看当前连接的设备
adb devices

# 让手机进入bootloader
adb reboot bootloader
```

fastboot常用命令如下。

```
# 查看当前连接的设备
fastboot devices

# 刷入recovery
fastboot flash recovery recovery.img

# 刷入boot
fastboot flash boot boot.img

# 刷入cache
fastboot flash cache cache.img

# 刷入userdata
fastboot flash userdata userdata.img

# 刷入system
fastboot flash system system.img

# 临时加载镜像
fastboot boot [镜像路径]

# 重启
fastboot reboot

# 解锁OEM
# 适用于所有Google品牌的Nexus设备
fastboot oem unlock
```

对于Samsung，因为其使用自定义的Odin 3 协议，因此fastboot命令将看不到设备。需要使用Heimdall，仓库如下。

```
https://github.com/Benjamin-Dobell/Heimdall
```

## 解锁Bootloader

即OEM解锁。一般而言，进入下载模式意味着需要更改系统，此时需要先解Bootloader锁，否则无法完成刷入。对于三星，可在下载模式下查看CROM SERVICE一栏，若为Lock则未解锁。

对于三星设备，可在设置-开发者设置下点击OEM解锁。若没有该选项则可使用官方OEM解锁工具CROM Service，下载链接如下。

```
https://www.apkmirror.com/apk/samsung-electronics-co-ltd/crom-service/crom-service-1-0-8-release/crom-service-1-0-8-android-apk-download/
```

对于一加设备，需要通过ADB工具。在手机上进入设置-关于手机，点击7次版本号启用开发者选项。启用开发者选项后，Google服务框架会被自动取消隐藏。然后前往设置-系统-开发者选项，启用OEM解锁和高级重启。

长按电源键，从电源菜单中选择引导器模式，手机将会自动重启进入Bootloader模式。打开电脑的终端，使用以下命令解锁Bootloader。

```
fastboot devices # 检查设备是否连接
fastboot oem unlock
```

手机会提示解锁Bootloader的风险警告信息，需要用音量键操作选择是否解锁，并使用电源键确认。解锁后手机会自动抹掉所有数据并执行重启。

## 开发者设置

在安卓设置中有一项隐藏的开发者设置，在该设置中可开启USB调试。一般进入`关于手机`，连续点击十次版本号，即可解除隐藏。

# 资料

```
https://source.android.com/setup/build/building-kernels-deprecated?hl=zh-cn
https://androidfilehost.com/
https://forum.renegade-project.org/t/845-windows/36
https://forum.xda-developers.com/t/halium-9-0-halium-boot-ubports-guide.4093517/
https://docs.halium.org/en/latest/
https://www.jianshu.com/p/9b85dce0dcb7
```

# Recovery

## 第三方包

第三方Recovery与原生Recovery相比，可实现更多功能。

### TWRP

```
https://twrp.me/
```

特定机型的非官方TWRP包如下。

```
# LG G3
https://forum.xda-developers.com/t/recovery-3-3-1-unofficial-twrp-for-lg-g3.3813552/
```

### SHRP

```
https://skyhawkrecovery.github.io/
```

### OrangeFox

```
https://orangefox.download/zh-CN
```

### PitchBlack

```
https://pitchblackrecovery.com/
```

### CWM

支持较老的机型。

## 刷入

以TWRP为例，对于官方提供支持的设备，可从Google Play商店中下载TWRP应用完成刷入。对于其它设备，可通过线刷的方式，如三星可将相应的recovery.img打包为tar后，通过Odin的AP包方式刷入。

## 机型适配

对于官方未提供支持机型的第三方包，可自行编译。

### 提取设备树

编译第三方Recovery需要与本设备相关的设备树。

可尝试在Github搜索`device [设备名]`、`kernel [设备名]`以及`vendor [设备名]`查找设备树。

<details>
<summary>【进阶】编译TWRP和Android所需设备树的不同</summary>

Android.mk、AndroidProduct.mk、omni_cheryl.mk一般情况下相同。编译TWRP需要对BoardConfig.mk等文件进行修改。
</details>

#### 通过twrpdtgen

twrpdtgen为python下的工具，可从原生Recovery中提取设备树，支持Android 4.4-12。仓库及相关说明如下。

```
https://github.com/SebaUbuntu/TWRP-device-tree-generator
https://forum.xda-developers.com/t/script-twrp-device-tree-generator.4085463/
```

建议在Linux环境下进行。以三星为例，从ROM中的AP压缩包提取recovery.img，如果是A/B分区则用Boot.img。然后打开终端，输入以下命令。

```
# 进入root模式
sudo -i
cd /home/[用户名]

# 新建目录并安装依赖
mkdir twrpdtgen
cd twrpdtgen
apt update
apt install software-properties-common
add-apt-repository ppa:deadsnakes/ppa
apt update
apt install python3.9
update-alternatives --install /usr/bin/python python /usr/bin/python3.9 1
update-alternatives --list python
update-alternatives --config python
apt install python3-pip
pip3 install --upgrade pip

# 安装twrpdtgen
apt install cpio
pip3 install twrpdtgen
```

把提取的recovery.img复制到twrpdtgen目录下，然后运行以下命令生成设备树。

```
python3 -m twrpdtgen -k --no-git -v recovery.img
```

`output/[厂商名]/[设备名]`下的内容即为本设备的设备树。

#### 通过修改模版

下载以下仓库作为模版，以下根据该模版进行修改。

```
https://github.com/shakalaca/android_device_asus_Z01G/tree/sample
```

编译系统从build/make/core/product_config.mk开始，读取设备参数文件目录device/下所有的AndroidProducts.mk，得到设备Makefile的列表。然后在列表中搜索用户指定设备的设备Makefile即`omni_<设备名>.mk`，若能搜索到则从设备Makefile中读取设备信息，配置好编译环境。

##### Android.mk

主要进入点。

```
# 将Z01G修改为设备名
ifeq ($(TARGET_DEVICE), Z01G)
...
```

##### AndroidProducts.mk

载入`[ROM]_[设备].mk`。

```
# ROM一般为omni，无需改变
# 将Z01G修改为设备名
PRODUCT_MAKEFILES := \
    $(LOCAL_DIR)/omni_Z01G.mk
```

##### omni_Z01G.mk

相当于Makefile，定义设备的基本信息。

对于OmniROM，文件名为`omni_<设备名>.mk`。对于CyanogenMod为`cm.mk`，对于魔趣为`mokee.mk`（旧版本）或`mk_<设备名>.mk`（Android 9）。统一命名为`omni_<设备名>.mk`即可。

```
# 使当前配置文件继承其他配置文件
# 将asus修改为厂商名，Z01G修改为设备名
$(call inherit-product, device/asus/Z01G/device.mk)

# 产品名，通常格式为omni_<PRODUCT_DEVICE>
# 必须填写，用于编译系统配置环境
PRODUCT_NAME := omni_Z01G

# 设备名
# 必须填写，用于编译系统对设备名的标识
PRODUCT_DEVICE := Z01G

# 品牌名
PRODUCT_BRAND := asus

# 设备型号
# 会显示在系统设置的「关于设备」中
PRODUCT_MANUFACTURER := asus

# 厂商名
PRODUCT_MODEL := ASUS_Z01GD
```

##### device.mk

只对Android UI起作用，对TWRP无效。

##### vendorsetup.sh

```
# 将Z01G修改为设备名
add_lunch_combo omni_Z01G-eng
```

##### recovery.fstab

手机分区信息。可在手机打开Termux等终端工具，通过df命令查看。

```
# 从/system到/recovery基本可以共用

...

# 最后为SD卡和OTG
# mmcblk0p1/mmcblk0在大部分设备应为mmcblk1p1/mmcblk1（第二个mmc装置）
# ZenFone 4 Pro用UFS，所以SD卡是第一个mmc装置
/external_sd    auto      /dev/block/mmcblk0p1    /dev/block/mmcblk0         flags=display="MicroSD";storage;wipeingui;removable

# sdg1/sdg在大部分设备应为sda1/sda
/usb-otg        auto      /dev/block/sdg1         /dev/block/sdg             flags=display="USB OTG";storage;wipeingui;removable
```

##### system.prop

放置额外property，通常不需要。

##### omni.dependencies

编译时TWRP会通过该文件获取额外的source code编译，一般为SoC相关的设定与核心源码。此处获取Qualcomm共享代码。

##### BoardConfig.mk

编译TWRP需要的所有参数。

###### 基本参数

```
# 主板型号
TARGET_BOOTLOADER_BOARD_NAME := msm8998
TARGET_BOARD_PLATFORM := msm8998

# 是否排除默认USB设置
# 一般情况下设为false，且删除recovery/root/init.recovery.usb.rc与recovery/root/init.qcom.usb.sh
# 以直接使用TWRP提供的USB设置
# 对于ZenFone 4 Pro，需要取出boot.img的init.qcom.usb.sh与init.asus.usb.rc（重命名为init.recovery.usb.rc）
# 放在recovery/root，并将TW_EXCLUDE_DEFAULT_USB_INIT设为true，才可进行adb相关设置
TW_EXCLUDE_DEFAULT_USB_INIT := true
```

###### 内核打包参数

```
# 内核的运行参数
# 需要从原生boot.img中提取
BOARD_KERNEL_CMDLINE := console=ttyHSL0,115200,n8 androidboot.console=ttyHSL0 androidboot.hardware=qcom msm_rtb.filter=0x237 ehci-hcd.park=3 lpm_levels.sleep_disabled=1 androidboot.bootdevice=7824900.sdhci earlycon=msm_hsl_uart,0x78af000 androidboot.selinux=permissive
BOARD_KERNEL_CMDLINE := console=ttyMSM0,115200,n8 androidboot.console=ttyMSM0 
BOARD_KERNEL_CMDLINE += earlycon=msm_serial_dm,0xc1b0000 androidboot.hardware=qcom 
BOARD_KERNEL_CMDLINE += user_debug=31 msm_rtb.filter=0x237 ehci-hcd.park=3 
BOARD_KERNEL_CMDLINE += lpm_levels.sleep_disabled=1 sched_enable_hmp=1 sched_enable_power_aware=1 
BOARD_KERNEL_CMDLINE += service_locator.enable=1 swiotlb=2048 androidboot.configfs=true 
BOARD_KERNEL_CMDLINE += androidboot.usbcontroller=a800000.dwc3
BOARD_KERNEL_CMDLINE += androidboot.selinux=permissive

# 内核在启动映像中的基址
# 需要从原生boot.img中提取
BOARD_KERNEL_BASE := 0x00000000

# 内核的页面大小
# 需要从原生boot.img中提取
BOARD_KERNEL_PAGESIZE := 4096

# 需要传递给mkbootimg工具的额外参数
BOARD_MKBOOTIMG_ARGS := --kernel_offset 0x00008000 --ramdisk_offset 0x01000000

# 启动分区的大小
BOARD_BOOTIMAGE_PARTITION_SIZE := ...

# Recovery分区的大小
BOARD_RECOVERYIMAGE_PARTITION_SIZE := ...

# 一些特殊的设备需要用专门的mkbootimg工具来生成启动映像（如瑞芯微）
# 在这里指定该工具的路径
# 不再适用于Android 8.0及以上版本
BOARD_CUSTOM_MKBOOTIMG := ...

# 对于一些格式特殊的启动镜像，用户可以自己编写Makefile
# 在这里指定自定义的Makefile文件路径
# 不再适用于Android 8.0及以上版本
BOARD_CUSTOM_BOOTIMG_MK := ...
```

###### 内核编译参数

如果有可编译的内核源码，则可使用以下参数，然后在omni.dependencies中加入kernel source repository。这种方法编译出来的recovery将可用于各个版本的系统。

```
# 指定编译内核使用的配置文件
# 配置文件位于内核源码arch/<系统平台>/configs中
# TARGET_KERNEL_CONFIG := [kernel名]
# 
# 指定内核源码所在的目录
# TARGET_KERNEL_SOURCE := kernel/[厂商]/[设备]

# 指定内核映像名，Android编译系统根据它来查找内核映像
# 编译而成的内核映像位于内核源码arch/<系统平台>/boot中
# ARM平台为zImage，x86平台为bzImage，采用U-Boot的平台（如NXP、树莓派）为uImage
# BOARD_KERNEL_IMAGE_NAME := ...
# 
# TARGET_KERNEL_ARCH := ...
```

如果使用预编译好的内核，如从能正常运行的boot.img与recovery.img中提取出来的内核，则使用以下参数。因为已编译的内核映像无法修改，因此这种方法编译出来的recovery只能适用于特定版本的系统。

```
# TARGET_KERNEL_CONFIG := ze520kl-userdebug_defconfig
# 指定预编译内核的路径
TARGET_PREBUILT_KERNEL := device/asus/Z01G/kernel
# 指定用于Recovery的预编译内核路径
TARGET_PREBUILT_RECOVERY_KERNEL := ...
```

其它可能用到的参数如下。

```
# 指定用于编译内核的交叉工具链
# 有些设备使用Android源码自带的编译器编译的内核无法启动
# 必须使用专用或旧版本的编译器
# KERNEL_TOOLCHAIN := ...
# 
# 与KERNEL_TOOLCHAIN配合使用，指定交叉工具链的前缀
# TARGET_KERNEL_CROSS_COMPILE_PREFIX := ...
```

###### Recovery参数

```
# 指定Recovery显示的像素格式
# 不同的设备有不同的像素格式，常见的有RGB_8888、RGB_565等
# 设置不当会引起花屏、黑屏等故障
TARGET_RECOVERY_PIXEL_FORMAT := ...

# 指定Recovery分区表信息文件（fstab）的路径
# 该文件记载了可供挂载的分区信息
TARGET_RECOVERY_FSTAB := ...

# 启用滑动操作，一般启用
# 在非触屏Recovery中可以允许用户上下滑动屏幕来移动高亮选项
BOARD_RECOVERY_SWIPE := ...

# 指定设备的分辨率
DEVICE_RESOLUTION := ...

# Recovery图形显示时使用行距
# 设置不当会导致Recovery花屏
RECOVERY_GRAPHICS_USE_LINELENGTH := ...

# 设置设备是否有内置SD卡
# 现阶段的新设备均拥有至少8GB的eMMC存储，都将内置存储的/data/media/0划为内置SD卡
BOARD_HAS_SDCARD_INTERNAL := ...

# 在Recovery中，确定SD卡位于data分区
# 现阶段的新设备都将内置存储的/data/media/0划为内置SD卡
# 与上面的BOARD_HAS_SDCARD_INTERNAL呼应
RECOVERY_SDCARD_ON_DATA := ...

# 指定init.rc路径
# init.rc是Android初始化程序init最主要的脚本，起到main()函数的作用
# 该选项允许用户编写自己的init.rc，以支持各种设备平台
# 仅适用于AOSP官方Recovery，以及Android 6.0之前的旧版本Recovery（如TWRP 2.x、ClockworkMod）
# 新版本的TWRP（≥3.0）会直接忽略该选项，只使用它提供的init.rc
TARGET_RECOVERY_INITRC := ...
```

###### TWRP专用参数

```
# 指定TWRP的主题。不同的主题决定TWRP显示的不同样式，包括分辨率、屏幕方向等
# 默认可选的主题有：portrait_hdpi、portrait_mdpi、landscape_hdpi、landscape_mdpi、watch_mdpi
# 必须设置，否则编译过程中TWRP的编译规则会报错
TW_THEME := ...

# 指定电池路径，TWRP访问它以显示电池电量
# 电池路径为内核系统目录/sys中电池设备所在的路径，
# 如华为P6的路径是/sys/devices/platform/bq_bci_battery.1/power_supply/Battery
TW_CUSTOM_BATTERY_PATH := ...


# 指定亮度路径，TWRP编辑它以更改屏幕亮度
# 亮度路径为内核系统目录/sys中屏幕调节文件所在的路径
# 如华为P6的路径是/sys/devices/platform/k3_fb.1/leds/lcd_backlight0/brightness
TW_BRIGHTNESS_PATH := ...

# 指定默认亮度
# 取值范围为[0, 255]
TW_DEFAULT_BRIGHTNESS := ...

# 指定最大亮度
# 取值范围为[0, 255]
TW_MAX_BRIGHTNESS := ...

# 可能仅适用于2.x版本，在3.2.3-0版本中已经失效
TW_FLASH_FROM_STORAGE := ...

# 指定外部存储器的挂载路径
TW_EXTERNAL_STORAGE_PATH := ...

# 指定外部存储器的挂载点
TW_EXTERNAL_STORAGE_MOUNT_POINT := ...

# 指定是否将默认存储器设为外置存储
# 在3.2.3-0版本中已经失效
TW_DEFAULT_EXTERNAL_STORAGE := ...

# 指定是否不包含SuperSU
# 包含了SuperSU的TWRP会在每次重启时提示用户Root手机
TW_EXCLUDE_SUPERSU := ...

# 指定是否包含NTFS-3G模块，以支持NTFS分区
TW_INCLUDE_NTFS_3G := ...

# 指定是否忽略从Bootloader传递而来的清除data分区的指令
# misc分区存放了Bootloader传递给启动映像（boot或recovery）的指令，可以控制它们启动的行为
TW_IGNORE_MISC_WIPE_DATA := ...

# 指定是否增加额外的语言，包括中文、日本语等
# 默认情况下TWRP只会包含英语与若干欧洲语言（如德语、法语、俄语、丹麦语等）
TW_EXTRA_LANGUAGES := ...
```

###### 加密相关参数

data分区通常会加密，若TWRP没有包含相关解密工具，将无法备份或root该分区，但可以格式化。对于Qualcomm平台，加解密通过qseecomd完成。因此需要另外独立出一个目录，让TWRP编译时可以将内部/system/bin/linker64的链接改为/sbin/linker64，从而使工具在没有挂载/system时也能运行。

加解密的流程由init.recovery.qcom_decrypt.rc控制，基本上不用修改，会由init.recovery.qcom.rc导入。本例所用的init.recovery.qcom.rc可以只留下第一行import，其余的是载入手机专用exfat kernel module与让TWRP可以刷ASUS原厂ROM的hack。

会用到的so动态库则放在root/vendor/lib64目录。root/vendor/lib64/hw目录里有一个keystore.msm8998.so，中间的msm8998即为TARGET_BOARD_PLATFORM，需要按照机型进行修改。

```
# 指定TWRP是否包含加密组件，并启用加密解密功能
TW_INCLUDE_CRYPTO := ...

# 指定设备是否包含硬件加密功能
# 现阶段启用加密的设备，一般都是硬件加密
TARGET_HW_DISK_ENCRYPTION := ...

# 对于高通方案，指定在Recovery启动时是否等待高通加密服务程序qseecomd完成解密
# 必须开启，否则TWRP的解密功能形同虚设
TARGET_KEYMASTER_WAIT_FOR_QSEE := ...
```

###### 调试相关参数

```
# 指定是否在TWRP中包含logcat
TWRP_INCLUDE_LOGCAT := ...

# 指定是否在TWRP中启用日志服务logd
TARGET_USES_LOGD := ...
```

### 编译出包

#### 通过Github Actions

在Github上新建仓库，将设备树的内容上传，示例如下。

```
https://github.com/guanpeiheng1/TWRP_device_samsung_j3300
```

Fork以下仓库，然后点击Actions-Make Recovery-Run Workflow。

```
https://github.com/azwhikaru/Action-Recovery-builder
```

LIBRARY_NAME一般无需修改。LIBRARY_URL根据需要编译的包类型修改，列表如下。

```
# TWRP
https://github.com/minimal-manifest-twrp/platform_manifest_twrp_omni.git

# OFRP
https://gitlab.com/OrangeFox/Manifest.git

# SHRP
https://github.com/SHRP/platform_manifest_twrp_omni.git
```

LIBRARY_BRANCH需要跟Android版本匹配。如编译TWRP时，若Android版本为7.1，则需要选择的源码分支为twrp-7.1。选择错误的源码分支会导致无法启动到recovery。

DEVICE_URL和DEVICE_BRANCH为刚才上传的设备树仓库地址及分支，DEVICE_PATH为`device/[厂商名]/[设备名]`，DEVICE_NAME为设备名。填写完成后运行，运行结束后即可在Release获取编译好的包。

#### 通过源码

以下均在Linux环境下进行。

##### TWRP

打开终端并输入以下命令。

```
# 安装必要依赖
sudo apt install git-core gnupg flex bison gperf \
zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 \
lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache \
libgl1-mesa-dev libxml2-utils xsltproc unzip

# 安装OpenJDK
sudo apt install openjdk-8-jdk

# 安装编译套件build-essential
sudo apt install build-essential
# 若以上命令出错则使用以下命令
sudo apt install gcc g++ make

# 安装python
# 缺少python会导致repo init失败
sudo apt install python
```

打开`~/.bashrc`，在文件尾部加入以下语句以开启ccache，保存后重启终端。

```
# 启用ccache
export USE_CCACHE=1
```

开始时可先使用minimal-manifest-twrp，该仓库为编译TWRP所需要的最小单位。输入以下命令以拉取代码。

```
# 下载repo工具
sudo curl https://storage.googleapis.com/git-repo-downloads/repo > /usr/bin/repo
sudo chmod +x /usr/bin/repo

# 建立文件夹用来存放代码树
mkdir omni8_minimal
cd omni8_minimal

# 初始化源码仓库
# 可把8.1改为需要下载的Android版本号，支持4.4-9.0
repo init -u git://github.com/minimal-manifest-twrp/platform_manifest_twrp_omni.git -b twrp-8.1

# 开始同步
repo sync -f --force-sync
```

建议拉取代码前先配置Github的SSH密钥，并输入以下命令配置以免输密码。配置完成后pull一次并输入密码，以后便不需要再输入。

```
git config --global credential.helper store
```

注意拉取代码需要设置为全局翻墙，图形工具可使用Clash for Windows。注意需要在`~/.bashrc`写入以下内容后重启终端，否则可能会出现curl: (35) OpenSSL SSL_connect: SSL_ERROR_SYSCALL in connection to android.googlesource.com:443错误。

```
# Clash for Windows的默认代理端口为7890
# 按照实际情况修改
ALL_PROXY=127.0.0.1:7890
```

同时需要在hosts文件添加规则。输入以下命令打开hosts文件。

```
sudo gedit /etc/hosts
```

添加以下规则。

```
[IP地址] android.googlesource.com
[IP地址] android.googlesource.com
[IP地址] storage.googleapis.com
```

IP可在以下网站找到。

```
https://check-host.net/ip-info?host=android.googlesource.com
```

建议设置Github代理，以排除可能错误。

```
git config --global http.proxy 'socks5://127.0.0.1:7890'
git config --global https.proxy 'socks5://127.0.0.1:7890'
```

<details>
<summary>【进阶】不翻墙的其它方法</summary>

不建议，拉取过程中仍可能出现gnutls_handshake() failed: Error in the pull function错误，此时翻墙是最快捷的方法。

在终端输入以下命令打开repo工具的配置文件。

```
sudo gedit /usr/bin/repo
```

然后按照以下内容修改。

```
# REPO_URL = os.environ.get('REPO_URL', None)
REPO_URL = 'https://gerrit-googlesource.proxy.ustclug.org/git-repo'
```

修改后保存，然后输入以下命令关闭git证书认证。

```
git config --global http.sslverify false
```

还需要修复连接不上Github的问题。输入以下命令打开hosts文件。

```
sudo gedit /etc/hosts
```

在hosts文件中添加Github网址和IP的对应规则，可直接使用以下文件的内容。添加后保存即可。

```
https://cdn.jsdelivr.net/gh/ineo6/hosts/hosts
```
</details>

将刚才设备树的文件复制到对应位置，然后通过以下命令执行编译。编译而成的Recovery位于`out/target/product/[设备名]/recovery.img`。

```
source build/envsetup.sh
lunch
make recoveryimage
```

编译完成后，建议在OmniROM环境下再次编译，但拉取android-9.0分支代码时提示有库不存在。可参考以下链接。

```
# TWRP
https://forum.xda-developers.com/t/dev-how-to-compile-twrp-touch-recovery.1943625/
https://zhuanlan.zhihu.com/p/292939576
https://www.cnblogs.com/iJessie/p/6522748.html
https://cofface.com/archives/1730.html
https://baolong24.top/2020/12/03/Razer_Phone_TWRP/

# SHRP
https://forum.xda-developers.com/t/recovery-official-sky-hawk-recovery-project-v3-1-05-04-2021.4258111/
```

### 调试查错

#### 提取日志

若recovery不能正常启动，可查看内核日志。高通和瑞芯微（如RK3188）的内核日志存储在/proc/last_kmsg，海思早期芯片（如K3V2）存储在内核参数CONFIG_APANIC_PLABEL所指定的分区中，一般是splash。

但由于有些故障发生在Android初始化程序init的运行过程中，而在使用调试方式构建的ROM中，若遇到panic则自动重启进入Bootloader，阻碍内核转存日志，因此需要禁用此功能。

将以下代码保存为system/core/init/no_reboot_into_bootloader.diff。

```
diff --git a/init/Android.bp b/init/Android.bp
index 45cf327f8..8a16fb2d2 100644
--- a/init/Android.bp
+++ b/init/Android.bp
@@ -14,6 +14,9 @@
 // limitations under the License.
 //
 
+// AnClark modify: I want to read logs. If handles panic as rebooting into bootloader, I won't get any logs!
+// Force setting DREBOOT_BOOTLOADER_ON_PANIC=0 on product_variables. 
+
 cc_defaults {
     name: "init_defaults",
     cpp_std: "experimental",
@@ -41,7 +44,7 @@ cc_defaults {
                 "-UALLOW_PERMISSIVE_SELINUX",
                 "-DALLOW_PERMISSIVE_SELINUX=1",
                 "-UREBOOT_BOOTLOADER_ON_PANIC",
-                "-DREBOOT_BOOTLOADER_ON_PANIC=1",
+                "-DREBOOT_BOOTLOADER_ON_PANIC=0",
                 "-UWORLD_WRITABLE_KMSG",
                 "-DWORLD_WRITABLE_KMSG=1",
                 "-UDUMP_ON_UMOUNT_FAILURE",
diff --git a/init/Android.mk b/init/Android.mk
index f1fe5168b..d28b4e489 100644
--- a/init/Android.mk
+++ b/init/Android.mk
@@ -5,10 +5,12 @@ LOCAL_PATH:= $(call my-dir)
 # --
 
 ifneq (,$(filter userdebug eng,$(TARGET_BUILD_VARIANT)))
+# AnClark modify: I want to read logs. If handles panic as rebooting into bootloader, I won't get any logs!
+# Force setting DREBOOT_BOOTLOADER_ON_PANIC=0 on product_variables. 
 init_options += \
     -DALLOW_LOCAL_PROP_OVERRIDE=1 \
     -DALLOW_PERMISSIVE_SELINUX=1 \
-    -DREBOOT_BOOTLOADER_ON_PANIC=1 \
+    -DREBOOT_BOOTLOADER_ON_PANIC=0 \
     -DDUMP_ON_UMOUNT_FAILURE=1
 else
 init_options += \
diff --git a/init/util.cpp b/init/util.cpp
index fdcb22d1c..a468082af 100644
--- a/init/util.cpp
+++ b/init/util.cpp
@@ -370,11 +370,19 @@ bool expand_props(const std::string& src, std::string* dst) {
     return true;
 }
 
+// AnClark MODIFY: Use abort() instead of rebooting into BL to trigger panic.
+/**
 void panic() {
     LOG(ERROR) << "panic: rebooting to bootloader";
     // Do not queue "shutdown" trigger since we want to shutdown immediately
     DoReboot(ANDROID_RB_RESTART2, "reboot", "bootloader", false);
 }
+**/
+
+void panic() {
+   LOG(ERROR) << "android::init::panic() invoked. Abort init to trigger kernel panic!";
+   abort();
+}
 
 static std::string init_android_dt_dir() {
     // Use the standard procfs-based path by default
```

打开init所在的目录system/core/init，运行以下命令以应用补丁即可。

```
patch -p2 < no_reboot_into_bootloader.diff
```

#### 功能测试

可检查以下项目是否正常。

##### 挂载

进入主菜单的Mount，点击每个分区的选项，检查是否能够正常挂载。

##### 解密

对于高通设备，观察解密是否正常的最直接方法，是看日志中是否有解密失败（decryption failed）的提示，以及“Mount”菜单中的data分区是否无法挂载。

##### ADB

连接电脑，运行adb devices，是否能检测到设备处在Recovery状态。

运行adb shell是否能进入设备上的终端，并拥有Root权限。

##### MTP

连接电脑，检查是否出现MTP设备。如未出现，在Mount中点击Enable MTP。

##### OTG

2014年起的大部分主流机型都支持OTG。插入一根OTG数据线，在TWRP终端中运行lsusb，看是否检测到一个USB设备。

再插入一个新设备，看lsusb的输出里是否多出了另外一个设备。如果插入的是U盘，还可检查/dev/block/下是否多出了设备sdx（x为任意一个小写英文字母）。

##### 备份/恢复系统

测试一下系统是否能正常备份和恢复，检查可选的备份分区是否包含必要的分区（system、data、boot、recovery）。

### 常见问题

#### 出现Recovery is not SEANDROID enforcing/set warrenty bit: recovery

若出现这几条消息，且设备陷入重启循环，则说明刷入的recovery与本机不匹配，拔电池或等待手机电量完全耗尽后进入下载模式并重刷即可。

若设备没有重启，由于从5.1.1版本开始的系统写保护/安全策略，出现这几条消息是正常的。Recovery is not SEANDROID enforcing来自TWRP，表示selinux处于所需状态。内核必须允许selinux宽容模式（permissive）而非强制模式（enforcing），以便授予TWRP root权限。Set Warranty Bit: recovery来自Bootloader，表示Bootloader已解锁，且设备处于第三方recovery模式。

若出现这几条消息并卡住，则说明刷入的recovery不匹配，可能是编译第三方recovery时选择的源码树不正确，重新选择与系统版本对应的源码树即可。

# 系统

Android可以随意升降级。

## 官方ROM

### 三星

#### 下载

可在以下网站下载固件。

```
# 可查看和下载固件
https://galaxyfirmware.com/
https://romsamsung.com/

# 可查看，下载固件需要注册或会员
https://updato.com/
https://www.sammobile.com/firmwares/
https://www.sxrom.com/

# 下载脚本
https://github.com/nlscc/samloader
https://github.com/jesec/samfirm.js
https://github.com/SlackingVeteran/frija
```

以下适用于J3300。

```
# 7.1.1版本
# 实测可用，推荐同版本系统下选比较新的包，成功率较高
https://samfw.com/firmware/SM-J3300/CHC/J3300ZCU2ARH1

# 全版本
https://nerdschalk.com/samsung-galaxy-j3-firmware/
https://samsungfirmware.net/samsung-galaxy-j3-pro-2017-sm-j3300-repair-firmware/
```

#### 刷入

三星需要专用的线刷工具Odin，下载链接如下。

```
https://odindownload.com/
https://samsungodin.com/download/
https://www.osamsung.com/cn/
```

让手机进入下载模式，连接到电脑并打开Odin，依次选择ROM包中对应的组件即可。注意CSC应当选择HOME_CSC前缀的包。如果找不到手机，需要安装驱动。

```
# 官方下载
https://developer.samsung.com/android-usb-driver

# 其它
https://www.onlinedown.net/soft/392434.htm
https://samsungusbdriver.com/
```

三星ROM各文件的含义如下。

| 文件前缀 |                                                        说明                                                       |
|----------|-------------------------------------------------------------------------------------------------------------------|
| CP       | Core Processor，调制解调器                                                                                        |
| AP       | Application Processor or PDA，包含boot.img、system.img、recovery.img、hboot.nb0、data.img、cache.img、radio.img等 |
| BL       | Bootloader                                                                                                        |
| PIT      | 分区信息表，一般不需要                                                                                            |
| CSC      | 消费者软件定制，包含特定于该地区的软件包、运营商品牌和APN设置                                                     |

<details>
<summary>【进阶】三星线刷包说明</summary>

每个包会写特定的部分，且为覆盖写。如果AP包中仅包含recovery.img，则刷入时只会更新Recovery分区。

在刷第三方recovery时，可刷只包含recovery.img的tar格式的AP包，以仅更新Recovery分区。若Recovery分区损坏，可提取原版AP包的recovery.img，打包为tar后刷入即可，无需全量刷入。

但刷入boot时建议要把其他部分也选上，防止只刷AP包导致Odin将data分区缩小。将AP包替换为仅包含要刷入的boot.img的tar包即可。

Odin通过名称识别该包需要刷入的分区，因此tar包中需要刷入到boot分区的必须命名为boot.img，刷入到recovery分区的必须命名为recovery.img，否则刷入将出错。
</details>

#### 参见问题

##### 刷入时提示SW REV CHECK FAIL

刷入版本低的ROM时会出现该错误。更新ROM时会同时更新Bootloader，更新的Bootloader不允许刷入旧的Bootloader。

### 氧OS

适用于一加手机。

#### 下载

```
# OnePlus 7
# 国行、印度和全球版本的OnePlus 7需要刷GM57AA（美版、印版）的氧OS
# GM57BA（欧版）的由于欧洲运营商频段覆盖不同，基带存在差异
https://forum.xda-developers.com/t/oneplus-7-rom-ota-oxygen-os-repo-of-oxygen-os-builds.3937152/
```

#### 刷入

刷入前需先下载好氧OS，并解锁Bootloader。

到TWRP官网下载OnePlus 7的TWRP。在手机上进入设置-系统-开发者选项并启用高级重启后，长按电源键并选择Bootloader模式。在电脑终端使用以下指令以临时启动TWRP。

```
fastboot boot [TWRP的img包路径]
```

正常情况下，手机会临时进入一次TWRP。在TWRP中刷入氧OS，注意刷完后先不要重启，将TWRP包导入手机存储，并在TWRP中刷入，使TWRP在手机中持久化。安装包会在AB两个分区中都安装一次TWRP，这样无论手机从哪个分区启动都可以进入TWRP。

若未正常进入TWRP，由于氢氧OS可以无缝互刷但不能从高版本降级到低版本，若当前的氢OS并非最新，可直接将氧OS的固件导入手机存储中，直接在设置-系统-系统升级-右上角齿轮按钮-本地升级中选择氧OS固件以升级。

若上述方法失败，可使用氢氧互刷工具，如下。

```
http://www.oneplusbbs.com/thread-2805359-1.html
```

<details>
<summary>【进阶】</summary>
如果希望下一次通过TWRP刷入ROM时要保留所有TWRP和Magisk，需要在刷入ROM之前尽量关闭所有Magisk 模块，将ROM、TWRP包和magisk安装包全部导入手机存储中，按顺序刷入后重启手机。

如果希望在系统内升级并保留TWRP和Magisk，需要将ROM、TWRP包和magisk安装包全部导入手机存储中，使用系统设置中的系统升级来升级系统后，在Magisk Manager的Modules页面，通过+按钮刷入TWRP包，然后点击Magisk旁边的Install-Install-Direct Install以及Install-Install-Install to Inactive Slot，重启手机。
</details>

## 自定义ROM

自定义ROM有LineageOS、pixel-experience、MoKee ROM、Havoc OS、Nitrogen-OS、OmniROM等。Samsung J3300均不支持这些自定义ROM。

使用自定义ROM前一般均需对手机进行Root，解锁Bootloader，刷入TWRP。刷入自定义ROM前一般需要擦除数据，以TWRP为例，进入TWRP recovery后，选择Wipe-Advanced Wipe，勾选Dalvik/ART Cache、Cache、Data、Internal Storage、System以完成擦除。

```
# PixelExperience
https://download.pixelexperience.org/

# OmniROM
https://omnirom.org/

# Nitrogen OS
https://theunlockr.com/rom/nitrogen-os-rom/

# AOSPEXTENDED ROM
https://www.aospextended.com/

# dotOS
https://droidontime.com/

# Resurrection Remix OS
https://resurrectionremix.com/

# MoKee
https://download.mokeedev.com/
```

### LineageOS

CyanogenMod的继承者。LineageOS可在旧款Android手机上流畅运行。

```
https://www.lineageos.org/
```

LineageOS不带Google框架，需要自行下载，链接如下。

```
https://opengapps.org/
```

## 机型适配

自行编译ROM必须获得内核源码、设备树和供应商文件。

其中内核源码kernel source需要厂商开源，如三星可在以下网站查找。

```
https://opensource.samsung.com/main
```

设备树device tree示例如下。

```
https://forum.xda-developers.com/t/guide-how-to-make-a-device-tree-for-your-phone.3698419/
```

供应商文件vendor files示例如下。

```
<?xml version="1.0" encoding="UTF-8"?>

<manifest>

    <remote name="vendor"
            fetch="https://github.com"
            revision="master" />
    
    <remote name="device"
            fetch="https://github.com"
            revision="lineage-16.0"/>

    <remote name="kernel"
            fetch="https://github.com"
            revision="nitrogen-p-oss"/>

    <!--Device Trees-->
    <project path="device/xiaomi/platina" name="nysascape/device_xiaomi_platina" remote="device" />


    <!--Kernel-->
    <project path="kernel/xiaomi/platina" name="MiCode/Xiaomi_Kernel_OpenSource" remote="kernel" />



    <!-- Vendor folders -->
    <project path="vendor/xiaomi/platina" name="nysascape/vendor_xiaomi_platina" remote="vendor" />
</manifest>
```

### LineageOS

如果设备有官方维护，则直接运行以下命令即可。

```
brunch <设备代号>
```

如果设备没有官方维护，但已有非官方源码，可将非官方代码clone到本地，并放置到对应位置，一般为device(kernel, vendor)/设备厂商名/设备代号，再运行以上命令。


如果没有官方维护及非官方源码，但有其它自定义ROM的源码，同理将代码clone到对应位置，在此基础上进行修改，再运行以上命令。修改方式可参照以下链接。

```
https://github.com/GuaiYiHu/android_device_xiaomi_clover-oss/commit/c9426d95559576e61f7544443a24df42786b7aab
```

参考教程如下。

```
https://www.link-nemo.com/u/10156/post/260119
https://www.jianshu.com/p/19cb9c08ee51
https://android.stackexchange.com/questions/216435/how-to-build-lineageos-for-a-device-without-official-support
https://forum.xda-developers.com/t/guide-build-lineageos-how-to-use-github.3551484/
https://forum.xda-developers.com/t/guide-how-to-build-an-unsupported-rom-using-sources-from-other-roms.3844972/
https://www.cnblogs.com/luoyesiqiu/p/10701419.html
https://acytoo.com/ladder/build-lineageos-for-tab-s/
https://mystery00.github.io/2017/04/23/%E7%BC%96%E8%AF%91LineageOS/
```

# Root

Root后将无法通过OTA升级的方式升级系统，只能通过线刷的方式。

## Magisk

### 安装

从以下链接下载Magisk Manager。

```
https://github.com/topjohnwu/Magisk
```

打开APP，如果一直在更新，则需要先翻墙。

查看设备相关信息，一般而言Ramdisk为Yes，较低版本的系统上A/B为No，SAR为No。这种情况下，可从官方ROM包中提取boot.img，以三星为例，用压缩软件打开AP包即可看到。将提取出来的包复制到手机根目录。然后APP点击安装，选择刚才复制进去的包，APP将完成修补并输出修补好的boot.img。将输出的修补包改名为boot.img，在普通设备上即可通过ADB工具包的fastboot命令刷入，对于有TWRP的也可以在recovery模式下刷入。

对于三星，需要将boot.img压缩为tar，然后用Odin刷入，注意需要将ROM的所有部分都刷入，仅将AP包替换为Magisk修补过的包。

刷入后重启即完成，若Magisk Manager消失则重新下载即可。

若SAR（System-as-root）为Yes，其余一致，这种情况一般出现在三星Android 9.0及以上系统，修补时需要将整个AP包传入而不是只取boot.img，用Odin刷入时需要将ROM的所有部分都刷入，仅将AP包替换为Magisk修补过的包。注意，将修补的AP包复制到电脑时，手机与电脑的连接方式不要使用MTP，因为MTP会损坏大文件。

若Ramdisk为No，其余一致，Magisk只能劫持recovery分区，此时则需要修补recovery.img，步骤与上面基本一致，注意修补时需要确保Recovery Mode为勾选状态。这种修补下正常开机Magisk将不运行，需要按下进入恢复模式的按键组合，等待设备屏幕亮起后松开按键，此时Magisk将正常运行。若需要进入到真正的恢复模式，需要在屏幕亮起时长按音量上键。这种情况下不能用第三方recovery安装或升级Magisk。

### 模块



## 其它方法

### 通过CF-Auto-Root

不推荐，在Samsung J3300下实测失败。

下载CF包，然后通过线刷工具刷入即可。对于三星设备为刷入AP包。

```
https://autoroot.chainfire.eu/
https://desktop.firmware.mobi/
```

### 通过工具

#### PurpleDrake

可用于LG G3。

需要先将手机降级至KitKat。下载以下包，将手机连接到电脑，然后按照平台执行即可。

```
https://forum.xda-developers.com/t/root-root-your-lg-g3-easily-with-purpledrake-lite-osx-linux-windows.2821000/
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

## ADB出现devices offline的解决方法

```
https://blog.csdn.net/qq_33924155/article/details/79153000
```

## Difference between Bootloader, Download and Recovery mode ?

```
https://forum.xda-developers.com/t/difference-between-bootloader-download-and-recovery-mode.3661049/
```

## LG G3 LineageOS Tutorial

```
https://linustechtips.com/topic/1058206-lg-g3-lineageos-tutorial/
```

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

## 一加 OnePlus 7 刷入氧 OS、TWRP、Magisk (Root)

```
https://blog.skk.moe/post/op7-oos-twrp-magisk/
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

## 适用于任何机型的自编译 TWRP Recovery 和生成设备树

```
https://www.bilibili.com/video/BV12P4y1t7ZZ/
```

## Doc: fastboot intro

```
https://web.archive.org/web/20161224194012/https://wiki.cyanogenmod.org/w/Doc:_fastboot_intro#.3Cwaiting_for_device.3E_errors
```

## Samsung tablet root process

```
https://www.jianshu.com/p/f4f62fd2c93f
```

## Recovery is not Seandroid Enforcing(Samsung A5)

```
https://android.stackexchange.com/questions/159436/recovery-is-not-seandroid-enforcingsamsung-a5s
```

## Recovery is not SEAndroid enforcing.

```
https://forum.xda-developers.com/t/recovery-is-not-seandroid-enforcing.3158482/
```

## Magisk Documentation ｜ Magisk

```
https://topjohnwu.github.io/Magisk/
```

## 三星手机获取 root 权限：使用 Magisk Manager 为 Android 8.0/9 创建 root 文件

```
https://www.cnroms.com/samsung-root-with-magisk-manager.html
```

## SELinux 宽容模式(permissive) 强制模式(enforcing) 关闭(disabled) 几种模式之间的转换

```
https://blog.csdn.net/tangsilian/article/details/80144112
```

## How to Root Samsung Galaxy J3-2017 SM-J3300 | Odin Tool

```
https://www.rootdroids.com/how-to-root-samsung-galaxy-j3-2017-sm-j3300-odin-tool/
```

## 如何讓 TWRP 正式支援你的裝置

```
https://medium.com/@shakalaca/%E5%A6%82%E4%BD%95%E8%AE%93-twrp-%E6%AD%A3%E5%BC%8F%E6%94%AF%E6%8F%B4%E4%BD%A0%E7%9A%84%E8%A3%9D%E7%BD%AE-ba942c72dfa2
```

## 常用ROM的各大官网

```
https://www.jianshu.com/p/88066f6c12b3
```

## TWRP Recovery 编译适配教程

```
https://www.twblogs.net/a/5be370f62b717720b51d82d1
https://www.jianshu.com/p/912d61c4a184
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
