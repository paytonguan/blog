---
title: Android 虚拟机运行指南
categories: Android
abbrlink: Android-VM-Guide
date: 2020-02-17 00:00:00
tags:
---

![](topic.jpg)

在安卓手机上通过安装相应的 APP，可以运行电脑系统。

<!-- more -->

# 虚拟机

## Bochs

通过以下链接下载 Bochs 安装包和适用于 Bochs 的 XP 镜像。

```
https://pan.baidu.com/s/1i4FDLGT
http://pan.baidu.com/s/1gfj4a2Z
```

将 apk 格式的 Bochs 安装包和 img 格式的 Windows 系统镜像传送到手机，放置于任何目录下均可。安装 Bochs，将`ata0-master`前面的复选框选中，然后点击`select`。选择 img 格式的系统镜像文件 Windows.img，其他项无需更改。点击顶部的`HARDWARE`选项卡，按照以下设置。

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

点击顶部的 MISC 选项卡，将 Full screen 前面的复选框选中以使 Windows 可以全屏运行。点击右上角绿色的 Start 按钮以启动 Windows。

## Limbo

通过以下链接下载 Limbo 安装包和适用于 Limbo 的 XP 镜像。

```
http://pan.baidu.com/s/1pK7ua3D
http://pan.baidu.com/s/1miRtjJU
```

将 apk 格式的 Limbo 安装包和 qcow2 格式的 Windows 系统镜像传送到手机，放置于任何目录下均可。

安装 Limbo 并打开，加载虚拟机下选择新建，在弹出对话框中输入虚拟机名称，然后按照以下设置。

|       选项       |                   设置                  |
|------------------|-----------------------------------------|
| 用户界面项       | SDL                                     |
| CPU型号项        | athlon                                  |
| CPU核心数        | 2（可根据手机配置调整）                 |
| 运存项           | 64（可根据手机配置调整）                |
| 光驱/软盘A/软盘B | /                                       |
| 硬盘A            | 点击后选择打开，选择 qcow2 格式的镜像文件 |
| 硬盘B            | /                                       |
| 引导设备         | 硬盘                                    |
| 网络配置         | User                                    |
| 网卡             | rtl8139                                 |
| 显卡配置         | cirrus                                  |
| 声卡配置         | sb16                                    |
| 高级设置         | /                                       |
| 多线程AIO        | 勾选右侧复选框（警告可以无视）          |

全部设置完毕后，回到顶部，点击运行以启动 Windows。

## APQ

通过以下链接下载 APQ 安装包、Zarchiver 安装包和适用于 APQ 的 XP 镜像。

```
https://www.cr173.com/soft/692055.html
https://dl.pconline.com.cn/download/1815731.html
https://pan.baidu.com/s/1_aoxWWi0mfN1cXyS3V4I6g
```

获取 root 权限后打开 APQ，点击 data 分区，等待文件处理完成。

打开 zarchiver，打开 APQ/kolibri.conf，将以下位置的文件名改为下载好的镜像文件即可。

```
# 第一块硬盘，硬盘镜像文件为 xp.qcow2，应位于 SD 卡上的 APQ 主目录下或写绝对路径
-fda kolibri.img
```
