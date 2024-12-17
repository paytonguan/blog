---
title: AMD安装黑苹果指南
categories: Mac
abbrlink: AMD-Hackintosh-Guide
date: 2019-12-01 09:55:29
tags:
---

![](topic.jpg)

AMD安装黑苹果指南。

<!-- more -->

# 内核处理

需要对系统内核进行处理后，才可在AMD CPU上正常运行。

## 内核修补补丁

适用的CPU如下，可用于10.13及以上系统。

|   家族   |    代号   |                         示例                         |
|----------|-----------|------------------------------------------------------|
| 15h      | Bulldozer | FX Series                                            |
| 16h      | Jaguar    | A Series（包括AM4 A-Series）                         |
| 17h和19h | Zen       | Ryzen, 1st, 2nd + 3rd Gen Threadripper, Athlon 2xxGE |

在OpenCore配置文件中合并以下补丁即可。

```
# 15h/16h
https://github.com/AMD-OSX/AMD_Vanilla/blob/opencore/15h_16h/patches.plist

# 17h/19h
https://github.com/AMD-OSX/AMD_Vanilla/blob/opencore/17h_19h/patches.plist
```

## 内核替换

对于32位系统，需要进行内核替换，下载链接如下。注意替换内核后将失去iMessage支持。

```
https://wiki.osx86project.org/wiki/index.php/Patched_Kernels
```

## 驱动修补

对于10.13，可能需要以下kext。

```
https://github.com/amd-osx-kb/HighSierraLegacy/tree/master/files
```

列表如下。

```
AppleActuatorDriver.kext
AppleSMCRTC.kext
AppleUSBCommon.kext
IOSlaveProcessor.kext
KernelRelayHost.kext
IONetworkingFamily.kext（在10.13.3+上应使用10.13.3版）
IOUSBFamily.kext
```

kexts作用如下。

|            名称            |       作用      |
|----------------------------|-----------------|
| AMDRyzenCPUPowerManagement | AMD CPU电源管理 |
| SMCAMDProcessor            | AMD CPU监测     |
| SMCBatteryManager          | AMD CPU电池修复 |

## USB

### FX

需要使用DummyUSBEHCIPCI和DummyUSBXHCIPCI，下载链接如下。

```
https://github.com/amd-osx-kb/HighSierraLegacy/blob/master/files/DummyUSB.zip
```

### Ryzen

仅适用于10.13。打开DSDT并添加以下源。

```
# Ryzen USB
https://raw.githubusercontent.com/AlGreyy/Ryzen-USB-fix-/master
```

点击Patch，使用USB Ryzen补丁。保存后将DSDT.aml放到Clover的ACPI部分，然后需要在配置文件中添加以下kext补丁。

对于10.13.1-10.13.3，内容如下。

```
Name: AppleUSBXHCI
Find: 21F281FA 000002
Replace: 21F281FA 000011
Comment: ydeng USB patch

Name: AppleUSBXHCI
Find: D1000000 83F901
Replace: D1000000 83F910
Comment: ydeng USB patch

Name: AppleUSBXHCI
Find: 83BD7CFF FFFF0F
Replace: 83BD7CFF FFFF1F
Comment: ydeng USB patch
```

对于10.13.4-10.13.6，内容如下。

```
Name: AppleUSBXHCI
Find: C8000000 83FB02
Replace: C8000000 83FB11
Comment: algrey USB patch for ryzen
```

# SSE4.2仿真

对于AMD CPU，可用MouSSE.kext在SSE4.1 CPU下运行要求为SSE 4.2的系统，下载链接如下。

```
https://forums.macrumors.com/threads/mp3-1-others-sse-4-2-emulation-to-enable-amd-metal-driver.2206682/
```

# 支持性

## USB

原生USB受到支持。

## 音频与麦克风

原生音频受到支持。

G系列APU上音频问题无法修复，必须使用外部DAC。

麦克风支持仅限于Ryzen上的VoodooHDA，不支持15/16H CPU的麦克风。

## IOMMU

IOMMU不工作。

## 三码

iCloud、iMessage、FaceTime、Siri可用。

## 显卡

内置显卡（Ax CPU/G Ryzen）无法工作。

## Adobe

Adobe从2019年开始大部分应用都高度依赖基于Intel的特殊数学函数库Intel Math Kernel Library（Intel-mkl），以及其它一些Intel特殊指令集用于硬件加速，涉及到的有MXMCore、FastCore、CameraRAW等。因此在AMD处理器上安装黑苹果，使用Photoshop的某些功能会导致闪退甚至Kernel Panic。

可通过禁用诸如RAW支持之类的功能以避免崩溃。打开终端并输入以下代码。

```
for file in MMXCore FastCore TextModel libiomp5.dylib; do
    find /Applications/Adobe* -type f -name $file | while read -r FILE; do
        sudo -v
        echo "found $FILE"
        [[ ! -f ${FILE}.back ]] && sudo cp -f $FILE ${FILE}.back || sudo cp -f ${FILE}.back $FILE
        echo $FILE | grep libiomp5 >/dev/null
        if [[ $? == 0 ]]; then
            dir=$(dirname "$FILE")
            [[ ! -f ${HOME}/libiomp5.dylib ]] && cd $HOME && curl -sO https://excellmedia.dl.sourceforge.net/project/badgui2/libs/mac64/libiomp5.dylib
            echo -n "replacing " && sudo cp -vf ${HOME}/libiomp5.dylib $dir && echo
            rm -f ${HOME}/libiomp5.dylib
            continue
        fi
        echo $FILE | grep TextModel >/dev/null
        [[ $? == 0 ]] && echo "emptying $FILE" && sudo echo -n >$FILE && continue
        echo "patching $FILE \n"
        sudo perl -i -pe 's|\x90\x90\x90\x90\x56\xE8\x6A\x00|\x90\x90\x90\x90\x56\xE8\x3A\x00|sg' $FILE
        sudo perl -i -pe 's|\x90\x90\x90\x90\x56\xE8\x4A\x00|\x90\x90\x90\x90\x56\xE8\x1A\x00|sg' $FILE
    done
done
```

然后输入以下代码，重启即可。

```
[ ! -d $HOME/Library/LaunchAgents ] && mkdir $HOME/Library/LaunchAgents
AGENT=$HOME/Library/LaunchAgents/environment.plist
sysctl -n machdep.cpu.brand_string | grep FX >/dev/null 2>&1
x=$(echo $(($? != 0 ? 5 : 4)))
cat >$AGENT <<EOF
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
 <key>Label</key>
 <string>mkl-debug</string>
 <key>ProgramArguments</key>
 <array>
 <string>sh</string>
 <string>-c</string>
    <string>launchctl setenv MKL_DEBUG_CPU_TYPE $x;</string>
 </array>
 <key>RunAtLoad</key>
 <true/>
</dict>
</plist>
EOF
launchctl load ${AGENT} >/dev/null 2>&1
launchctl start ${AGENT} >/dev/null 2>&1
```

若希望撤销操作，则输入以下代码。

```
for file in MMXCore FastCore TextModel libiomp5.dylib; do
    find /Applications/Adobe* -type f -name $file | while read -r FILE; do
        sudo -v
        [[ -f ${FILE}.back ]] && echo "found backup $FILE" && sudo mv -f ${FILE}.back $FILE
    done
done

AGENT=$HOME/Library/LaunchAgents/environment.plist
if [[ -f $AGENT ]]; then
    launchctl unload ${AGENT} >/dev/null 2>&1
    launchctl stop ${AGENT} >/dev/null 2>&1
    rm -rf $AGENT
fi
```

## Matlab

AMD黑苹果无法运行MatLab。即使能够运行，运算速度也非常缓慢，原因是缺乏Intel-mkl。

## 音频软件

Cubase、REAPER、Waves插件等在启动时崩溃。使用Clang构建的REAPER可以工作。

## 32/64位

不支持32位指令。

## CPU电源管理

不可设置CPU电源管理。

## 虚拟机

由于使用AMD CPU需要注释XNU内核对Intel特有指令集的调用，因此VMX不受支持，AppleHV框架不可用，VMWare、Parallels、Docker、Android Studios等虚拟机软件均无法使用，只能使用VirtualBox，或某些虚拟机的特定版本，如VMware 10、Parallels 13.1.0。

## XCode

XCode的Apple Watch在Catalina中损坏，在Mojave中正常。

# 常见问题

## 啰嗦模式

### 出现AppleIntelMCEReporter报错

双插槽支持被打破，受影响的SMBIOS包括MacPro6,1、MacPro7,1、iMacPro1,1。在引导器放置AppleMCEReporterDisabler.kext即可，需要10.15及更高版本。

```
https://github.com/acidanthera/bugtracker/files/3703498/AppleMCEReporterDisabler.kext.zip
https://github.com/AMD-OSX/AMD_Vanilla/blob/opencore/Extra/AppleMCEReporterDisabler.kext.zip
```

### 出现Still Waiting for Root Device

先按照普通情况处理，若无效则可能需要添加XLNCUSBFix.kext，以修复AMD FX系统的USB控制器。需要10.13及更高版本，下载链接如下。

```
https://cdn.discordapp.com/attachments/566705665616117760/566728101292408877/XLNCUSBFix.kext.zip
```

若无效，则尝试AMD StopSign-fixv5。

## 系统启动

### 在Data&Privacy页重启

进入单用户模式并输入以下命令。以上命令将跳过设置屏幕，并新建一个用户名为Temp User，密码为password的账户。

```
/sbin/fsck -fy
/sbin/mount -uw /
touch /var/db/.AppleSetupDone

launchctl load /System/Library/LaunchDaemons/com.apple.opendirectoryd.plist

dscl . -create /Users/temp
dscl . -create /Users/temp UserShell /bin/bash
dscl . -create /Users/temp RealName "Temp User" 
dscl . -create /Users/temp UniqueID "510"
dscl . -create /Users/temp PrimaryGroupID 20
dscl . -create /Users/temp NFSHomeDirectory /Users/temp
dscl . -passwd /Users/temp password
dscl . -append /Groups/admin GroupMembership temp
dseditgroup -o edit -a temp -t user admin

shutdown -r +0
```

### Safari一直重新加载YouTube等网站

打开系统偏好设置-通用，将高亮显示颜色设置为最后一个颜色。

# 存档

## AMD内核补丁

### 15h/16h

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Kernel</key>
    <dict>
        <key>Patch</key>
        <array>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - commpage_populate -remove rdmsr</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uaABAAAPMg==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                Dx+AAAAAAA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string>_cpu_topology_sort</string>
                <key>Comment</key>
                <string>algrey - cpu_topology_sort -disable _x86_validate_topology</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                6AAA//8=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                /wAA//8=
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                Dx9EAAA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_cache_info - cpuid 0x8000001D instead 0 - 10.15/10.16</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                McAx2zHJMdIPokGJxgAAAAAAAAB0
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                /////////////////wAAAAAA////
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>19.0.0</string>
                <key>Replace</key>
                <data>
                uB0AAIAx2zHJMdIPokGJxg8fQADr
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_cache_info - cpuid 0x8000001D instead 0</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                McAx2zHJMdIPokGJxkGJ0QAAAAAAAAA=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                /////////////////////wAAAAAA//8=
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                uB0AAIAx2zHJMdIPokGJxkGJ0escZpA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>Shaneee - cpuid_set_cache_info - cpuid 0x8000001D instead 0 10.16</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                McAx2zHJMdIPokGJxoM9RD2oAAB0G0E=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>20.0.0</string>
                <key>Replace</key>
                <data>
                uB0AAIAx2zHJMdIPokGJxkGJ0escZpA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_cache_info - cpuid 0x8000001D instead 4</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uAQAAABEifFEiQ==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                uB0AAIBEifFEiQ==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_cache_info - don't set cpuid_cores_per_package</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                weAa/8A=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                //D///A=
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                Dx8A6wY=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>NoOne - skip cpuid_cores_per_package test - 10.15/10.16</string>
                <key>Count</key>
                <integer>0</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                gz0AAAAAAA8AAAAAAIsAvA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                //8AAAD///8AAAAA//8A/w==
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>19.0.0</string>
                <key>Replace</key>
                <data>
                AAAAAAAAAQAAAAAAAAAAAA==
                </data>
                <key>ReplaceMask</key>
                <data>
                AAAAAAAADwAAAAAAAAAAAA==
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - - skip cpuid_cores_per_package test</string>
                <key>Count</key>
                <integer>0</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                gz0AAAAAAHQAi128
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                //8AAAD///8A////
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                AAAAAAAAAQAAAAAA
                </data>
                <key>ReplaceMask</key>
                <data>
                AAAAAAAADwAAAAAA
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_generic_info - remove wrmsr</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uYsAAAAxwDHSDzA=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                Zg8fhAAAAAAAZpA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_generic_info - set microcode=186</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uYsAAAAPMg==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                uroAAABmkA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_generic_info - set flag=1</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uRcAAAAPMsHqEoDiBw==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                sgFmDx+EAAAAAABmkA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_generic_info - disable check to allow leaf7</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                ADoPgg==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                AAAPgg==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - GenuineIntel to AuthenticAMD </string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                R2VudWluZUludGVsAA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                QXV0aGVudGljQU1EAA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_cpufamily - force CPUFAMILY_INTEL_PENRYN</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                MduAPQAAAAAGdQA=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                /////wAAAP///wA=
                </data>
                <key>MaxKernel</key>
                <string>20.3.0</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                u7xP6njpXQAAAJA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>DhinakG - cpuid_set_cpufamily - force CPUFAMILY_INTEL_PENRYN - 11.3b1</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                MdIAAIA9AAAAAAZ1AA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                //8AAP//AAAA////AA==
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>20.4.0</string>
                <key>Replace</key>
                <data>
                swG6vE/qeOldAAAAkA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string>_cpuid_set_info</string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - jmp to calculations and set cpuid_cores_per_package - 10.15/10.16</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                dXHoAAAAAEiLBQAAAABIiQUAAAAA
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                ////AAAAAP///wAAAAD///8AAAAA
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>19.0.0</string>
                <key>Replace</key>
                <data>
                dE4AAAAAAJCJDQAAAADpfgAAAGaQ
                </data>
                <key>ReplaceMask</key>
                <data>
                //8AAAAAAP///wAAAAD/////////
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string>_cpuid_set_info</string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - cores and threads calculations - 10.15/10.16</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uTUAAAAPMkjB4iCJwUgJ0bkBAAEAD0XID7fBwekQg+EP
                6x65NQAAAA8ySMHiIInBSAnRuQEAAQAPRcgPt8HB6RA=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>19.0.0</string>
                <key>Replace</key>
                <data>
                uAgAAIAx2zHJMdIPokGJzkUPtvZB/8ZEifFEifBmDx+E
                AAAAAABmDx+EAAAAAAAPH4QAAAAAAOl8////Dx9EAAA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - cores and logicals count - part 1 - 10.13</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                6xa5NQAAAA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>17.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                6yK5NQAAAA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - cores and logicals count - part 1 - 10.14</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                6zi5NQAAAA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>18.0.0</string>
                <key>Replace</key>
                <data>
                6xK5NQAAAA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string>_cpuid_set_info</string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - cores and logicals count - part 2</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uTUAAAAPMkjB4iAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                //////////////8AAAAAAAAAAAAAAAAAAAAAAAAAAAA=
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                ichmDx+EAAAAAABmDx+EAAAAAABmDx+EAAAAAAAPHwA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string>_cpuid_set_info</string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - cores and logicals count - part 3 - 10.13</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                iQUAAAAAiRUAAAAAhcB1GA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                //8AAAD///8AAAD//////w==
                </data>
                <key>MaxKernel</key>
                <string>17.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                Dx+EAAAAAAAPH4QAAAAAAA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string>_cpuid_set_info</string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - cores and logicals count - part 3 - 10.14</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                hcB0
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>18.0.0</string>
                <key>Replace</key>
                <data>
                ZpDr
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>1</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string>_cpuid_set_info</string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - cores and logicals count - part 4 - 10.13</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                iwUAAAAAiQAAAAAAiwU=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                //8AAAD//wAAAAD///8=
                </data>
                <key>MaxKernel</key>
                <string>17.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                iQAAAAAAAAAAAAAAAAA=
                </data>
                <key>ReplaceMask</key>
                <data>
                /wAAAAAAAAAAAAAAAAA=
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string>_cpuid_set_info</string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - cores and logicals count - part 4 - 10.14</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                SIsFAAAAAEiJBQAAAAA=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                ////AAAA/////wAAAP8=
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>18.0.0</string>
                <key>Replace</key>
                <data>
                kIkAAAAAAJAAAAAAAAA=
                </data>
                <key>ReplaceMask</key>
                <data>
                //8AAAAAAP8AAAAAAAA=
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - i386_init - remove rdmsr (x3)</string>
                <key>Count</key>
                <integer>0</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uZkBAAAPMkjB4iCJxkgJ1rmYAQAADzJIweIgicBICcK/
                WAIxBTHJRTHA
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                Zg8fhAAAAAAAZg8fhAAAAAAAZg8fhAAAAAAAZg8fhAAA
                AAAAZg8fRAAA
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - tsc_init - remove Penryn check to execute default case 10.14.1-.3</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                gfm8T+p4D4TFAQAA
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>18.0.0</string>
                <key>Replace</key>
                <data>
                ZmZmDx+EAAAAAACQ
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - tsc_init - remove Penryn check to execute default case</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                PbxP6ngPhAABAAA=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                /////////wD///8=
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                ZmZmDx+EAAAAAAA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - tsc_init - grab DID and FID from MSR</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uZQBAAAPMonDuc4AAAAPMg+2zokNAAAAAA+2xIkFAAAA
                AA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                ////////////////////////////AAAA////////AAAA
                /w==
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                uXEAAcAPMonASInBSMHpBoPgP0iDwBCA4QdI0+gPH0QA
                AA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string>_tsc_init</string>
                <key>Comment</key>
                <string>algrey - tsc_init - skip msr_flex_ratio test and go grab FSBFrequency from EFI</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                98MAAAEAdA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                Zg8fRAAA6w==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - lapic_init - remove version check and panic - 10.15</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                g/gTdl4=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>19.99.99</string>
                <key>MinKernel</key>
                <string>19.0.0</string>
                <key>Replace</key>
                <data>
                Dx9EAAA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>NoOne - lapic_init - remove version check and panic - 10.16</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                g/gTD4aBAAAA
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>20.0.0</string>
                <key>Replace</key>
                <data>
                kJCQkJCQkJCQ
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - lapic_init - remove version check and panic - 10.13/10.14</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                JfwAAACD+BM=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                JfAAAADrI5A=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - lapic_interrupt - skip checks and prevent panic - 10.15/10.16</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                gz0AAAAAAHQO
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                //8AAAD/////
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>19.0.0</string>
                <key>Replace</key>
                <data>
                6zkPH4AAAAAA
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string>_lapic_interrupt</string>
                <key>Comment</key>
                <string>algrey - lapic_interrupt - skip checks and prevent panic - 10.13/10.14</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                gz0AAAAAAHQK
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>250</integer>
                <key>Mask</key>
                <data>
                //8AAAD/////
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                60gPH4AAAAAA
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - mtrr_update_action - fix PAT</string>
                <key>Count</key>
                <integer>0</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                icCB4v//AP+BygAAAQC5dwIAAA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                ////////D////////////////w==
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                uXcCAAC4BgEHALoGAQcADx9AAA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>Any</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>Shaneee - mtrr_update_action - fix PAT</string>
                <key>Count</key>
                <integer>0</integer>
                <key>Enabled</key>
                <false/>
                <key>Find</key>
                <data>
                icCB4v//AP+BygAAAQC5dwIAAA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                ////////D////////////////w==
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                uXcCAAC4BgYGBroGBgYGDzAPCQ==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
        </array>
    </dict>
</dict>
</plist>
```

### 17h/19h

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Kernel</key>
    <dict>
        <key>Patch</key>
        <array>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string>_i386_switch_lbrs</string>
                <key>Comment</key>
                <string>algrey - Disable _i386_switch_lbrs</string>
                <key>Count</key>
                <integer>0</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>20.1.0</string>
                <key>Replace</key>
                <data>
                ww==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string>_i386_lbr_init</string>
                <key>Comment</key>
                <string>algrey - Disable _i386_lbr_init</string>
                <key>Count</key>
                <integer>0</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>20.1.0</string>
                <key>Replace</key>
                <data>
                ww==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - _i386_init_slave - Remove wrmsr 0x1c8</string>
                <key>Count</key>
                <integer>0</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uAEAAAC5yAEAADHSDzA=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>20.1.0</string>
                <key>Replace</key>
                <data>
                Zg8fhAAAAAAADx9EAAA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string>_i386_lbr_native_state_to_mach_thread_state</string>
                <key>Comment</key>
                <string>algrey - Disable _i386_lbr_native_state_to_mach_thread_state</string>
                <key>Count</key>
                <integer>0</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>20.1.0</string>
                <key>Replace</key>
                <data>
                ww==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - commpage_populate -remove rdmsr</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uaABAAAPMg==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                Dx+AAAAAAA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string>_cpu_topology_sort</string>
                <key>Comment</key>
                <string>algrey - cpu_topology_sort -disable _x86_validate_topology</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                6AAA//8=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                /wAA//8=
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                Dx9EAAA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_cache_info - cpuid 0x8000001D instead 0 - 10.15/10.16</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                McAx2zHJMdIPokGJxgAAAAAAAAB0
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                /////////////////wAAAAAA////
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>19.0.0</string>
                <key>Replace</key>
                <data>
                uB0AAIAx2zHJMdIPokGJxg8fQADr
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_cache_info - cpuid 0x8000001D instead 0 - 10.13/10.14</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                McAx2zHJMdIPokGJxkGJ0QAAAAAAAAA=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                /////////////////////wAAAAAA//8=
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                uB0AAIAx2zHJMdIPokGJxkGJ0escZpA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_cache_info - cpuid 0x8000001D instead 4</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uAQAAABEifFEiQ==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                uB0AAIBEifFEiQ==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_cache_info - don't set cpuid_cores_per_package</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                weAa/8A=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                //D///A=
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                Dx8A6wY=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>NoOne - skip cpuid_cores_per_package test - 10.15/10.16</string>
                <key>Count</key>
                <integer>0</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                gz0AAAAAAA8AAAAAAIsAvA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                //8AAAD///8AAAAA//8A/w==
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>19.0.0</string>
                <key>Replace</key>
                <data>
                AAAAAAAAAQAAAAAAAAAAAA==
                </data>
                <key>ReplaceMask</key>
                <data>
                AAAAAAAADwAAAAAAAAAAAA==
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - skip cpuid_cores_per_package test - 10.13/10.14</string>
                <key>Count</key>
                <integer>0</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                gz0AAAAAAHQAi128
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                //8AAAD///8A////
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                AAAAAAAAAQAAAAAA
                </data>
                <key>ReplaceMask</key>
                <data>
                AAAAAAAADwAAAAAA
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_generic_info - remove wrmsr</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uYsAAAAxwDHSDzA=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                Zg8fhAAAAAAAZpA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_generic_info - set microcode=186</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uYsAAAAPMg==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                uroAAABmkA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_generic_info - set flag=1</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uRcAAAAPMsHqEoDiBw==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                sgFmDx+EAAAAAABmkA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_generic_info - disable check to allow leaf7</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                ADoPgg==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                AAAPgg==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - GenuineIntel to AuthenticAMD</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                R2VudWluZUludGVsAA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                QXV0aGVudGljQU1EAA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_cpufamily - force CPUFAMILY_INTEL_PENRYN</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                MduAPQAAAAAGdQA=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                /////wAAAP///wA=
                </data>
                <key>MaxKernel</key>
                <string>20.3.0</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                u7xP6njpXQAAAJA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>DhinakG - cpuid_set_cpufamily - force CPUFAMILY_INTEL_PENRYN - 11.3b1</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                MdIAAIA9AAAAAAZ1AA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                //8AAP//AAAA////AA==
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>20.4.0</string>
                <key>Replace</key>
                <data>
                swG6vE/qeOldAAAAkA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string>_cpuid_set_info</string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - jmp to calculations and set cpuid_cores_per_package - 10.15/10.16</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                dXHoAAAAAEiLBQAAAABIiQUAAAAA
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                ////AAAAAP///wAAAAD///8AAAAA
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>19.0.0</string>
                <key>Replace</key>
                <data>
                dE4AAAAAAJCJDQAAAADpfgAAAGaQ
                </data>
                <key>ReplaceMask</key>
                <data>
                //8AAAAAAP///wAAAAD/////////
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string>_cpuid_set_info</string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - cores and threads calculations - 10.15/10.16</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uTUAAAAPMkjB4iCJwUgJ0bkBAAEAD0XID7fBwekQg+EP
                6x65NQAAAA8ySMHiIInBSAnRuQEAAQAPRcgPt8HB6RA=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>19.0.0</string>
                <key>Replace</key>
                <data>
                uAgAAIAx2zHJMdIPokGJzkUPtvZB/8a4HgAAgDHbMckx
                0g+iD7b3/8ZEifEx0onI9/aJwUSJ8Ol8////Dx9EAAA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - ryzen cores and logicals count - part 1 - 10.13</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                6xa5NQAAAA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>17.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                6yK5NQAAAA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - ryzen cores and logicals count - part 1 - 10.14</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                6zi5NQAAAA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>18.0.0</string>
                <key>Replace</key>
                <data>
                6xK5NQAAAA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string>_cpuid_set_info</string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - ryzen cores and logicals count - part 2 - 10.13/10.14</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uTUAAAAPMkjB4iAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                //////////////8AAAAAAAAAAAAAAAAAAAAAAAAAAAA=
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                QYnOuB4AAIAx2zHJMdIPog+29//GRInxMdKJyPf2ZpA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string>_cpuid_set_info</string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - ryzen cores and logicals count - part 3 - 10.13</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                iQUAAAAAiRUAAAAAhcB1GA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                //8AAAD///8AAAD//////w==
                </data>
                <key>MaxKernel</key>
                <string>17.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                Dx+EAAAAAAAPH4QAAAAAAA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string>_cpuid_set_info</string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - ryzen cores and logicals count - part 3 - 10.14</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                hcB0
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>18.0.0</string>
                <key>Replace</key>
                <data>
                ZpDr
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>1</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string>_cpuid_set_info</string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - ryzen cores and logicals count - part 4 - 10.13</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                iwUAAAAAiQAAAAAAiwU=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                //8AAAD//wAAAAD///8=
                </data>
                <key>MaxKernel</key>
                <string>17.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                iQAAAAAAAAAAAAAAAAA=
                </data>
                <key>ReplaceMask</key>
                <data>
                /wAAAAAAAAAAAAAAAAA=
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string>_cpuid_set_info</string>
                <key>Comment</key>
                <string>algrey - cpuid_set_info - ryzen cores and logicals count - part 4 - 10.14</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                SIsFAAAAAEiJBQAAAAA=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                ////AAAA/////wAAAP8=
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>18.0.0</string>
                <key>Replace</key>
                <data>
                kIkAAAAAAJAAAAAAAAA=
                </data>
                <key>ReplaceMask</key>
                <data>
                //8AAAAAAP8AAAAAAAA=
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - i386_init - remove rdmsr (x3)</string>
                <key>Count</key>
                <integer>0</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uZkBAAAPMkjB4iCJxkgJ1rmYAQAADzJIweIgicBICcK/
                WAIxBTHJRTHA
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                Zg8fhAAAAAAAZg8fhAAAAAAAZg8fhAAAAAAAZg8fhAAA
                AAAAZg8fRAAA
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - tsc_init - remove Penryn check to execute default case 10.14</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                gfm8T+p4D4TFAQAA
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>18.0.0</string>
                <key>Replace</key>
                <data>
                ZmZmDx+EAAAAAACQ
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - tsc_init - remove Penryn check to execute default case</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                PbxP6ngPhAABAAA=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                /////////wD///8=
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                ZmZmDx+EAAAAAAA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - tsc_init - grab DID and VID from MSR</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                uZQBAAAPMonDuc4AAAAPMg+2zokNAAAAAA+2xIkFAAAA
                AA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                ////////////////////////////AAAA////////AAAA
                /w==
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                uWQAAcAPMg+2yInGwe4Ig+Y/RTH/MdJIichI9/ZIAcBm
                kA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string>_tsc_init</string>
                <key>Comment</key>
                <string>algrey - tsc_init - skip msr_flex_ratio test and go grab FSBFrequency from EFI</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                98MAAAEAdA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                Zg8fRAAA6w==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - lapic_init - remove version check and panic - 10.15</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                g/gTdl4=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>19.99.99</string>
                <key>MinKernel</key>
                <string>19.0.0</string>
                <key>Replace</key>
                <data>
                Dx9EAAA=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>NoOne - lapic_init - remove version check and panic - 10.16</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                g/gTD4aBAAAA
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>20.0.0</string>
                <key>Replace</key>
                <data>
                kJCQkJCQkJCQ
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - lapic_init - remove version check and panic - 10.13/10.14</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                JfwAAACD+BM=
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                JfAAAADrI5A=
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - lapic_interrupt - skip checks and prevent panic - 10.15/10.16</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                gz0AAAAAAHQO
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                //8AAAD/////
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>19.0.0</string>
                <key>Replace</key>
                <data>
                6zkPH4AAAAAA
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string>_lapic_interrupt</string>
                <key>Comment</key>
                <string>algrey - lapic_interrupt - skip checks and prevent panic - 10.13/10.14</string>
                <key>Count</key>
                <integer>1</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                gz0AAAAAAHQK
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>250</integer>
                <key>Mask</key>
                <data>
                //8AAAD/////
                </data>
                <key>MaxKernel</key>
                <string>18.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                60gPH4AAAAAA
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>algrey - mtrr_update_action - fix PAT</string>
                <key>Count</key>
                <integer>0</integer>
                <key>Enabled</key>
                <true/>
                <key>Find</key>
                <data>
                icCB4v//AP+BygAAAQC5dwIAAA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                ////////D////////////////w==
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                uXcCAAC4BgEHALoGAQcADx9AAA==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
            <dict>
                <key>Arch</key>
                <string>x86_64</string>
                <key>Base</key>
                <string></string>
                <key>Comment</key>
                <string>Shaneee - mtrr_update_action - fix PAT</string>
                <key>Count</key>
                <integer>0</integer>
                <key>Enabled</key>
                <false/>
                <key>Find</key>
                <data>
                icCB4v//AP+BygAAAQC5dwIAAA==
                </data>
                <key>Identifier</key>
                <string>kernel</string>
                <key>Limit</key>
                <integer>0</integer>
                <key>Mask</key>
                <data>
                ////////D////////////////w==
                </data>
                <key>MaxKernel</key>
                <string>20.99.99</string>
                <key>MinKernel</key>
                <string>17.0.0</string>
                <key>Replace</key>
                <data>
                uXcCAAC4BgYGBroGBgYGDzAPCQ==
                </data>
                <key>ReplaceMask</key>
                <data>
                </data>
                <key>Skip</key>
                <integer>0</integer>
            </dict>
        </array>
    </dict>
</dict>
</plist>
```

# 参考教程

## naveenkrdy/AdobeAMDFix.md

```
https://gist.github.com/naveenkrdy/26760ac5135deed6d0bb8902f6ceb6bd
```

## AMD Mojave Kernel Development and Testing

```
https://www.insanelymac.com/forum/topic/335877-amd-mojave-kernel-development-and-testing/page/7/?tab=comments#comment-2658085
https://www.insanelymac.com/forum/topic/335877-amd-mojave-kernel-development-and-testing/page/9/?tab=comments#comment-2661857
```

## Vanilla AMD Hackintosh

```
https://kb.amd-osx.com/guides/HS/
https://kb.amd-osx.com/guides/MJ/
```

## SNOWLEOPARDAMD INSTALL AND POSTINSTALL

```
https://web.archive.org/web/20201129192905/https://amd-osx.com/forum/viewtopic.php?t=4482#p39746
```

## Installation Guides/Kalway AMD 10 5 2

```
https://wiki.osx86project.org/wiki/index.php/Installation_Guides/Kalway_AMD_10_5_2
```

## MP3,1 (& others?) SSE 4.2 emulation (to enable AMD Metal driver)

```
https://forums.macrumors.com/threads/mp3-1-others-sse-4-2-emulation-to-enable-amd-metal-driver.2206682/
```
## Install Snow Leopard on AMD PC, Laptop

```
https://geeknizer.com/install-snow-leopard-on-amd/
```