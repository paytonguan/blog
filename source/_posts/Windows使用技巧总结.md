---
title: Windows使用技巧总结
categories: Windows
abbrlink: Windows-Skills
date: 2020-05-04 12:55:29
tags:
---

![](topic.jpg)

Windows相关技巧。

<!-- more -->

# TODOS

```
Office 365 E5 账号申请及永久续期教程
https://logi.im/script/permanently-keeping-an-office-e5-account.html
```

# 系统技巧

## 更换系统版本

如将家庭版转为专业版。使用`版本万能转换工具`即可。

```
http://www.winwin7.com/soft/xtgj-8432.html
```

## 获取超级管理员权限

将以下内容保存为reg文件后运行，在右键菜单中即出现超级管理员的菜单。

```
Windows Registry Editor Version 5.00

[-HKEY_CLASSES_ROOT\*\shell\runas]

[HKEY_CLASSES_ROOT\*\shell\runas]
@="获取超级管理员权限"
"Icon"="C:\\Windows\\System32\\imageres.dll,-78"
"NoWorkingDirectory"=""

[HKEY_CLASSES_ROOT\*\shell\runas\command]
@="cmd.exe /c takeown /f \"%1\" && icacls \"%1\" /grant administrators:F"
"IsolatedCommand"="cmd.exe /c takeown /f \"%1\" && icacls \"%1\" /grant administrators:F"

[-HKEY_CLASSES_ROOT\Directory\shell\runas]

[HKEY_CLASSES_ROOT\Directory\shell\runas]
@="获取超级管理员权限"
"Icon"="C:\\Windows\\System32\\imageres.dll,-78"
"NoWorkingDirectory"=""

[HKEY_CLASSES_ROOT\Directory\shell\runas\command]
@="cmd.exe /c takeown /f \"%1\" /r /d y && icacls \"%1\" /grant administrators:F /t"
"IsolatedCommand"="cmd.exe /c takeown /f \"%1\" /r /d y && icacls \"%1\" /grant administrators:F /t"

[-HKEY_CLASSES_ROOT\dllfile\shell]

[HKEY_CLASSES_ROOT\dllfile\shell\runas]
@="获取超级管理员权限"
"HasLUAShield"=""
"NoWorkingDirectory"=""

[HKEY_CLASSES_ROOT\dllfile\shell\runas\command]
@="cmd.exe /c takeown /f \"%1\" && icacls \"%1\" /grant administrators:F"
"IsolatedCommand"="cmd.exe /c takeown /f \"%1\" && icacls \"%1\" /grant administrators:F"

[-HKEY_CLASSES_ROOT\Drive\shell\runas]

[HKEY_CLASSES_ROOT\Drive\shell\runas]
@="获取超级管理员权限"
"Icon"="C:\\Windows\\System32\\imageres.dll,-78"
"NoWorkingDirectory"=""

[HKEY_CLASSES_ROOT\Drive\shell\runas\command]
@="cmd.exe /c takeown /f \"%1\" /r /d y && icacls \"%1\" /grant administrators:F /t"
"IsolatedCommand"="cmd.exe /c takeown /f \"%1\" /r /d y && icacls \"%1\" /grant administrators:F /t"
```

若要取消该菜单，则用以下reg。

```
Windows Registry Editor Version 5.00

[-HKEY_CLASSES_ROOT\*\shell\runas]

[-HKEY_CLASSES_ROOT\Directory\shell\runas]

[-HKEY_CLASSES_ROOT\dllfile\shell]

[-HKEY_CLASSES_ROOT\Drive\shell\runas]

[-HKEY_CLASSES_ROOT\exefile\shell\runas]

[HKEY_CLASSES_ROOT\exefile\shell\runas]
"HasLUAShield"=""

[HKEY_CLASSES_ROOT\exefile\shell\runas\command]
@="\"%1\" %*"
"IsolatedCommand"="\"%1\" %*"
```

## 虚拟Wi-Fi功能

适用于Win7及以上系统。以管理员身份运行命令提示符，运行以下命令以启用虚拟无线网卡。

```
netsh wlan set hostednetwork mode=allow ssid=”Win7 AP WOW!” key=wifimima
```

打开网络与共享中心，进入适配器选项。如果在运行上述命令后没有出现虚拟无线网卡，就说明真实网卡不支持该功能。若出现该网卡，可继续以下操作。

选择已连接到互联网的网络，右键点击属性-共享，勾选`允许其他网络用户通过此计算机的Internet连接来连接`并选择刚才开启的虚拟网卡，同时勾选`允许其他网络用户控制或禁用贡献的Internet连接`。

继续在命令提示符运行以下命令以开启无线网络。开启后连接该无线网络即可。

```
netsh wlan start hostednetwork
```

## 设置Hyper-V

Hyper-V为Windows自带的虚拟机软件。打开控制面板，选择程序和功能-启用或关闭Windows功能，勾选Hyper-V，确定并安装即可。

## 设置Linux子系统

### 安装

进入系统设置，点击更新和安全-针对开发人员，选择开发人员模式。打开控制面板，选择程序和功能-启用或关闭Windows功能，勾选`适用于Linux的Windows子系统`，确定并安装。打开Windows应用商店，搜索Linux，安装并打开Ubuntu。注意需要经常在命令行界面敲回车键，以确认其安装情况。

完成安装后可直接通过打开应用启动Linux，也可打开命令行并输入`bash`。

### 基本配置

#### 更换软件源

依次输入以下命令以备份并打开源配置文件。

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list_backup
sudo vim /etc/apt/sources.list
```

在vim中输入以下命令以替换为阿里云的源地址，并保存退出。

```
:%s#deb http://archive.ubuntu.com/ubuntu/#deb http://mirrors.aliyun.com/ubuntu/#g
:%s#deb http://security.ubuntu.com/ubuntu/#deb http://mirrors.aliyun.com/ubuntu/#g
:wq
```

输入以下命令以更新配置。

```
apt-get update
```

#### 更新为WSL2

Windows 10 19041及更高版本可进行此更新。打开以下链接并点击Linux内核更新包，下载并安装。

```
https://docs.microsoft.com/zh-cn/windows/wsl/wsl2-kernel
```

以管理员身份打开PowerShell，执行以下命令并重启即可。

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
wsl.exe --set-version Ubuntu 2
wsl.exe --set-default-version 2
```

#### WSL1图形界面

##### 安装

打开终端并输入以下命令以安装。

```
sudo apt-get update
sudo apt-get install xorg
sudo apt-get install xfce4
sudo apt-get install xrdp
```

##### 使用

打开终端并输入以下命令。

```
sudo sed -i 's/port=3389/port=3390/g' /etc/xrdp/xrdp.ini
sudo echo xfce4-session >~/.xsession
sudo service xrdp restart
```

在Windows打开`远程桌面连接`，计算机名称为`[本机IP]:3390`，登录即可。注意图形界面运行时需保证终端运行，且`xrdp`服务处于开启状态。

#### WSL2图形界面

##### 安装

打开终端并输入以下命令。

```
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install tasksel -y
sudo tasksel
```

选择`Ubuntu Desktop`，然后用Tab键选择OK，回车确定。

如果突然退出，遇到`tasksel: apt-get failed (100)`，则需要重新运行上面最后一条命令，并且重新选择`Ubuntu Desktop`。安装完成会出现`xserver-xorg install`。

继续输入以下命令。

```
sudo apt-get install -y tigervnc-standalone-server
wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
sudo apt-get install -y dotnet-runtime-3.1
sudo apt-get install -y lightdm
sudo dpkg-reconfigure lightdm
```

选择LightDM并保存。然后输入以下命令，不用设置view-only password。

```
vncpasswd
sudo vncpasswd
sudo -u lightdm vncpasswd
// 如果窗口管理器选择的是GDM，则输入sudo -u gdm vncpasswd
sudo mv /usr/bin/Xorg /usr/bin/Xorg_old
```

通过以下命令编辑Xorg文件。

```
sudo vim /usr/bin/Xorg
```

内容如下，保存并退出。

```
#!/bin/bash
for arg do
  shift
  case $arg in
    # Xvnc doesn't support vtxx argument. So we convert to ttyxx instead
    vt*)
      set -- "$@" "${arg//vt/tty}"
      ;;
    # -keeptty is not supported at all by Xvnc
    -keeptty)
      ;;
    # -novtswitch is not supported at all by Xvnc
    -novtswitch)
      ;;
    # other arguments are kept intact
    *)
      set -- "$@" "$arg"
      ;;
  esac
done

# Here you can change or add options to fit your needs
## command=("/usr/bin/Xvnc" "-geometry" "2736x1824" "-PasswordFile" "${HOME:-/root}/.vnc/passwd" "$@") 
# Because my host monitor resolution matches 1440x860
command=("/usr/bin/Xvnc" "-geometry" "1440x860" "-PasswordFile" "${HOME:-/root}/.vnc/passwd" "$@") 

systemd-cat -t /usr/bin/Xorg echo "Starting Xvnc:" "${command[@]}"

exec "${command[@]}"
```

保存并退出后继续输入以下命令。

```
sudo chmod 0755 /usr/bin/Xorg
curl -s https://packagecloud.io/install/repositories/arkane-systems/wsl-translinux/script.deb.sh | sudo bash
sudo apt-get install -y systemd-genie
sudo mkdir /usr/lib/genie/
```

通过以下命令编辑deviated-preverts.conf文件。

```
sudo vim /usr/lib/genie/deviated-preverts.conf
```

将以下内容写入文件，保存并退出。

```
{
  "daemonize": "/usr/bin/daemonize"
}
```

继续执行以下命令。

```
genie -s
```

通过以下命令编辑lightdm.conf文件。

```
sudo vim /etc/lightdm/lightdm.conf
```

写入以下内容，保存并退出。

```
[LightDM]
start-default-seat=false

[XDMCPServer]
enabled=true
port=177
```

继续执行以下命令。

```
sudo service lightdm restart
```

##### 使用

打开以下链接以下载VcXsrv Windows X Server。

```
https://sourceforge.net/projects/vcxsrv/
```

连接前在Linux的终端输入以下命令。

```
genie -s
hostname -I | awk '{print $1}'
```

记住输出的IP地址。打开VcXsrv，选择One window without titlebar后点击下一步，然后选择Open session via XDMCP后点击下一步。在Connect to host输入框中输入刚才的IP地址后点击下一步。选择Clipboard和Disable access control，取消选择Native opengl，然后点击下一步，完成即可。

## 设置打印机

### 网络打印机安装

将打印机插入网口，配置好IP地址。安装打印机驱动后，在电脑上添加网络打印机，选择没有端口的打印机，再选择不共享这台打印机即可。

如果出现可以Ping通打印机但无法搜索安装打印机的情况，重启打印机即可。

### 本地打印机共享

在连接有打印机的主机上设置打印机为共享，然后进入网络与共享中心，点击更改高级共享设置-启用文件和打印机共享。

在需要连接共享打印机的电脑上打开控制面板，选择程序和功能-启用或关闭Windows功能，勾选打印与文件服务、SMB 1.0/CIFS文件共享支持，确定并安装。

对于普通打印机，在文件管理器输入连接有打印机的主机的IP地址，注意需要添加双反斜杠，即`\\[IP地址]`。正常情况下即访问到电脑资源，若需要输入用户名和密码，则输入主机的用户名和密码即可。进入后双击打印机安装即可，若安装失败，则安装打印机驱动后重试。

对于较老的打印机，打开设备管理器，点击操作-添加过时硬件，选择手动从列表选择，找到打印机并点击下一步。然后选择创建新端口，端口类型为Local Port，端口名称为`\\[IP地址]\[打印机名称]`。添加后选择`从磁盘安装`，选择驱动程序中的INF文件后，在列表中点击对应打印机的型号，完成安装。

### 常见问题

#### 无法打开添加打印机

打开服务选项，找到Print Spooler这个服务的选项，点击启动即可。

#### 无法安装打印机驱动

切换到管理员用户即可。

#### Ping不通网络打印机

检查打印机端口线路是否正常，打印机是否接入正确的端口。若无问题，则刷新打印机IP，开启端口。仍无效，则关闭打印机所有正在打印的任务，然后打开运行，输入spool，找到printers文件夹，删除所有文件，再打开命令提示符并输入以下命令以重启打印机服务即可。

```
net stop spooler
net start spooler
```

## 开启卓越模式电源计划

打开Powershell，执行以下命令即可。

```
powercfg -duplicatescheme e9a42b02-d5df-448d-aa00-03f14749eb61
```

## 删除「添加或删除程序」里的无用选项

Win+R调出运行框，输入regedit，定位到HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall，将要删除的软件对应的键删除即可。

## 登录相关

### 取消按Ctrl+Alt+Del登录

进入控制面板，双击管理工具，选择本地安全策略，点击安全设置-本地策略-安全选项，在右侧选择`交互式登录：无须按Ctrl+Alt+Del`，启用即可。

### 开启自动登录

Win+R打开`运行`，输入`control userpasswords2`后回车，取消勾选`要使用本机，用户必须输入用户名和密码`即可。

## 运行库安装

提示运行库缺失时，可通过AiO Runtimes一键安装。

```
// Windows 2000/XP
https://pan-yz.chaoxing.com/external/m/file/337329473334308864

// Windows Vista及以上
https://pan-yz.chaoxing.com/external/m/file/337350294375264256
```

## 停止自动更新

### 通过删除文件

卸载升级相关软件后，打开系统盘并删除以下文件。

```
C:\$*
C:\Windows10Update
C:\Windows\UpdateAssistant*
C:\Windows\SoftwareDistribution\Download\*
```

打开以下链接，下载Control Panel Plus后打开并取消Windows更新即可。

```
https://www.lanzoux.com/iUuXtez2fde
```

### 通过把网络设为按流量计费的连接

打开网络和Internet属性，选择更改连接属性，打开`设为按流量计费的连接`即可。

### 通过禁止系统自动下载更新程序

在此电脑上右键点击属性，选择高级系统设置-硬件-设备安装设置，在`是否要自动下载适合你设备的制造商应用和自定义图标`选择否即可。

## 破解登录密码

准备Windows 10安装U盘，从U盘启动后点击下一步-修复计算机-疑难解答-命令提示符。用`dir`命令查看系统盘符（假设为C盘），然后输入以下命令。

```
C:
cd Windows/System32
ren sethc.exe AAA.exe
ren cmd.exe sethc.exe
```

重启到登录界面，按五次Shift进入命令行，输入以下命令以启用内置管理员账户并重置密码。

```
net user Administrator /active:yes 
net user Administrator [新密码]
```

进入系统后将Windows/System32下的sethc.exe重命名为cmd.exe，AAA.exe重命名为sethc.exe。

## 桌面美化

可通过以下软件。

```
http://www.apumiao.com/
```

## 重置网络设置

打开命令行并输入以下命令即可。

```
netsh winsock reset(需要管理员身份，输完之后重启)
ipconfig/release
ipconfig/renew
ipconfig/displaydns
ipconfig/flushdns
```

## 降低CPU占用率

按Win+R打开运行，输入services.msc打开服务，禁用HomeGroup开头、Diagnostics开头与Connected User Experiences and Telemetry服务。

按Win+R打开运行，输入regedit打开服务注册表编辑器，点击HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\TimeBroker，找到Start值，双击打开，设置数值数据为4。

关闭Windows更新中的更新来自多个位置。

打开系统设置-个性化-锁屏界面，将Windows聚焦改为其他背景模式。

关闭Windows提示。

## 驱动安装

### 显卡

#### AMD

先卸载Windows自动安装的显卡驱动，再安装AMD官方的显卡驱动即可。ATI Mobility Radeon HD 3400 Series显卡驱动下载链接如下。

```
链接 / https://pan.baidu.com/s/1slDL7Y1
提取码 / heyd
```

## 安装Android子系统

### Windows 11

Windows 11可使用Windows Subsystem for Android运行安卓APP。

在安装前需要先通过Windows Update安装所有的更新。在设置-系统-关于下，操作系统版本需要在`22000.526`及以上，硬盘需要为SSD，Microsoft Store版本需要高于`22110.1402.6.0`，可在Microsoft Store点击头像，选择应用设置，下拉到底查看。

需要通过抓取Microsoft Store中的Windows Subsystem for Android安装包。原安装地址如下。

```
https://www.microsoft.com/store/productId/9P3395VX91NR
```

打开以下抓包网址，复制以上商店链接，通道选择Slow。Fast、Slow、RP、Retail分别对应Windows的Dev渠道、Beta渠道、RP渠道和正式版，目前只有Beta渠道有发布。下载最下面名为`MicrosoftCorporationII.WindowsSubsystemForAndroid_*.msixbundle`的包。

```
https://store.rg-adguard.net/
```

打开PowerShell并运行以下命令，安装下载下来的包即可。

```
add-appxpackage [msixbundle包路径]
```

### Windows 10

Windows 10可使用以下仓库实现。

```
https://github.com/cinit/WSAPatch
```

教程如下。

```
https://www.kdocs.cn/l/ctwrfU5tAOjU
```

### 安装APK

可使用Apk Installer。

```
https://gitee.com/haoyu3/wsainstall-tool
```

## 快速打开网站

进入浏览器快捷方式属性，在目标后空一格并输入网址即可。

## 远程控制

### Mac连接到Windows

在App Store下载Microsoft Remote Deskto‪p，注意需要切换到美区账号。也可通过以下链接。

```
https://apps.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12
```

在Windows下打开系统属性，将远程登录设置允许远程协助，并允许任意版本桌面连接。完成后在Mac的远程登录客户端输入IP或主机名即可完成远程登录。

### Windows连接到Mac

以VNCViewer为例，下载链接如下。

```
https://www.realvnc.com/en/connect/download/viewer/
```

在Mac开启屏幕共享，然后在Windows的VNCViewer输入用户名密码即可。

也可使用TigerVNC，方式与VNCViewer一致，注意在软件设置中开启`Allow loopback connections`，即允许本地接入。

## 批处理脚本

复制以下代码到记事本，并另存为bat文件，使用时双击打开即可。

### 批量修改后缀名

注意，本bat文件对文件夹内的所有文件均适用。

```
# 修改gif为jpg
ren *.gif *.jpg

# 修改所有文件为jpg
ren *.* *.jpg
```

### 可信任站点设置

```
# 可信任站点设置
# 网址及地址，改写成你自己需要设置的网址及地址
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\baidu.com\www" /v http /t REG_DWORD /d 0x00000002 /f
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\8.8.4.8" /v http /t REG_DWORD /d 0x00000002 /f
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\test.com.cn" /v http /t REG_DWORD /d 0x00000002 /f

# 主页设置
reg add "HKEY_CURRENT_USER\Software\Microsoft\Internet Explorer\Main" /v "Start Page" /t reg_sz /d www.google.com /f
reg add "HKEY_CURRENT_USER\Software\Microsoft\Internet Explorer\Main" /v "Default_Page_URL" /t reg_sz /d www.google.com /f
# 主页死锁！
reg add "HKCU\Software\Policies\Microsoft\Internet Explorer\Control Panel" /v HomePage /d 1 /f >nul
# 异议！该指令用于解除死锁！
reg delete "HKCU\Software\Policies\Microsoft\Internet Explorer\Control Panel" /v HomePage /f
pause
```

## Python脚本

### 批量打印

```
import win32api
import win32print
import os
def printer_loading(filename):
    win32api.ShellExecute (
    0,
    "print",
    filename,
    '/d:"%s"' % win32print.GetDefaultPrinter (),
    ".",
    0
    )
path='D:/文件夹'
for filenames in os.listdir(path):
    printer_loading(os.path.join(path,filenames))
```

## 给文件添加备注

可在文件上右键，点击设置-详细信息，部分格式可以在此处添加备注。

对于没有备注一栏的格式，可通过FileMeta添加。打开软件后选择需要添加的格式，左侧选择Simple后点击`Add File Meta Handler`即可。

注意修改后的文件只能使用Windows内置机制复制文件，且均需为NTFS。如果需要添加到压缩包，则在压缩时需要勾选`保存文件流数据`。如果不满足条件，可对修改过的文件点击右键，选择Metadata-Export to XML以导出备注，导入时则选择文件和XML并点击Metadata-Import from XML即可。

```
https://github.com/Dijji/FileMeta
```

如果需要给文件夹添加备注，可通过编辑该文件夹下的desktop.ini，添加以下内容即可。

```
InfoTip=[需要添加的备注]
```

也可使用FolderMeta。

```
https://gitee.com/alwcel/FolderMeta
```

## 主题美化

### 桌面

可通过以下软件。

```
http://www.huoying666.com/
https://www.rainmeter.net/
```

### 指针

指针库如下。

```
http://www.cursors-4u.com/
https://zhutix.com/tag/cursors/
```

### 图标

图标库如下。

```
https://zhutix.com/tag/tubiao/
```

# 常见问题

## 分区表错误导致硬盘无法识别

进入DOS版的Diskgenius并修复分区表即可。注意，WinPE的Diskgenius无效。

## 删除文件显示「访问被拒绝」

对于XP系统，需要先选择菜单栏的工具-文件夹选项-查看，取消勾选`使用简单文件共享`后确定。在需要操作的文件上右键选择属性-安全-高级-所有者，选择`Administrators`并勾选`替换子容器及对象的所有者`，确定即可。

如果仍然不行，则点击安全-添加-高级-立即查找，然后一路确定即可。

## 蓝屏

一般由硬件的驱动与软件程序的不兼容造成程序的冲突而引起，或主机内部系统程序造成硬件执行异常。

### 0xc000014c启动视窗界面失败

计算机确认操作冲突由此造成的数据的不一致，导致系统无法处理异常而崩溃。在PE界面，打开命令提示符，输入如下指令即可。

```
copy C:\windows\system32\config\RegBack\* C:\windows\system32\config
```

# 参考教程

## 安装好电驴插件后，怎么用来下ED2K

```
https://tieba.baidu.com/p/5381182810?red_tag=x2303093348&traceid=
```

## Windows 10下忘记密码的解决方案(本地账户，微软账户通杀)

```
https://www.bugprogrammer.me/2018/05/27/reset-password-for-windows-10.html
```


## 从Ubuntu for WSL 1升级成WSL 2的命令及设WSL 2为默认版本

```
https://ywnz.com/linuxjc/7219.html
```

## WSL2下让Ubuntu20.04安装Gnome桌面

```
https://www.chengxuzhilu.com/2395.html
https://www.most-useful.com/ubuntu-20-04-desktop-gui-on-wsl-2-on-surface-pro-4.html
```

## 用Win7，电脑就是路由器

```
https://hoochanlon.github.io/fq-book/#/append/win7-wifi
```

## 微软PsPing适用于Windows平台的网络测试工具

```
https://51.ruyo.net/15171.html
```

## TCPing 一款检测端口是否开启和TCP延迟的工具

```
https://51.ruyo.net/14528.html
```

## window10怎么降低CPU利用率过高问题

```
https://jingyan.baidu.com/article/d5c4b52b96c251da570dc548.html
```

## win10降低cpu使用率的四种方法

```
http://www.qiuyexitong.com/article/2005.html
```

## windows10下AMD显卡驱动无法安装的解决方法

```
https://jingyan.baidu.com/article/7908e85cb3701aaf481ad2d9.html
```

## Python实现批量打印功能

```
http://www.lanqibing.com/python%E5%AE%9E%E7%8E%B0%E6%89%B9%E9%87%8F%E6%89%93%E5%8D%B0%E5%8A%9F%E8%83%BD/
```

## 构建自己的远程接入系统

```
https://zhuanlan.zhihu.com/p/146548321
```

## EditPlus注册码在线生成，附开源算法代码

```
https://51.ruyo.net/4780.html
```

## [安装 Windows Subsystem for Android™️ ] 安装 Windows Subsystem for Android™️ · GitHub

```
https://gist.github.com/nufeng1999/9ffb42044bc9b239c89180684819cabe
```
