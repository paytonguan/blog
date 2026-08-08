---
title: GPU 调整指南
categories: Computer
abbrlink: Instructions-Of-GPU-Adjust
date: 2024-12-18 12:04:29
tags:
---

![](topic.jpg)

GPU 调整指南。

<!-- more -->

# 超频

## ATI Tray Tools

ATI 显卡超频工具，可用于取代 AMD 官方的 Catalyst Control Center，下载链接如下。

```
https://www.onlinedown.net/soft/28765.htm
```

点击硬件-超频设置即可对显卡进行超频。也可开启 Anti-Alisaing、Temporal AA、Adaptive anti-alisaing、Anisotropic Filtering，关闭 Enable High Quality AF、Wait for Vertical Sync、Anisotropic Filter Optimization、Trilinear Filter Optimization，调高 Texture Preference、MipMap Detail Level、Catalyst AI。

## MSI Afterburner

非 MSI 图形卡也可使用，下载链接如下。

```
https://www.msi.com/Landing/afterburner
```

打开后进入设置，点击常规，勾选解除电压调整控制和解锁电压监控控制。回到主界面后调整核心电压即可。

若希望解锁频率上限，可进入 MSI Afterburner 的安装目录，打开 MSIAfterBurner.cfg，定位到[ATIADLHAL]一项，将相关内容修改为如下，保存后重启电脑即可。

```
UnofficialOverclockingEULA = I confirm that I am aware of unofficial overclocking limitations and fully understand that MSI will not provide me any support on it.
UnofficialOverclockingMode = 1
```

## EVGA Precision X 16

用于 Nvidia 显卡，下载链接如下。

```
https://www.evga.com/precisionx/
```

打开软件后将 Power Target 和 GPU Temp Target 拉满，点亮 Overvoltage 以设置电压。

## NVIDIA Profile Inspector

修改 Nvidia 游戏配置文件，下载链接如下。

```
https://github.com/Orbmu2k/nvidiaProfileInspector
```

## GMABooster

用于 Intel 显卡，下载链接如下。打开后选择高频率即可。

```
https://www.techspot.com/downloads/4828-gmabooster.html
```

## NVIDIA nTune

用于 Nvidia 显卡，下载链接如下。安装后找到 GPU 超频选项，根据实际情况调整即可。

```
https://www.nvidia.com/en-us/drivers/ntune-5052500/
```

## ASUS GPU Tweak II

用于华硕显卡，下载链接如下。打开后直接进行参数调整即可。

```
https://www.asus.com/supportonly/GPUTweak%20II/HelpDesk_Download/
```

## SAPPHIRE TriXX

下载链接如下。打开后点击 TriXX BOOST 并配置即可。

```
https://www.sapphiretech.com/zh-cn/software
```

# 工具

## GPU-Z

查看显卡信息。

## Kombustor

显卡测试拷机工具。

## Furmark

测试显卡性能。
