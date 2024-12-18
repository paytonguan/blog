---
title: CPU调整指南
categories: Computer
abbrlink: Instructions-Of-CPU-Adjust
date: 2024-12-18 11:54:29
tags:
---

![](topic.jpg)

CPU调整指南。

<!-- more -->

# 查看被限制性能的处理器

在Windows下打开事件查看器，选择Windows日志-系统，然后点击右侧栏的`筛选当前日志`，类型选择Kernel-Processor-Power即可。

# 超频

对于CPU，BCLK乘以倍增系数即为内核频率。BCLK即基本时钟速度，一般为100MHz。倍增系数即倍频，随CPU变化。

如果CPU未锁定倍频，可直接超频。若CPU锁定了倍频，可通过提高主板FSB速度，或屏蔽CPU部分针脚实现超频。

对于Intel，名称末尾带K或X的CPU表明该CPU未锁频。超频前建议将CPU各核设为相等的频率。

## 通过BIOS

一般提高CPU Core Ratio即可。

## 通过软件

### XTU

下载链接如下。

```
https://downloadcenter.intel.com/zh-cn/download/24075/Intel-Extreme-Tuning-Utility-Intel-XTU-
```

安装完成后打开，点击Basic Tuning，向右调整两个滑块即可，注意缓存倍频如果低于内核倍频，CPU 性能会降低。建议每次上调一倍，以防止电脑死机。完成设定后用Stress Test进行压力测试，查看CPU能否稳定运行。

若需要更精细的调整，可点击左边Advanced Tuning，会出现提示警告。点击`I agree,don't show again`即可打开高级选项。

一般调节Turbo Boost Short Power Max、Turbo Boost Power Max、Turbo Boost Power Time Window，控制温度使用Core Voltage Offset。

|                   项目                  |        中文名        |                                                                        注释                                                                        |                                                                                                                                                                                    调整方法                                                                                                                                                                                    |
|-----------------------------------------|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Core**                                |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Refrence Clock                          | 外频                 |                                                                                                                                                    | 保持Default                                                                                                                                                                                                                                                                                                                                                                    |
| Max Non Turbo Boost Ratio               | 默频                 | 最大非睿频下倍频                                                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                |
| Core Voltage Mode                       | CPU电压模式          | 分为 Adaptive（自适应）和 Static（静态）                                                                                                           | 保持默认的Adaptive                                                                                                                                                                                                                                                                                                                                                             |
| Core Voltage                            | CPU电压              | 设置CPU的绝对电压                                                                                                                                  | 保持Default                                                                                                                                                                                                                                                                                                                                                                    |
| Core Voltage Offset                     | CPU偏移电压          | 相对于绝对电压偏移（增大或减小）的电压                                                                                                             | 降压的Offset是负值。建议以0.01V（10mV）为单位降压，先调整Offset至-0.01V，然后转至Stress Test，勾选CPU Stress Test并测试10分钟，若无异常继续降0.01V。在降压0.05V之后以0.005V（5mV）为单位进行降压，并且每次降压之后应该至少有20分钟的P95或者FPU的测试并且保证显卡有负载（比如GPUZ的Render Test或者甜甜圈）。若意外关机，则把最后一次降压幅度减小0.01V，并测试两小时，无异常即可 |
| Enhanced Intel SpeedStep Technology     | 电源管理相关         | 控制CPU电压和频率关系，使得不同场景下CPU可以在高性能和电池优化模式中来回切换                                                                       |                                                                                                                                                                                                                                                                                                                                                                                |
| Intel Turbo Boost Technology            | 睿频加速技术         |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Processor Core IccMax                   | 核心最大电流         | 调节核心电流                                                                                                                                       | 保持Default                                                                                                                                                                                                                                                                                                                                                                    |
| AVX Ratio Offset                        | AVX倍频补偿          | 控制CPU跑AVX指令集（比如浮点运算）的最大频率。比如设置x3的AVX频率补偿，倍频45x外频100MHz，那么在浮点运算的时候，CPU的主频只会到（45-3）*100=4.2GHz | 保持Default                                                                                                                                                                                                                                                                                                                                                                    |
| Turbo Boost Short Power Max Enable      | 短时间超功耗睿频模式 | 可以让CPU短时间睿频到一个超过标定频率很多的一个频率，但代价是温度高，而且CPU可能承受不住太长时间                                                   |                                                                                                                                                                                                                                                                                                                                                                                |
| Turbo Boost Short Power Max             | 短时间最大睿频功耗   | PL2，决定瞬间性能（在瞬间负载下这个数值会限制CPU的功耗）                                                                                           | 拉至最大                                                                                                                                                                                                                                                                                                                                                                       |
| Turbo Boost Power Max                   | 长时间睿频最大功耗   | PL1，决定稳定性能（PL1数值上肯定不会超过PL2）                                                                                                      | 拉至最大                                                                                                                                                                                                                                                                                                                                                                       |
| Turbo Boost Power Time Window           | 睿频加速时间         | PL2时间                                                                                                                                            | 拉至最大                                                                                                                                                                                                                                                                                                                                                                       |
| Turbo Boost Short Power Time Windows    | 睿频最大功耗持续时间 | PL1时间（在这个此时间内，CPU功耗可以突破PL1但不高于PL2）                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                |
| Multipliers                             | 多核倍频             | 超频专用                                                                                                                                           | 拉至最大                                                                                                                                                                                                                                                                                                                                                                       |
| **Cache**                               |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Cache Voltage Mode                      | 缓存电压模式         |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Cache Voltage                           | 缓存绝对电压         |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Cache Voltage Offset                    | 缓存偏移电压         |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Cacha IccMac                            |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| **Graphics**                            |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Precessor Graphics Ratio Limit          |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Processor Graphics Valtage Mode         |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Processor Graphics Valtage              |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Processor Gtaphics Valtage Offset       |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Processor Graphics IccMax               |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Processor Graphics Media Voltage        |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Processor Graphics Media Voltage Offset |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| Processor Graphics Unslice Iccmax       |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| **Other**                               |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |
| System Agent IccMax                     |                      |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                |

XTU可用命令行进行控制。打开记事本，复制以下代码并保存，后缀名改为ps1，即可通过PowerShell运行。各种参数ID可以打开C:\Program Files (x86)\Intel\Intel(R) Extreme Tuning Utility\Client\XtuCLI.exe查看。

```
$status = Get-Service -Name "XTU3SERVICE" | Select-Object "status" | Format-Wide
if ($status -ne "Running") {start-service -name "XTU3SERVICE"}
& 'C:\Program Files (x86)\Intel\Intel(R) Extreme Tuning Utility\Client\XTUCli.exe' -t -id 48 -v 45.00 # TDP
& 'C:\Program Files (x86)\Intel\Intel(R) Extreme Tuning Utility\Client\XTUCli.exe' -t -id 47 -v 60.00 # 短时间功耗墙
& 'C:\Program Files (x86)\Intel\Intel(R) Extreme Tuning Utility\Client\XTUCli.exe' -t -id 66 -v 28.00 # 睿频时间
& 'C:\Program Files (x86)\Intel\Intel(R) Extreme Tuning Utility\Client\XTUCli.exe' -t -id 34 -v -125 # 所降电压
sleep 4
stop-process -id $PID -force
```

### Intel Performance Maximizer

官方自动超频软件，适用于i5、i7、i9、X系列的部分处理器。下载后运行即可。

```
https://downloadcenter.intel.com/zh-cn/product/188727/Intel-Performance-Maximizer
```

### Throttlestop

下载链接如下。XP下可运行的版本为8.40。

```
# 最新版本
https://www.techpowerup.com/download/techpowerup-throttlestop/

# 8.40版本
http://xiazai.zol.com.cn/detail/44/439686.shtml
```

打开软件后勾选Set Multiplier和Speed Shift EPP，取消勾选BD PROCHOT，将Set Multiplier后面的数字拉到最大。点击Limit Reasons可查看功耗被限制的原因。

点击BCLK以设置PL1/PL2，可先尝试200。点击FIVR可设置电压，首先选择CPU Core/Cache，然后勾选Adjustable Voltage，再将下方的Offset Voltage拉到最大。

其余选项解释如下。

|           项目           |    中文名     |                                                                          注释                                                                          | 调整方法 |
|--------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| Clock/Chipset Modulation | 时钟/芯片调整 | 大多数新芯片不开启，开启后ThrottleStop中的功能有的不起作用                                                                                             | 关闭     |
| Set Multipliet           | 乘数调整      |                                                                                                                                                        | 拉到最大 |
| Speed Shift-EPP          | 转换速度      | 如果勾选需要到TPL选项里面开启，开启了后根据CPU负载率来快速的降低和提升频率，数字可填，0是保持较大的性能上下浮动，可以向下浮动很低的频率，一般用于老CPU |          |
| Power Saver              | 节电调整      | 让机器用总线速率的一半运行，使相对老的CPU实现降低频率来降低功耗                                                                                        |          |
| Disable Turbo            | 关闭睿频      |                                                                                                                                                        |          |
| BD PROCHOT               | 温度墙        |                                                                                                                                                        |          |
| C1E                      |               | 空闲时随机上下数值频率                                                                                                                                 |          |

### SetFSB

需要使用2.1.91.1版本，否则将提示无法使用。

使用前需知道CPU所用的时钟发生器的型号，可尝试搜索得到。若仍未知，则首先打开CPU-Z，记录总线速度和额定FSB，然后打开SetFSB，逐个时钟发生器进行尝试，点击Get FSB，查看下面的FSB值是否与CPU-Z中得到的一致，若一致则有可能为该时钟发生器。

确定时钟发生器的型号后，拖动滑块即可进行调整。本机可能的时钟发生器为ICS9LPR501HGLF和ICSLP505-2HGLF。

### ClockGen

以管理员身份运行ClockGen，点击Options，确保已勾选`Apply current settings at startup`，保存。点击PLL Setup，勾选Ignore GSB/PCI，选择正确的PLL，可通过搜索或逐个尝试的方式，点击Read Clocks，然后点击PLL Control并查看数据是否正确。若正确，稍微向右拖动Selection的滑块并Apply即可。

### AMD Ryzen Master

AMD锐龙处理器超频工具，下载链接如下。打开后调整速度和电压即可。

```
https://www.amd.com/zh-hans/technologies/ryzen-master
```

### CPU-Tweaker

支持AMD Phenom DDR2&DDR3、INTEL Core i7 DDR3，不支持intel四核处理器。下载链接如下。

```
https://www.majorgeeks.com/files/details/cpu_tweaker.html
```

# CPU State

P State是实际的速度步长，定义了各种频率/电压组合。当CPU不在重负载下时，它可以降低使用的功率。可以通过降压进一步控制功率。

C State是CPU深度睡眠状态，一般C1，C2，C3或C1，C2，C4三个C状态可以同时工作。在DSDT中，Name (C1M4, Package (0x04)定义以上三个状态。

可在IORegistryExplorer中选择CPU0@0->AppleACPICPI->ACPI_SMC_PlatformPlugin，查看右侧参数。若CPUPlimit为0x0，PerformanceStateArray包含五个状态，说明已经开启P State。若有CSTInfo，说明已经开启C State。

对于Mac，开启P State需要修改SLE下的IOPlatformPluginFamily.kext/Contents/PlugIns/ACPI_SMC_PlatformPlugin.kext/Contents/Resources/Info.plist，该文件规定了不同Mac型号可使用的SpeedStep和C State。示例如下。由于Mac一般从SSDT而非DSDT读取P State，因此除非需要降压，否则不需要修改DSDT。

```
<key>ACPI_SMC_PlatformPlugin</key>
<dict>
<key>CFBundleIdentifier</key>
<string>com.apple.driver.ACPI_SMC_PlatformPlugin</string>
<key>IOClass</key>
<string>ACPI_SMC_PlatformPlugin</string>
<key>IOPlatformThermalProfile</key>
<dict>
// 以下为特定型号的配置内容，不同型号内容不同
    <dict>
        <key>model</key>
        <string>MacBookPro2,3</string>
        <key>restart-actions</key>
        <dict>
            // 开启P State
            <key>cpu-p-state</key>
            <integer>0</integer>
        </dict>
    </dict>
    </array>
    <key>ControlArray</key>
    ...
    <key>PLimitDict</key>
    <dict>
    <key>MacBookPro2,3</key>
    <integer>0</integer>
    </dict>
    <key>StepDataDict</key>
    ....
</dict>
// 结束
<key>IOProbeScore</key>
<integer>1200</integer>
<key>IOPropertyMatch</key>
<dict>
    <key>IOCPUNumber</key>
    <integer>0</integer>
</dict>
<key>IOProviderClass</key>
<string>AppleACPICPU</string>
<key>IOResourceMatch</key>
<string>ACPI</string>
</dict>
```

开启C State则需要修改DSDT。在系统SSDT或DSDT中搜索Name (C1M4, Package (0x04)，找到定义C1，C2，C4的块，然后放置到DSDT中，示例如下。此处重写了`_CST`方法，使其能返回正确的C State。若C State定义与本机不符，则Mac将不会执行该部分代码，不会对硬件造成影响。

```
Scope (_PR)
{
Processor (CPU0, 0x00, 0x00001010, 0x06) {}
Processor (CPU1, 0x01, 0x00001010, 0x06) {}
}
Scope (\_PR.CPU0)
{
Name (C1M4, Package (0x04)
{
    0x03, 
    Package (0x04)
    {
        ResourceTemplate ()
        {
            Register (FFixedHW, 
                0x01,              // Bit Width
                0x02,              // Bit Offset
                0x0000000000000000, // Address
                0x01,              // Access Size
                )
        }, 
        0x01, 0x01, 0x03E8
    }, 
    Package (0x04)
    {
        ResourceTemplate ()
        {
            Register (SystemIO, 
                0x08,              // Bit Width
                0x00,              // Bit Offset
                0x0000000000001014, // Address
                ,)
        }, 
        0x02, 0x01, 0x01F4
    }, 
    Package (0x04)
    {
        ResourceTemplate ()
        {
            Register (SystemIO, 
                0x08,              // Bit Width
                0x00,              // Bit Offset
                0x0000000000001016, // Address
                ,)
        }, 
        0x03, 0x39, 0x64
    }
})

Method (_CST, 0, NotSerialized)
{
Return (C1M4)
}
}
Scope (\_PR.CPU1)
{
Method (_CST, 0, NotSerialized)
{
    Return (\_PR.CPU0._CST)
}
}
```

在Mac下可打开DPCI Manager并单击P States，即可查看SpeedStep。

# 修改MSR寄存器

CPU的功率限制被写在MSR寄存器中。通过修改MSR寄存器的值，可以提升CPU的功率。

## 开启CFG

在BIOS中，若CFG Lock处于Enabled状态，将不能对MSR寄存器进行读写。故须先用grub将CFG Lock设为Disabled。

## 查看寄存器地址

### 通过Hackintool

推荐使用该方法。

进入Mac并打开Hackintool，切换到工具选项卡。点击Intel图标，将输出复制到文本文档中，该输出即有CPU的相关信息，包括MSR寄存器的地址。

### 通过AppleIntelInfo.kext

通过以下链接下载压缩包。

```
https://github.com/Piker-Alpha/AppleIntelInfo
```

在Mac下载压缩包并用Xcode编译，编译完成后切换到kext所在目录，输入以下命令以获取CPU信息。

```
sudo chown -R root:wheel AppleIntelInfo.kext
sudo chmod -R 755 AppleIntelInfo.kext
sudo kextload AppleIntelInfo.kext

// 在终端查看
sudo cat /tmp/AppleIntelInfo.dat | more

// 或将包含CPU信息的文件复制到当前目录下
sudo cp /tmp/AppleIntelInfo.dat AppleIntelInfo.txt
sudo kextunload AppleIntelInfo.kext
```

### 通过官方文档

在Linux的终端下输入以下命令以获取CPU信息。

```
cat /proc/cpuinfo | less
```

根据输出判定CPU类型，主要用到的是cpu family和model，将其转为十六进制后，可得到CPUID。CPUID由DisplayFamily_DisplayModel构成，以本机为例，本机的DisplayFamily为06h，DisplayModel为8Eh（均为十六进制），故CPUID为06_8EH。

```
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 142
model name      : Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz
stepping        : 10
......
```

在Intel官方文档下载页面中下载Four-Volume Set of Intel® 64 and IA-32 Architectures Software Developer’s Manuals中的第四章，链接如下。

```
https://software.intel.com/en-us/download/intel-64-and-ia-32-architectures-software-developers-manual-volume-4-model-specific-registers
```

在文档中搜索CPUID（06_8EH）、CPU架构（Kaby Lake）或CPU代数（8TH GENERATION），查询其MSR地址。如本CPU在2.17一节中。

## 查询寄存器含义

在Intel官方文档下载页面中下载Four-Volume Set of Intel® 64 and IA-32 Architectures Software Developer’s Manuals中的第三章，链接如下。

```
https://software.intel.com/en-us/download/intel-64-and-ia-32-architectures-sdm-combined-volumes-3a-3b-3c-and-3d-system-programming-guide
```

打开文档并搜索需要更改的寄存器（如MSR_PKG_POWER_LIMIT），即可查询其对应含义。

## 准备修改环境

进入Linux系统并执行以下命令，以安装msr-tools并加载系统MSR模块。

```
sudo apt-get install msr-tools
sudo modprobe msr
```

然后便可利用msr-tools完成MSR寄存器的读取与修改。主要用到以下两个命令，均为十六进制。 

```
// 读取[A]地址的值
sudo rdmsr [A]

// 将[A]地址的值改为[B]
sudo wrmsr [A] [B]
```

## 修改寄存器内容

以下以修改Package power limits为例进行说明。

首先需要读取MSR_RAPL_POWER_UNIT寄存器的值，此寄存器存储功率/能量/时间单位。在本CPU中，该寄存器的地址为0x606，故输入以下命令。

```
sudo rdmsr 0x606 // 输出A0E03
```

从官方文档（第三章）可得到关于此寄存器的说明，原文如下。

```
MSR_RAPL_POWER_UNIT (Figure 14-35) provides the following information across all RAPL domains: 
Power Units (bits 3:0): Power related information (in Watts) is based on the multiplier, 1/ 2^PU; where PU is an unsigned integer represented by bits 3:0. Default value is 0011b, indicating power unit is in 1/8 Watts increment. 
Energy Status Units (bits 12:8): Energy related information (in Joules) is based on the multiplier, 1/2^ESU; where ESU is an unsigned integer represented by bits 12:8. Default value is 10000b, indicating energy status unit is in 15.3 micro-Joules increment. 
Time Units (bits 19:16): Time related information (in Seconds) is based on the multiplier, 1/ 2^TU; where TU is an unsigned integer represented by bits 19:16. Default value is 1010b, indicating time unit is in 976 micro- seconds increment. 
```

将A0E03转换为二进制为1010 0000 1110 0000 0011。bits A:B表示二进制的第B到A位（从低位开始数，个位是第0位），故按照说明可得下表。

| 原文                              | 位置    | 含义     | 内容（二进制）      | 内容含义                                                       | 说明                                                                        |
| --------------------------------- | ------- | -------  | ------------------- | -------------------------------------------------------------- |                                                                             |
| Power Units                       | 3:0     | 功率单位 | 0011                | 0.125W                                                         | 0011转换为十进制为3，故功率以$(\frac{1}{2})^3$=0.125W为单位                 |
| Energy Status Units               | 12:8    | 能量单位 | 01110               | 0.00006103515625J                                              | 01110转换为十进制为14，故能量以$(\frac{1}{2})^{14}$=0.00006103515625J为单位 |
| Time Units                        | 19:16   | 时间单位 | 1010                | 0.0009765625s                                                  | 01110转换为十进制为10，故时间以$(\frac{1}{2})^{10}$=0.0009765625s为单位     |

Package power limits位于MSR_PKG_POWER_LIMIT寄存器中，本CPU对应的地址为0x610。输入以下指令得到该地址的值，并对应进行修改。

```
sudo rdmsr 0x610 // 输出42816000DD8160（二进制为0000 0000 0100 0010 1000 0001 0110 0000 0000 0000 1101 1101 1000 0001 0110 0000）
```

|              原文              |  位置 | 内容（二进制）  | 更改为（二进制） |                                                         说明                                                         |
|--------------------------------|-------|-----------------|------------------|----------------------------------------------------------------------------------------------------------------------|
| Package Power Limit #1         | 14:0  | 000000101100000 | /                | 原值转换为十进制为352，因功率以0.125W为单位，故为352×0.125=44W                                                       |
| Enable Power Limit #1          | 15    |               1 | 0                | 0为关闭，1为开启                                                                                                     |
| Package Clamping Limitation #1 | 16    |               1 |                  |                                                                                                                      |
| Time Window for Power Limit #1 | 23:17 |         1101110 |                  | 时间限制值= $(2^Y*(1+Z/4)*TimeUnit)$，其中Y是寄存器的第21:17位，Z是寄存器的第23:22位，TimeUnit是前面所定义的时间单位 |
| Package Power Limit #2         | 46:32 | 000000101100000 |                  |                                                                                                                      |
| Enable Power Limit #2          | 47    |               1 | 0                | 0为关闭，1为开启                                                                                                     |
| Package Clamping Limitation #2 | 48    |               0 |                  |                                                                                                                      |
| Time Window for Power Limit #2 | 55:49 |         0100001 |                  | 时间限制值= $(2^Y*(1+Z/4)*TimeUnit)$，其中Y是寄存器的第21:17位，Z是寄存器的第23:22位，TimeUnit是前面所定义的时间单位 |
| Lock                           | 63    |               0 |                  | CFG锁，若为1则无法写入                                                                                               |

将更改后的内容从低位到高位组成一个64位的二进制数并转换为十六进制 ，然后用wrmsr写入即可。写入后用rdmsr读取，检验结果是否正确。

## 可修改的寄存器

### MSR_PKG_POWER_LIMIT

参考值为42819800FC80C8h。

|             原文            |  位置 | 参考值（二进制） | 说明 |
|-----------------------------|-------|------------------|------|
| Pkg power limit             | 14:0  |         11001000 |      |
| Pkg power enabled           | 15:15 |                1 |      |
| Pkg clamping limit          | 16:16 |                0 |      |
| Pkg power limit time window | 23:17 |          1111110 |      |
| Pkg power limit 2           | 46:32 |        110011000 |      |
| Pkg power 2 enabled         | 47:47 |                1 |      |
| Pkg clamping limit 2        | 48:48 |                0 |      |
| Pkg power limit time window | 55:49 |          0100001 |      |
| MSR lock                    | 63:63 |                0 |      |

### MSR_PLATFORM_INFO

|                原文                | 位置 | 参考值（二进制） |                  说明                  |
|------------------------------------|------|------------------|----------------------------------------|
| Maximum non-turbo                  | 15:8 |                  |                                        |
| Programmable ratio limit for turbo | 28   |                  | 为1时可修改MSR_TURBO_RATIO_LIMIT的内容 |
| Programmable TDP limit for turbo   | 29   |                  |                                        |
| Programmable TJ offset             | 30   |                  |                                        |

### MSR_TURBO_RATIO_LIMIT

参考值为28282828h。

|   原文   |  位置 | 参考值（二进制） | 说明 |
|----------|-------|------------------|------|
| Ratio 1C | 7:0   |         00101000 |      |
| Ratio 2C | 15:8  |         00101000 |      |
| Ratio 3C | 23:16 |         00101000 |      |
| Ratio 4C | 31:24 |         00101000 |      |

# DPTF

DPTF是Intel的一项技术，用于温度控制。DPTF会根据温度动态调节睿频性能的软件驱动，在低温下上调TDP限制，高温下下调限制。

## 禁用

若BIOS中有开关，可直接通过BIOS关闭。

若控制面板有DPTF相关软件，直接卸载即可。若无卸载程序，可通过任务管理器/服务找到DPTF并定位，一般在C:\Windows\System32\Intel下，直接删除整个文件夹即可。

从原理上来说，在驱动层面的DPTF会优先掌控系统性能。如果DPTF作为设备出现，则需要禁用该设备。Windows自动更新会把驱动安装回来，因此需要通过策略组的方式禁用设备安装。

按Win+R打开运行命令，输入`gpedit.msc`打开策略组。打开计算机配置-管理模板-系统-设备安装-设备安装限制，在右侧窗口点击`阻止安装与下列任何设备id相匹配的设备`，打开面板，在左边的选择中点击已启用，在`禁止安装与下列任何设备id相匹配的设备`中打开显示按钮，在输入框中输入dptf的硬件ID即可。

# 工具

## Core Temp

查看CPU各核温度。

```
https://www.alcpu.com/CoreTemp/
```

## CPU-Z

查看系统硬件信息。

# 附录

## 本机CPU信息

由Hackintool输出。

```
AppleIntelInfo.kext v3.0 Copyright © 2012-2017 Pike R. Alpha. All rights reserved.

Settings:
------------------------------------------
enableHWP............................... : 1
logMSRs................................. : 1
logIGPU................................. : 1
logIntelRegs............................ : 0
logCStates.............................. : 1
logIPGStyle............................. : 1
InitialTSC.............................. : 0x9fd6f3b73 (2 MHz)
MWAIT C-States.......................... : 286531872

Processor Brandstring................... : Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz

Processor Signature..................... : 0x806EA
------------------------------------------
 - Family............................... : 6
 - Stepping............................. : 10
 - Model................................ : 0x8E (142)

Model Specific Registers (MSRs)
------------------------------------------

MSR_IA32_PLATFORM_ID..............(0x17) : 0x1C000000000000
------------------------------------------
 - Processor Flags...................... : 7

MSR_CORE_THREAD_COUNT.............(0x35) : 0x40008
------------------------------------------
 - Core Count........................... : 4
 - Thread Count......................... : 8

MSR_PLATFORM_INFO.................(0xCE) : 0x4043DF1011400
------------------------------------------
 - Maximum Non-Turbo Ratio.............. : 0x14 (2000 MHz)
 - Ratio Limit for Turbo Mode........... : 1 (programmable)
 - TDP Limit for Turbo Mode............. : 1 (programmable)
 - Low Power Mode Support............... : 1 (LPM supported)
 - Number of ConfigTDP Levels........... : 2 (additional TDP level(s) available)
 - Maximum Efficiency Ratio............. : 4
 - Minimum Operating Ratio.............. : 4

MSR_PMG_CST_CONFIG_CONTROL........(0xE2) : 0x7E000008
------------------------------------------
 - I/O MWAIT Redirection Enable......... : 0 (not enabled)
 - CFG Lock............................. : 0 (MSR not locked)
 - C3 State Auto Demotion............... : 1 (enabled)
 - C1 State Auto Demotion............... : 1 (enabled)
 - C3 State Undemotion.................. : 1 (enabled)
 - C1 State Undemotion.................. : 1 (enabled)
 - Package C-State Auto Demotion........ : 1 (enabled)
 - Package C-State Undemotion........... : 1 (enabled)

MSR_PMG_IO_CAPTURE_BASE...........(0xE4) : 0x51814
------------------------------------------
 - LVL_2 Base Address................... : 0x1814
 - C-state Range........................ : 5 (C-States not included, I/O MWAIT redirection not enabled)

IA32_MPERF........................(0xE7) : 0x1A1BB41C1
IA32_APERF........................(0xE8) : 0x1B512EEF7

MSR_FLEX_RATIO...................(0x194) : 0x11400
------------------------------------------

MSR_IA32_PERF_STATUS.............(0x198) : 0x274B00002400
------------------------------------------
 - Current Performance State Value...... : 0x2400 (3600 MHz)

MSR_IA32_PERF_CONTROL............(0x199) : 0x1100
------------------------------------------
 - Target performance State Value....... : 0x1100 (1700 MHz)
 - Intel Dynamic Acceleration........... : 0 (IDA engaged)

IA32_CLOCK_MODULATION............(0x19A) : 0x0

IA32_THERM_INTERRUPT.............(0x19B) : 0x10
------------------------------------------
 - High-Temperature Interrupt Enable.... : 0 (disabled)
 - Low-Temperature Interrupt Enable..... : 0 (disabled)
 - PROCHOT# Interrupt Enable............ : 0 (disabled)
 - FORCEPR# Interrupt Enable............ : 0 (disabled)
 - Critical Temperature Interrupt Enable : 1 (enabled)
 - Threshold #1 Value................... : 0
 - Threshold #1 Interrupt Enable........ : 0 (disabled)
 - Threshold #2 Value................... : 0
 - Threshold #2 Interrupt Enable........ : 0 (disabled)
 - Power Limit Notification Enable...... : 0 (disabled)

IA32_THERM_STATUS................(0x19C) : 0x882C2800
------------------------------------------
 - Thermal Status....................... : 0
 - Thermal Log.......................... : 0
 - PROCHOT # or FORCEPR# event.......... : 0
 - PROCHOT # or FORCEPR# log............ : 0
 - Critical Temperature Status.......... : 0
 - Critical Temperature log............. : 0
 - Thermal Threshold #1 Status.......... : 0
 - Thermal Threshold #1 log............. : 0
 - Thermal Threshold #2 Status.......... : 0
 - Thermal Threshold #2 log............. : 0
 - Power Limitation Status.............. : 0
 - Power Limitation log................. : 1
 - Current Limit Status................. : 0
 - Current Limit log.................... : 1
 - Cross Domain Limit Status............ : 0
 - Cross Domain Limit log............... : 0
 - Digital Readout...................... : 44
 - Resolution in Degrees Celsius........ : 1
 - Reading Valid........................ : 1 (valid)

MSR_THERM2_CTL...................(0x19D) : 0x0

IA32_MISC_ENABLES................(0x1A0) : 0x850089
------------------------------------------
 - Fast-Strings......................... : 1 (enabled)
 - FOPCODE compatibility mode Enable.... : 0
 - Automatic Thermal Control Circuit.... : 1 (enabled)
 - Split-lock Disable................... : 0
 - Performance Monitoring............... : 1 (available)
 - Bus Lock On Cache Line Splits Disable : 0
 - Hardware prefetch Disable............ : 0
 - Processor Event Based Sampling....... : 0 (PEBS supported)
 - GV1/2 legacy Enable.................. : 0
 - Enhanced Intel SpeedStep Technology.. : 1 (enabled)
 - MONITOR FSM.......................... : 1 (MONITOR/MWAIT supported)
 - Adjacent sector prefetch Disable..... : 0
 - CFG Lock............................. : 0 (MSR not locked)
 - xTPR Message Disable................. : 1 (disabled)

MSR_TEMPERATURE_TARGET...........(0x1A2) : 0xB640000
------------------------------------------
 - Turbo Attenuation Units.............. : 0 
 - Temperature Target................... : 100
 - TCC Activation Offset................ : 11

MSR_MISC_PWR_MGMT................(0x1AA) : 0x401CC1
------------------------------------------
 - EIST Hardware Coordination........... : 1 (hardware coordination disabled)
 - Energy/Performance Bias support...... : 1
 - Energy/Performance Bias.............. : 0 (disabled/MSR not visible to software)
 - Thermal Interrupt Coordination Enable : 1 (thermal interrupt routed to all cores)
 - SpeedShift Technology Enable......... : 1 (enabled)
 - SpeedShift Interrupt Coordination.... : 1 (enabled)
 - SpeedShift Energy Efficient Perf..... : 1 (enabled)
 - SpeedShift Technology Setup for HWP.. : Yes (setup for HWP)

MSR_TURBO_RATIO_LIMIT............(0x1AD) : 0x25252828
------------------------------------------
 - Maximum Ratio Limit for C01.......... : 28 (4000 MHz) 
 - Maximum Ratio Limit for C02.......... : 28 (4000 MHz) 
 - Maximum Ratio Limit for C03.......... : 25 (3700 MHz) 
 - Maximum Ratio Limit for C04.......... : 25 (3700 MHz) 

IA32_ENERGY_PERF_BIAS............(0x1B0) : 0x5
------------------------------------------
 - Power Policy Preference.............. : 5 (balanced performance and energy saving)

MSR_POWER_CTL....................(0x1FC) : 0x4005E
------------------------------------------
 - Bi-Directional Processor Hot......... : 0 (disabled)
 - C1E Enable........................... : 1 (enabled)

MSR_RAPL_POWER_UNIT..............(0x606) : 0xA0E03
------------------------------------------
 - Power Units.......................... : 3 (1/8 Watt)
 - Energy Status Units.................. : 14 (61 micro-Joules)
 - Time Units .......................... : 10 (976.6 micro-Seconds)

MSR_PKG_POWER_LIMIT..............(0x610) : 0x42816000DD8160
------------------------------------------
 - Package Power Limit #1............... : 44 Watt
 - Enable Power Limit #1................ : 1 (enabled)
 - Package Clamping Limitation #1....... : 1 (allow going below OS-requested P/T state during Time Window for Power Limit #1)
 - Time Window for Power Limit #1....... : 110 (163840 milli-Seconds)
 - Package Power Limit #2............... : 44 Watt
 - Enable Power Limit #2................ : 1 (enabled)
 - Package Clamping Limitation #2....... : 0 (disabled)
 - Time Window for Power Limit #2....... : 33 (10 milli-Seconds)
 - Lock................................. : 0 (MSR not locked)

MSR_PKG_ENERGY_STATUS............(0x611) : 0x262E65
------------------------------------------
 - Total Energy Consumed................ : 152 Joules (Watt = Joules / seconds)

MSR_PP0_POWER_LIMIT..............(0x638) : 0x0

MSR_PP0_ENERGY_STATUS............(0x639) : 0x1ADE34
------------------------------------------
 - Total Energy Consumed................ : 107 Joules (Watt = Joules / seconds)

MSR_PP0_POWER_LIMIT..............(0x638) : 0x0

MSR_PP0_ENERGY_STATUS............(0x639) : 0x1AE008
------------------------------------------
 - Total Energy Consumed................ : 107 Joules (Watt = Joules / seconds)

MSR_PP1_POWER_LIMIT..............(0x640) : 0x0

MSR_PP1_ENERGY_STATUS............(0x641) : 0x295B
------------------------------------------
 - Total Energy Consumed................ : 0 Joules (Watt = Joules / seconds)

MSR_PP1_POLICY...................(0x642) : 0x18
------------------------------------------
 - Priority Level....................... : 24

MSR_CONFIG_TDP_NOMINAL...........(0x648) : 0x12
MSR_CONFIG_TDP_LEVEL1............(0x649) : 0x80050
MSR_CONFIG_TDP_LEVEL2............(0x64a) : 0x1400C8
MSR_CONFIG_TDP_CONTROL...........(0x64b) : 0x0
MSR_TURBO_ACTIVATION_RATIO.......(0x64c) : 0x0
MSR_PKGC3_IRTL...................(0x60a) : 0x884E
MSR_PKGC6_IRTL...................(0x60b) : 0x8876
MSR_PKGC7_IRTL...................(0x60c) : 0x8894
MSR_PKG_C2_RESIDENCY.............(0x60d) : 0x40F8F2A24
MSR_PKG_C3_RESIDENCY.............(0x3f8) : 0x0
MSR_PKG_C2_RESIDENCY.............(0x60d) : 0x40F8F2A24
MSR_PKG_C3_RESIDENCY.............(0x3f8) : 0x0
MSR_PKG_C6_RESIDENCY.............(0x3f9) : 0x0
MSR_PKG_C7_RESIDENCY.............(0x3fa) : 0x0
MSR_PKG_C8_RESIDENCY.............(0x630) : 0x0
MSR_PKG_C9_RESIDENCY.............(0x631) : 0x0
MSR_PKG_C10_RESIDENCY............(0x632) : 0x0
MSR_PKG_C8_LATENCY...............(0x633) : 0x0
MSR_PKG_C9_LATENCY...............(0x634) : 0x0
MSR_PKG_C10_LATENCY..............(0x635) : 0x0

MSR_PLATFORM_ENERGY_COUNTER......(0x64D) : 0x0 (not supported by hardware/BIOS)

MSR_PPERF........................(0x64E) : 0x1FA2D5C7D
------------------------------------------
 - Hardware workload scalability........ : 8492244093

MSR_CORE_PERF_LIMIT_REASONS......(0x64F) : 0x3D001000
------------------------------------------
 - PROCHOT Status....................... : 0
 - Thermal Status....................... : 0
 - Residency State Regulation Status.... : 0
 - Running Average Thermal Limit Status. : 0
 - VR Therm Alert Status................ : 0
 - VR Therm Design Current Status....... : 0
 - Other Status......................... : 0
 - Package/Platform-Level #1 Power Limit : 0
 - Package/Platform-Level #2 Power Limit : 0
 - Max Turbo Limit Status............... : 1 (frequency reduced below OS request due to multi-core turbo limits)
 - Turbo Transition Attenuation Status.. : 0
 - PROCHOT Log.......................... : 0
 - Thermal Log.......................... : 0
 - Residency State Regulation Log....... : 0
 - Running Average Thermal Limit Log.... : 0
 - VR Therm Alert Log................... : 0
 - VR Thermal Design Current Log........ : 0
 - Other Status Log..................... : 1 (status bit has asserted)
 - Package/Platform-Level #1 Power Limit : 1 (status bit has asserted)
 - Package/Platform-Level #2 Power Limit : 1 (status bit has asserted)
 - Max Turbo Limit Log.................. : 1 (status bit has asserted)
 - Turbo Transition Attenuation Log..... : 1 (status bit has asserted)
HDC Supported

IA32_PKG_HDC_CTL.................(0xDB0) : 0x0

IA32_PM_CTL1.....................(0xDB1) : 0x1
------------------------------------------
HDC Allow Block..................(0xDB1) : 1 (HDC blocked)

IA32_THREAD_STALL................(0xDB2) : 0x0

MSR_PKG_HDC_CONFIG...............(0x652) : 0x2
------------------------------------------
Pkg Cx Monitor ..................(0x652) : 2 (count package C3 and deeper)
MSR_CORE_HDC_RESIDENCY...........(0x653) : 0x0

MSR_PKG_HDC_SHALLOW_RESIDENCY....(0x655) : 0x0

MSR_PKG_HDC_DEEP_RESIDENCY.......(0x656) : 0x0

IA32_TSC_DEADLINE................(0x6E0) : 0xA0B7474ED
MSR_PPERF........................(0x63E) : 0x1 (19)

IA32_PM_ENABLE...................(0x770) : 0x1 (HWP Supported and Enabled)

IA32_HWP_CAPABILITIES............(0x771) : 0x1081228
------------------------------------------
 - Highest Performance.................. : 40
 - Guaranteed Performance............... : 18
 - Most Efficient Performance........... : 8
 - Lowest Performance................... : 1

IA32_HWP_INTERRUPT...............(0x773) : 0x1
------------------------------------------
 - Guaranteed Performance Change........ : 1 (Interrupt generated on change of)
 - Excursion Minimum.................... : 0 (Interrupt generation disabled)

IA32_HWP_REQUEST.................(0x774) : 0x282804
------------------------------------------
 - Minimum Performance.................. : 4
 - Maximum Performance.................. : 40
 - Desired Performance.................. : 40
 - Energy Efficient Performance......... : 0
 - Activity Window...................... : 0, 0
 - Package Control...................... : 0

IA32_HWP_STATUS..................(0x777) : 0x0
------------------------------------------
 - Guaranteed Performance Change........ : 0 (has not occured)
 - Excursion To Minimum................. : 0 (has not occured)

CPU Ratio Info:
------------------------------------------
Base Clock Frequency (BLCK)............. : 100 MHz
Maximum Efficiency Ratio/Frequency...... :  4 ( 400 MHz)
Maximum non-Turbo Ratio/Frequency....... : 20 (2000 MHz)
Maximum Turbo Ratio/Frequency........... : 40 (4000 MHz)

IGPU Info:
------------------------------------------
IGPU Current Frequency.................. :    0 MHz
IGPU Minimum Frequency.................. :  300 MHz
IGPU Maximum Non-Turbo Frequency........ :  300 MHz
IGPU Maximum Turbo Frequency............ : 1150 MHz
IGPU Maximum limit...................... : No Limit

P-State ratio * 100 = Frequency in MHz
------------------------------------------
CPU P-States [ (8) 18 37 ] iGPU P-States [ ]
CPU C3-Cores [ 1 6 7 ]
CPU C6-Cores [ 0 1 3 4 5 ]
CPU P-States [ (8) 18 37 ] iGPU P-States [ ]
CPU C3-Cores [ 0 1 5 6 7 ]
CPU C6-Cores [ 0 1 2 3 4 5 ]
CPU C6-Cores [ 0 1 2 3 4 5 6 7 ]
CPU P-States [ (8) 18 21 37 ] iGPU P-States [ ]
CPU C3-Cores [ 0 1 3 4 5 6 7 ]
CPU C3-Cores [ 0 1 2 3 4 5 6 7 ]
CPU P-States [ 8 18 21 (36) 37 ] iGPU P-States [ ]
CPU P-States [ (8) 18 19 21 36 37 ] iGPU P-States [ ]
CPU P-States [ 8 18 19 21 (29) 36 37 ] iGPU P-States [ ]
CPU P-States [ (8) 18 19 20 21 29 36 37 ] iGPU P-States [ ]
CPU P-States [ (8) 18 19 20 21 29 36 37 ] iGPU P-States [ ]
CPU P-States [ (8) 18 19 20 21 29 36 37 ] iGPU P-States [ ]
CPU P-States [ 8 18 19 20 21 29 36 37 (40) ] iGPU P-States [ ]
CPU P-States [ 8 18 19 20 21 22 29 36 (37) 40 ] iGPU P-States [ ]
CPU P-States [ 8 (9) 18 19 20 21 22 29 36 37 40 ] iGPU P-States [ ]
CPU P-States [ 8 9 18 19 20 21 22 29 36 (37) 40 ] iGPU P-States [ ]
```

# 参考教程

## TDP and turbo parameter modification with MSR on non-overclockable CPU

```
https://gist.github.com/Mnkai/5a8edd34bd949199224b33bd90b8c3d4
```

## Is it possible to unlock a clock-locked CPU?

```
https://www.quora.com/Is-it-possible-to-unlock-a-clock-locked-CPU
```

## How To Software OverClock Your CPU With ClockGen Fast And Easy Way to OC

```
https://www.youtube.com/watch?v=7yXW4pqLE70
```

## 如何通过 BIOS 超频 CPU - 英特尔® 官网

```
https://www.intel.cn/content/www/cn/zh/gaming/resources/bios-overclocking.html
```

## Quick CPU - Advanced CPU settings

```
https://coderbag.com/product/quickcpu/features/advanced-cpu-settings
```

## 如何超频您未锁频的英特尔® 酷睿™ 处理器-英特尔®️ 官网

```
https://www.intel.cn/content/www/cn/zh/gaming/resources/how-to-overclock.html
```

## The ThrottleStop Guide

```
http://forum.notebookreview.com/threads/the-throttlestop-guide.531329/page-72
```
