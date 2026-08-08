---
title: Android 高级使用指南
categories: Android
abbrlink: Android-Advanced-Guide
date: 2020-02-21 00:00:00
tags:
---

![](topic.jpg)

Android 刷机、Root 与 ROM 相关的高级使用指南。

<!-- more -->


# 基本知识

## 恢复模式

即 Recovery Mode，该模式将会启动 recovery 分区，可在该模式下执行擦除数据等操作。通过刷入第三方 recovery 可达到更高级的功能。

对于三星设备，在关机状态下同时按音量上键+Home 键+电源键即可进入 Recovery 模式。

## 下载模式

即Download Mode，也被称为Bootloader/Fastboot Mode，用于刷机。在三星上也被称为Odin Mode。

对于三星设备，在关机状态下按音量下键+Home 键+电源键，同时将手机连接到电脑，即可进入下载模式。或先进入恢复模式，再选择 Reboot to bootloader 进入到下载模式。

## ADB工具

Google 提供的用电脑命令行对手机进行相关操作的工具，注意需要在手机上打开 USB 调试模式。注意应当用最新的 ADB 工具，否则可能会识别不到设备，运行命令行可能会提示 device offline。

下载链接如下。

```
https://developer.android.com/studio/command-line/adb
https://developer.android.google.cn/studio/releases/platform-tools.html
```

对于 Mac，可通过 Homebrew 安装。

```
brew cask install android-platform-tools
```

该工具包包含 adb 和 fastboot 两个工具，其中 adb 用于普通模式，fastboot 用于 fastboot 模式。

注意，adb 和 fastboot 需要不同的驱动。如果运行命令行时找不到设备，可能是驱动问题，可尝试下载以下驱动。

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

命令行切换到工具目录后即可使用，注意需要以管理员身份运行。adb 常用命令如下。

```
# 查看当前连接的设备
adb devices

# 让手机进入bootloader
adb reboot bootloader
```

fastboot 常用命令如下。

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

对于 Samsung，因为其使用自定义的 Odin 3 协议，因此 fastboot 命令将看不到设备。需要使用 Heimdall，仓库如下。

```
https://github.com/Benjamin-Dobell/Heimdall
```

## 解锁Bootloader

即 OEM 解锁。一般而言，进入下载模式意味着需要更改系统，此时需要先解 Bootloader 锁，否则无法完成刷入。对于三星，可在下载模式下查看 CROM SERVICE 一栏，若为 Lock 则未解锁。

对于三星设备，可在设置-开发者设置下点击 OEM 解锁。若没有该选项则可使用官方 OEM 解锁工具 CROM Service，下载链接如下。

```
https://www.apkmirror.com/apk/samsung-electronics-co-ltd/crom-service/crom-service-1-0-8-release/crom-service-1-0-8-android-apk-download/
```

对于一加设备，需要通过 ADB 工具。在手机上进入设置-关于手机，点击 7 次版本号启用开发者选项。启用开发者选项后，Google 服务框架会被自动取消隐藏。然后前往设置-系统-开发者选项，启用 OEM 解锁和高级重启。

长按电源键，从电源菜单中选择引导器模式，手机将会自动重启进入 Bootloader 模式。打开电脑的终端，使用以下命令解锁 Bootloader。

```
fastboot devices # 检查设备是否连接
fastboot oem unlock
```

手机会提示解锁 Bootloader 的风险警告信息，需要用音量键操作选择是否解锁，并使用电源键确认。解锁后手机会自动抹掉所有数据并执行重启。

## 开发者设置

在安卓设置中有一项隐藏的开发者设置，在该设置中可开启 USB 调试。一般进入`关于手机`，连续点击十次版本号，即可解除隐藏。

# Recovery

## 第三方包

第三方 Recovery 与原生 Recovery 相比，可实现更多功能。

### TWRP

```
https://twrp.me/
```

特定机型的非官方 TWRP 包如下。

```
# LG G3
https://forum.xda-developers.com/t/recovery-3-3-1-unofficial-twrp-for-lg-g3.3813552/

# Razer Phone
https://blog.lynnrin.moe/posts/Razer_Phone_TWRP/
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

以 TWRP 为例，对于官方提供支持的设备，可从 Google Play 商店中下载 TWRP 应用完成刷入。对于其它设备，可通过线刷的方式，如三星可将相应的 recovery.img 打包为 tar 后，通过 Odin 的 AP 包方式刷入。

## 机型适配

对于官方未提供支持机型的第三方包，可自行编译。

### 提取设备树

编译第三方 Recovery 需要与本设备相关的设备树。

可尝试在 Github 搜索`device [设备名]`、`kernel [设备名]`以及`vendor [设备名]`查找设备树。

<details>
<summary>【进阶】编译TWRP和Android所需设备树的不同</summary>

Android.mk、AndroidProduct.mk、omni_cheryl.mk 一般情况下相同。编译 TWRP 需要对 BoardConfig.mk 等文件进行修改。
</details>

#### 通过twrpdtgen

twrpdtgen 为 python 下的工具，可从原生 Recovery 中提取设备树，支持 Android 4.4-12。仓库及相关说明如下。

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

把提取的 recovery.img 复制到 twrpdtgen 目录下，然后运行以下命令生成设备树。

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

相当于 Makefile，定义设备的基本信息。

对于 OmniROM，文件名为`omni_<设备名>.mk`。对于 CyanogenMod 为`cm.mk`，对于魔趣为`mokee.mk`（旧版本）或`mk_<设备名>.mk`（Android 9）。统一命名为`omni_<设备名>.mk`即可。

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

只对 Android UI 起作用，对 TWRP 无效。

##### vendorsetup.sh

```
# 将Z01G修改为设备名
add_lunch_combo omni_Z01G-eng
```

##### recovery.fstab

手机分区信息。可在手机打开 Termux 等终端工具，通过 df 命令查看。

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

放置额外 property，通常不需要。

##### omni.dependencies

编译时 TWRP 会通过该文件获取额外的 source code 编译，一般为 SoC 相关的设定与核心源码。此处获取 Qualcomm 共享代码。

##### BoardConfig.mk

编译 TWRP 需要的所有参数。

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

如果有可编译的内核源码，则可使用以下参数，然后在 omni.dependencies 中加入 kernel source repository。这种方法编译出来的 recovery 将可用于各个版本的系统。

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

如果使用预编译好的内核，如从能正常运行的 boot.img 与 recovery.img 中提取出来的内核，则使用以下参数。因为已编译的内核映像无法修改，因此这种方法编译出来的 recovery 只能适用于特定版本的系统。

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

加解密的流程由 init.recovery.qcom_decrypt.rc 控制，基本上不用修改，会由 init.recovery.qcom.rc 导入。本例所用的 init.recovery.qcom.rc 可以只留下第一行 import，其余的是载入手机专用 exfat kernel module 与让 TWRP 可以刷 ASUS 原厂 ROM 的 hack。

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

在 Github 上新建仓库，将设备树的内容上传，示例如下。

```
https://github.com/guanpeiheng1/TWRP_device_samsung_j3300
```

Fork 以下仓库，然后点击 Actions-Make Recovery-Run Workflow。

```
https://github.com/azwhikaru/Action-Recovery-builder
```

LIBRARY_NAME 一般无需修改。LIBRARY_URL 根据需要编译的包类型修改，列表如下。

```
# TWRP
https://github.com/minimal-manifest-twrp/platform_manifest_twrp_omni.git

# OFRP
https://gitlab.com/OrangeFox/Manifest.git

# SHRP
https://github.com/SHRP/platform_manifest_twrp_omni.git
```

LIBRARY_BRANCH 需要跟 Android 版本匹配。如编译 TWRP 时，若 Android 版本为 7.1，则需要选择的源码分支为 twrp-7.1。选择错误的源码分支会导致无法启动到 recovery。

DEVICE_URL和DEVICE_BRANCH为刚才上传的设备树仓库地址及分支，DEVICE_PATH为`device/[厂商名]/[设备名]`，DEVICE_NAME为设备名。填写完成后运行，运行结束后即可在Release获取编译好的包。

#### 通过源码

以下均在 Linux 环境下进行。

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

开始时可先使用 minimal-manifest-twrp，该仓库为编译 TWRP 所需要的最小单位。输入以下命令以拉取代码。

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

建议拉取代码前先配置 Github 的 SSH 密钥，并输入以下命令配置以免输密码。配置完成后 pull 一次并输入密码，以后便不需要再输入。

```
git config --global credential.helper store
```

注意拉取代码需要设置为全局翻墙，图形工具可使用Clash for Windows。注意需要在`~/.bashrc`写入以下内容后重启终端，否则可能会出现curl: (35) OpenSSL SSL_connect: SSL_ERROR_SYSCALL in connection to android.googlesource.com:443错误。

```
# Clash for Windows的默认代理端口为7890
# 按照实际情况修改
ALL_PROXY=127.0.0.1:7890
```

同时需要在 hosts 文件添加规则。输入以下命令打开 hosts 文件。

```
sudo gedit /etc/hosts
```

添加以下规则。

```
[IP地址] android.googlesource.com
[IP地址] android.googlesource.com
[IP地址] storage.googleapis.com
```

IP 可在以下网站找到。

```
https://check-host.net/ip-info?host=android.googlesource.com
```

建议设置 Github 代理，以排除可能错误。

```
git config --global http.proxy 'socks5://127.0.0.1:7890'
git config --global https.proxy 'socks5://127.0.0.1:7890'
```

<details>
<summary>【进阶】不翻墙的其它方法</summary>

不建议，拉取过程中仍可能出现 gnutls_handshake() failed: Error in the pull function 错误，此时翻墙是最快捷的方法。

在终端输入以下命令打开 repo 工具的配置文件。

```
sudo gedit /usr/bin/repo
```

然后按照以下内容修改。

```
# REPO_URL = os.environ.get('REPO_URL', None)
REPO_URL = 'https://gerrit-googlesource.proxy.ustclug.org/git-repo'
```

修改后保存，然后输入以下命令关闭 git 证书认证。

```
git config --global http.sslverify false
```

还需要修复连接不上 Github 的问题。输入以下命令打开 hosts 文件。

```
sudo gedit /etc/hosts
```

在 hosts 文件中添加 Github 网址和 IP 的对应规则，可直接使用以下文件的内容。添加后保存即可。

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

编译完成后，建议在 OmniROM 环境下再次编译，但拉取 android-9.0 分支代码时提示有库不存在。可参考以下链接。

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

但由于有些故障发生在 Android 初始化程序 init 的运行过程中，而在使用调试方式构建的 ROM 中，若遇到 panic 则自动重启进入 Bootloader，阻碍内核转存日志，因此需要禁用此功能。

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

进入主菜单的 Mount，点击每个分区的选项，检查是否能够正常挂载。

##### 解密

对于高通设备，观察解密是否正常的最直接方法，是看日志中是否有解密失败（decryption failed）的提示，以及“Mount”菜单中的 data 分区是否无法挂载。

##### ADB

连接电脑，运行 adb devices，是否能检测到设备处在 Recovery 状态。

运行 adb shell 是否能进入设备上的终端，并拥有 Root 权限。

##### MTP

连接电脑，检查是否出现 MTP 设备。如未出现，在 Mount 中点击 Enable MTP。

##### OTG

2014 年起的大部分主流机型都支持 OTG。插入一根 OTG 数据线，在 TWRP 终端中运行 lsusb，看是否检测到一个 USB 设备。

再插入一个新设备，看lsusb的输出里是否多出了另外一个设备。如果插入的是U盘，还可检查/dev/block/下是否多出了设备sdx（x为任意一个小写英文字母）。

##### 备份/恢复系统

测试一下系统是否能正常备份和恢复，检查可选的备份分区是否包含必要的分区（system、data、boot、recovery）。

### 常见问题

#### 出现Recovery is not SEANDROID enforcing/set warrenty bit: recovery

若出现这几条消息，且设备陷入重启循环，则说明刷入的 recovery 与本机不匹配，拔电池或等待手机电量完全耗尽后进入下载模式并重刷即可。

若设备没有重启，由于从5.1.1版本开始的系统写保护/安全策略，出现这几条消息是正常的。Recovery is not SEANDROID enforcing来自TWRP，表示selinux处于所需状态。内核必须允许selinux宽容模式（permissive）而非强制模式（enforcing），以便授予TWRP root权限。Set Warranty Bit: recovery来自Bootloader，表示Bootloader已解锁，且设备处于第三方recovery模式。

若出现这几条消息并卡住，则说明刷入的 recovery 不匹配，可能是编译第三方 recovery 时选择的源码树不正确，重新选择与系统版本对应的源码树即可。

# Root

Root 后将无法通过 OTA 升级的方式升级系统，只能通过线刷的方式。

## 安装

### Magisk

从以下链接下载 Magisk Manager。

```
https://github.com/topjohnwu/Magisk
https://blog.lynnrin.moe/posts/magisk
```

打开 APP，如果一直在更新，则需要先翻墙。

查看设备相关信息，一般而言Ramdisk为Yes，较低版本的系统上A/B为No，SAR为No。这种情况下，可从官方ROM包中提取boot.img，以三星为例，用压缩软件打开AP包即可看到。将提取出来的包复制到手机根目录。然后APP点击安装，选择刚才复制进去的包，APP将完成修补并输出修补好的boot.img。将输出的修补包改名为boot.img，在普通设备上即可通过ADB工具包的fastboot命令刷入，对于有TWRP的也可以在recovery模式下刷入。

对于三星，需要将 boot.img 压缩为 tar，然后用 Odin 刷入，注意需要将 ROM 的所有部分都刷入，仅将 AP 包替换为 Magisk 修补过的包。

刷入后重启即完成，若 Magisk Manager 消失则重新下载即可。

若 SAR（System-as-root）为 Yes，其余一致，这种情况一般出现在三星 Android 9.0 及以上系统，修补时需要将整个 AP 包传入而不是只取 boot.img，用 Odin 刷入时需要将 ROM 的所有部分都刷入，仅将 AP 包替换为 Magisk 修补过的包。注意，将修补的 AP 包复制到电脑时，手机与电脑的连接方式不要使用 MTP，因为 MTP 会损坏大文件。

若 Ramdisk 为 No，其余一致，Magisk 只能劫持 recovery 分区，此时则需要修补 recovery.img，步骤与上面基本一致，注意修补时需要确保 Recovery Mode 为勾选状态。这种修补下正常开机 Magisk 将不运行，需要按下进入恢复模式的按键组合，等待设备屏幕亮起后松开按键，此时 Magisk 将正常运行。若需要进入到真正的恢复模式，需要在屏幕亮起时长按音量上键。这种情况下不能用第三方 recovery 安装或升级 Magisk。

### CF-Auto-Root

不推荐，在 Samsung J3300 下实测失败。

下载 CF 包，然后通过线刷工具刷入即可。对于三星设备为刷入 AP 包。

```
https://autoroot.chainfire.eu/
https://desktop.firmware.mobi/
```

### PurpleDrake

可用于 LG G3。

需要先将手机降级至 KitKat。下载以下包，将手机连接到电脑，然后按照平台执行即可。

```
https://forum.xda-developers.com/t/root-root-your-lg-g3-easily-with-purpledrake-lite-osx-linux-windows.2821000/
```

### KernelSU

一种基于内核的 Root 方案。可通过编译内核实现。

```
https://www.bilibili.com/opus/806004201667690498
```


## 软件与框架

### Busybox

提供 Android 上缺失的 Linux 命令行实现，如 dirname 等。Root 后通过以下 APP 安装即可。

```
https://github.com/meefik/busybox
```

### 文件管理

可用 Root Explorer、MT 文件管理器。

### 系统工具

#### Scene

用于锁 CPU 频率、超频等。

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

### 游戏修改

#### GameGuardian

游戏修改器。

# 系统

## 三星ROM

### 下载

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

以下适用于 J3300。

```
# 7.1.1版本
# 实测可用，推荐同版本系统下选比较新的包，成功率较高
https://samfw.com/firmware/SM-J3300/CHC/J3300ZCU2ARH1

# 全版本
https://nerdschalk.com/samsung-galaxy-j3-firmware/
https://samsungfirmware.net/samsung-galaxy-j3-pro-2017-sm-j3300-repair-firmware/
```

### 刷入

三星需要专用的线刷工具。

#### Windows

对于 Windows，需要使用 Odin，下载链接如下。

```
https://odindownload.com/
https://samsungodin.com/download/
https://www.osamsung.com/cn/
```

让手机进入下载模式，连接到电脑并打开 Odin，依次选择 ROM 包中对应的组件即可。注意 CSC 应当选择 HOME_CSC 前缀的包。如果找不到手机，需要安装驱动。

```
# 官方下载
https://developer.samsung.com/android-usb-driver

# 其它
https://www.onlinedown.net/soft/392434.htm
https://samsungusbdriver.com/
```

三星 ROM 各文件的含义如下。

| 文件前缀 |                                                        说明                                                       |
|----------|-------------------------------------------------------------------------------------------------------------------|
| CP       | Core Processor，调制解调器                                                                                        |
| AP       | Application Processor or PDA，包含 boot.img、system.img、recovery.img、hboot.nb0、data.img、cache.img、radio.img 等 |
| BL       | Bootloader                                                                                                        |
| PIT      | 分区信息表，一般不需要                                                                                            |
| CSC      | 消费者软件定制，包含特定于该地区的软件包、运营商品牌和 APN 设置                                                     |

<details>
<summary>【进阶】三星线刷包说明</summary>

每个包会写特定的部分，且为覆盖写。如果 AP 包中仅包含 recovery.img，则刷入时只会更新 Recovery 分区。

在刷第三方 recovery 时，可刷只包含 recovery.img 的 tar 格式的 AP 包，以仅更新 Recovery 分区。若 Recovery 分区损坏，可提取原版 AP 包的 recovery.img，打包为 tar 后刷入即可，无需全量刷入。

但刷入 boot 时建议要把其他部分也选上，防止只刷 AP 包导致 Odin 将 data 分区缩小。将 AP 包替换为仅包含要刷入的 boot.img 的 tar 包即可。

Odin 通过名称识别该包需要刷入的分区，因此 tar 包中需要刷入到 boot 分区的必须命名为 boot.img，刷入到 recovery 分区的必须命名为 recovery.img，否则刷入将出错。
</details>

#### Mac

对于 Mac，需使用 Heimdall。下载链接如下。

```
https://git.sr.ht/~grimler/Heimdall
https://wiki.lineageos.org/devices/ks01lte/install/#preparing-for-installation
https://blob.lineageos.org/downloads/heimdall/Heimdall-macOS-master-012220.dmg
```

使用示例如下。

```
heimdall flash --VBMETA vbmeta.img --RECOVERY recovery.img --no-reboot
```

### 参见问题

#### 刷入时提示SW REV CHECK FAIL

刷入版本低的 ROM 时会出现该错误。更新 ROM 时会同时更新 Bootloader，更新的 Bootloader 不允许刷入旧的 Bootloader。

## 氧OS

适用于一加手机。

### 下载

```
# OnePlus 7
# 国行、印度和全球版本的OnePlus 7需要刷GM57AA（美版、印版）的氧OS
# GM57BA（欧版）的由于欧洲运营商频段覆盖不同，基带存在差异
https://forum.xda-developers.com/t/oneplus-7-rom-ota-oxygen-os-repo-of-oxygen-os-builds.3937152/
```

### 刷入

刷入前需先下载好氧 OS，并解锁 Bootloader。

到 TWRP 官网下载 OnePlus 7 的 TWRP。在手机上进入设置-系统-开发者选项并启用高级重启后，长按电源键并选择 Bootloader 模式。在电脑终端使用以下指令以临时启动 TWRP。

```
fastboot boot [TWRP的img包路径]
```

正常情况下，手机会临时进入一次 TWRP。在 TWRP 中刷入氧 OS，注意刷完后先不要重启，将 TWRP 包导入手机存储，并在 TWRP 中刷入，使 TWRP 在手机中持久化。安装包会在 AB 两个分区中都安装一次 TWRP，这样无论手机从哪个分区启动都可以进入 TWRP。

若未正常进入 TWRP，由于氢氧 OS 可以无缝互刷但不能从高版本降级到低版本，若当前的氢 OS 并非最新，可直接将氧 OS 的固件导入手机存储中，直接在设置-系统-系统升级-右上角齿轮按钮-本地升级中选择氧 OS 固件以升级。

若上述方法失败，可使用氢氧互刷工具，如下。

```
http://www.oneplusbbs.com/thread-2805359-1.html
```

<details>
<summary>【进阶】</summary>
如果希望下一次通过 TWRP 刷入 ROM 时要保留所有 TWRP 和 Magisk，需要在刷入 ROM 之前尽量关闭所有 Magisk 模块，将 ROM、TWRP 包和 magisk 安装包全部导入手机存储中，按顺序刷入后重启手机。

如果希望在系统内升级并保留 TWRP 和 Magisk，需要将 ROM、TWRP 包和 magisk 安装包全部导入手机存储中，使用系统设置中的系统升级来升级系统后，在 Magisk Manager 的 Modules 页面，通过+按钮刷入 TWRP 包，然后点击 Magisk 旁边的 Install-Install-Direct Install 以及 Install-Install-Install to Inactive Slot，重启手机。
</details>

## LineageOS

CyanogenMod 的继承者。LineageOS 可在旧款 Android 手机上流畅运行。

```
https://www.lineageos.org/
```

LineageOS 不带 Google 框架，需要自行下载，链接如下。

```
https://opengapps.org/
```

### 机型适配

#### 基本知识

自行编译 ROM 必须获得内核源码、设备树和供应商文件。

其中内核源码 kernel source 需要厂商开源，如三星可在以下网站查找。

```
https://opensource.samsung.com/main
https://android.googlesource.com/kernel/samsung.git/
https://github.com/goldfish07/android_kernel_samsung_j8y18lte_old/blob/10/build_msm8917.sh
https://github.com/lifehacker-101/android_kernel_samsung_msm8937
```

设备树 device tree 示例如下。

```
https://forum.xda-developers.com/t/guide-how-to-make-a-device-tree-for-your-phone.3698419/
```

供应商文件 vendor files 示例如下。

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

供应商文件 vendor files 也可从官方固件中提取，方法如下。

```
https://xdaforums.com/t/guide-extract-vendor-from-stock-firmware.4212587/
https://baalajimaestro.me/posts/extract-vendor/
```

#### 编译

如果设备有官方维护，则直接运行以下命令即可。

```
brunch <设备代号>
```

如果设备没有官方维护，但已有非官方源码，可将非官方代码clone到本地，并放置到对应位置，一般为device(kernel, vendor)/设备厂商名/设备代号，再运行以上命令。


如果没有官方维护及非官方源码，但有其它自定义 ROM 的源码，同理将代码 clone 到对应位置，在此基础上进行修改，再运行以上命令。修改方式可参照以下链接。

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
https://github.com/shantanu-sarkar/CustomROM
https://blog.yiyitec.com/2019/08/02/译如何移植-cyanogenos-lineageos-到您自己的手机/
https://fat-tire.github.io/porting-intro.html
https://medium.com/@daltonfury42/building-lineageos-for-your-device-a7d26ab50549
https://xdaforums.com/t/guide-how-to-building-lineageos-for-an-unsupported-device.4419263/
https://blog.lynnrin.moe/posts/ROM-bringup-guide-prebuilt/
https://blog.lynnrin.moe/posts/oneplus/
https://blog.lynnrin.moe/posts/dior-rom/
```


## 其它ROM

自定义 ROM 有 LineageOS、pixel-experience、MoKee ROM、Havoc OS、Nitrogen-OS、OmniROM 等。Samsung J3300 均不支持这些自定义 ROM。

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

# 附录：刷机调试记录

以下为刷机与 Root 过程中的调试记录，含部分个人经验，仅供参考。

## Magisk Root

另外一个可能的方法：先刷入 twrp。可以用 supersu 来看是不是真的被 root 了。

Magisk 修改的是 boot 分区。进不了 boot 分区会卡在 android 的界面，进不了 system 分区会卡在开机 logo 界面。

### 相关资源

```
# Samsung fastboot临时刷入
https://android.stackexchange.com/questions/156955/samsungs-equivalent-to-fastboots-temporary-flash

# magisk文件
https://github.com/HuskyDG/magisk-files

# Magisk工作原理
https://android.stackexchange.com/questions/213167/how-does-magisk-work
https://android.stackexchange.com/questions/218222/how-to-boot-system-as-root-device-always-as-rooted
```

### 救砖模块

刷了救砖模块手机反而启动不了了。补救方式：ADB 输命令，或者进 Safe Mode（其实就是 fastboot 或者 recovery）。

```
https://sspai.com/post/57320
https://www.52pojie.cn/thread-1600094-1-1.html
https://topjohnwu.github.io/Magisk/faq.html
```

### root的另外一个可能—kernelSU

```
https://blog.csdn.net/u010783226/article/details/115752477
```

### 临时 root

```
https://forum.xda-developers.com/t/how-do-i-temporarily-root-a-samsung-device-running-android-9-without-twrp-recovery.4421255/
```

### magisk不适用于android 9

```
https://forum.xda-developers.com/t/why-magisk-doesnt-work-with-pie-roms.3848025/page-3
https://forum.xda-developers.com/t/solved-i-do-have-root-access-but-magisk-is-not-installed.3861626/
```

### Samsung J3300 Root

用最新的 magisk 26.1 也不行。

这个也不行（操作是先刷原版固件包，再刷它的 AP 包）：

```
https://support.halabtech.com/index.php?a=downloads&b=file&id=318127
https://drive.google.com/file/d/1uZRLjK4I2--h69aKJ51WS40vtqINJJ4k/view
```

j3300 android 9 SAR 是 No，要装 magisk 23 才能看到。所以应该只传 boot.img 而不是整个 AP 包，但也还是不行。

尝试安装时保留AVB 2.0/dm-verity 和 保持强制加密：

```
https://zhuanlan.zhihu.com/p/143181151
```

用AP包和用img分别测试了"都取消勾选""都勾选""只勾选保留AVB 2.0/dm-verity""只勾选保持强制加密"等组合，结果都是"可启动，无Root"。

看日志可以看到：

```
E opjohnwu.magis: Not starting debugger since process cannot load the jdwp agent
```

```
https://forum.xda-developers.com/t/magisk-general-support-discussion.3432382/page-1565
https://forum.xda-developers.com/t/magisk-general-support-discussion.3432382/page-1566
```

原因很可能是升级了 bootloader 的版本，导致 magisk 的 boot 修补失效了。可能可以尝试 magisk in recovery。

更新（2022-10-6）：magisk 25.2+boot.img 或 recovery.img 都不行，安装后用 recovery 的方式进入也不行。

```
https://github.com/topjohnwu/Magisk/issues/4495
```

尝试 22-25.2 的版本均不行。如果想用 22 以下的，注意一进去的时候要马上按安装，不然检测到有更新的话，就按不了了。但其实就算进去了也不行，会下载失败的。

尝试以下方法，还是不行：

```
https://trendyport.com/how-to-extract-boot-img-from-boot-img-iz4-and-root-samsung/?amp
```

尝试刷入后取消自动重启，自行进入 Recovery 并重置系统，还是不行。

#### Samsung J3300无法root的原因

因为固件已经是ver4，比ver1/2更严格。通过解包boot.img和system.img，可以发现在system.img中的vendor\etc\fstab.qcom，有如下内容：

```
# Android fstab file.
# The filesystem that contains the filesystem checker binary (typically /system) cannot
# specify MF_CHECK, and must come before any filesystems that do specify MF_CHECK

#TODO: Add 'check' as fs_mgr_flags with data partition.
# Currently we dont have e2fsck compiled. So fs check would failed.

#<src>                                  <mnt_point>      <type>  <mnt_flags and options>                    <fs_mgr_flags>
/dev/block/platform/soc/7824900.sdhci/by-name/userdata      /data            ext4    noatime,nosuid,nodev,barrier=1,noauto_da_alloc,discard  wait,check,forceencrypt=footer,quota
/devices/soc/7864900.sdhci/mmc_host*                        auto            vfat    defaults        voldmanaged=sdcard:auto
/devices/soc/78db000.usb/msm_hsusb_host*                   auto            auto    defaults        voldmanaged=usb:auto
/dev/block/bootdevice/by-name/hidden        /preload        ext4            defaults                voldmanaged=preload:auto
```

其中 `/dev/block/platform/soc/7824900.sdhci/by-name/userdata /data ext4 ... wait,check,forceencrypt=footer,quota` 表示系统启动时，必须检查 Data 分区是否加密，如果没有，就强制加密。

而 Magisk 只会改动 boot.img，不改动 system.img，所以解密就没解开了。

所以现在唯一的方法：
1. 解包 system.img，改掉 fstab 的 forceencrypt 和 verify 后重新打包
2. boot.img 送给 Magisk 进行修复
3. 把 boot 和 system 打包为新的 tar，作为 AP 刷入

## Heimdall用法

使用教程如下。

```
https://forum.xda-developers.com/t/guide-how-to-install-use-heimdall-and-flash-back-to-stock.1113195/
```

两个 exe 文件，heimdall 是命令行，heimdall-frontend 是用户界面方式。替换 driver 的时候，如果没有 samsung 开头的设备，就选 MSM8953（反正选错了驱动也装不上去的）。

可能需要用 1.42 版本修复 ERROR: Failed to Send Data! 问题：

```
https://www.reddit.com/r/LineageOS/comments/aymmy2/heimdall_error_failed_to_send_data/
https://github.com/tothphu/heimdall_build
```

步骤：
1. 下载 1.40 和 1.42 的 heimdall
2. 用1.40下面的Drivers/zadig，更换MSM8953的驱动
3. 命令行切换到 1.42 所在目录，运行以下命令：

```
.\heimdall.exe detect
.\heimdall.exe download-pit --output pit.pit --no-reboot
.\heimdall.exe print-pit --no-reboot
```

然后就可以将输出的分区表复制出来了，可以搜索一下相关分区。主要分区是 BOOT RECOVERY SYSTEM。flash 命令示例如下：

```
https://forum.xda-developers.com/t/tool-heimdall-1-4-rc1.2071724/
.\heimdall.exe flash --resume --BOOT boot.img
```

### Mac上的odin——heimdall

```
https://glassechidna.com.au/heimdall/#downloads
https://bitbucket.org/benjamin_dobell/heimdall/downloads/heimdall-suite-1.4.0-mac.dmg
https://formulae.brew.sh/cask/heimdall-suite
```

可用 homebrew 安装，但在 mac12 上失败，因为它试图安装到系统盘。Jodin 基于 heimdall，所以也不行。

```
https://github.com/GameTheory-/jodin3
```

## 降级bootloader

基本不可能，bootloader 禁止从高 SW REV 刷到低 SW REV。

```
https://forum.xda-developers.com/t/downgrade-bootloader-check-fail.4111575/
https://forum.xda-developers.com/t/downgrading-bootloader-in-samsung.4382225/
https://www.reddit.com/r/AndroidQuestions/comments/nnlg6f/sw_rev_check_fail_bootloader_device_7_binary_2/
https://forum.gsmhosting.com/vbb/f777/how-downgrade-binaryu8-binary-u1-2963482/
https://forum.xda-developers.com/t/guide-skipping-kg-state-prenormal-on-oneui-android-pie-9-0.3911862/
https://forum.xda-developers.com/t/is-it-possible-to-downgrade-s8-from-pie-to-oreo-sw-rev-error.4135167/
https://forum.xda-developers.com/t/method-for-everyone-who-wants-to-downgrade-galaxy-s8-to-oreo-nougat.3946240/
https://forum.xda-developers.com/t/can-i-downgrade-the-samsung-bootloader-from-sw-rev-3-to-2.3650483/
```

从这里可以看到SW REV：`https://samfw.com/firmware/SM-J3300`

BIN/BINARY是什么：`https://samfw.com/blog/what-is-bit-binary-value`

只能刷 BINARY VALUE 比当前高或相同的固件，否则会报错。假设 value 是 4，那么可以这样来搜索 root 文件：`j3300 u4 root`。

## 组合固件

新 bootloader+旧系统，可以在新 bootloader 跑旧系统。

```
https://combinationfirmware.com/downloads/j3300zcu1aqh1/
```

## 解压/打包boot.img

```
https://www.whitewinterwolf.com/posts/2016/08/11/how-to-unpack-and-edit-android-boot-img/
https://forum.xda-developers.com/t/guide-unpack-repack-boot-recovery-img-without-kitchen.2839670/
https://github.com/cfig/Android_boot_image_editor
https://github.com/GameTheory-/mktool
https://github.com/pbatard/bootimg-tools
https://stackoverflow.com/questions/57485080/stock-boot-img-not-booting-after-re-packing
https://forum.xda-developers.com/t/tool-boot-img-tools-unpack-repack-ramdisk.2319018/
```

## 解压lz4

```
https://github.com/lz4/lz4
```

## Android看系统启动log

需要 root 权限。

```
https://android.stackexchange.com/questions/26080/does-android-keep-a-log-of-when-it-boots
https://www.xda-developers.com/how-to-take-logs-android/
```

## TWRP

twrp 不能挂载分区，应该是没有解密模块。

## fastboot命令大全

```
https://www.jkmeng.cn/219.html
```

## 双系统

```
https://www.jianshu.com/p/0045d820f586
https://zhuanlan.zhihu.com/p/48520404
```

## LineageOS

需要适配的设备，才能源码编译。

```
https://www.reddit.com/r/LineageOS/comments/zs2ras/how_to_get_started_with_a_unsupported_device/
```

# 资料

> [Android内核编译](https://source.android.com/setup/build/building-kernels-deprecated?hl=zh-cn)  
> [Android固件资源站](https://androidfilehost.com/)  
> [Halium（GNU/Linux运行于Android硬件）](https://forum.renegade-project.org/t/845-windows/36)  
> [Halium 9.0 - Ubports指南](https://forum.xda-developers.com/t/halium-9-0-halium-boot-ubports-guide.4093517/)  
> [Halium文档](https://docs.halium.org/en/latest/)  
> [Halium - 简书](https://www.jianshu.com/p/9b85dce0dcb7)  
> [sspai - 73603](https://sspai.com/post/73603)  
> [sspai - 53772](https://sspai.com/post/53772)  
> [sspai - 53075](https://sspai.com/post/53075)  


# 参考教程

> [ADB出现devices offline的解决方法](https://blog.csdn.net/qq_33924155/article/details/79153000)  
> [Difference between Bootloader, Download and Recovery mode ?](https://forum.xda-developers.com/t/difference-between-bootloader-download-and-recovery-mode.3661049/)  
> [LG G3 LineageOS Tutorial](https://linustechtips.com/topic/1058206-lg-g3-lineageos-tutorial/)  
> [一加 OnePlus 7 刷入氧 OS、TWRP、Magisk (Root)](https://blog.skk.moe/post/op7-oos-twrp-magisk/)  
> [适用于任何机型的自编译 TWRP Recovery 和生成设备树](https://www.bilibili.com/video/BV12P4y1t7ZZ/)  
> [Doc: fastboot intro](https://web.archive.org/web/20161224194012/https://wiki.cyanogenmod.org/w/Doc:_fastboot_intro#.3Cwaiting_for_device.3E_errors)  
> [Samsung tablet root process](https://www.jianshu.com/p/f4f62fd2c93f)  
> [Recovery is not Seandroid Enforcing(Samsung A5)](https://android.stackexchange.com/questions/159436/recovery-is-not-seandroid-enforcingsamsung-a5s)  
> [Recovery is not SEAndroid enforcing.](https://forum.xda-developers.com/t/recovery-is-not-seandroid-enforcing.3158482/)  
> [Magisk Documentation ｜ Magisk](https://topjohnwu.github.io/Magisk/)  
> [三星手机获取 root 权限：使用 Magisk Manager 为 Android 8.0/9 创建 root 文件](https://www.cnroms.com/samsung-root-with-magisk-manager.html)  
> [SELinux 宽容模式(permissive) 强制模式(enforcing) 关闭(disabled) 几种模式之间的转换](https://blog.csdn.net/tangsilian/article/details/80144112)  
> [How to Root Samsung Galaxy J3-2017 SM-J3300 | Odin Tool](https://www.rootdroids.com/how-to-root-samsung-galaxy-j3-2017-sm-j3300-odin-tool/)  
> [如何讓 TWRP 正式支援你的裝置](https://medium.com/@shakalaca/%E5%A6%82%E4%BD%95%E8%AE%93-twrp-%E6%AD%A3%E5%BC%8F%E6%94%AF%E6%8F%B4%E4%BD%A0%E7%9A%84%E8%A3%9D%E7%BD%AE-ba942c72dfa2)  
> [常用ROM的各大官网](https://www.jianshu.com/p/88066f6c12b3)  
> [TWRP Recovery 编译适配教程（twblogs）](https://www.twblogs.net/a/5be370f62b717720b51d82d1)  
> [TWRP Recovery 编译适配教程（简书）](https://www.jianshu.com/p/912d61c4a184)  
> [Odin and Heimdall: Free Your Samsung Android](https://wrily.foad.me.uk/odin-and-heimdall-free-your-samsung-android)  
