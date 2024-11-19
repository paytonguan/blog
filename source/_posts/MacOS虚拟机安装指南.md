---
title: MacOS虚拟机安装指南
categories: Mac
abbrlink: Mac-VirtualBox-Installation
date: 2019-12-01 14:24:16
tags:
---

![](topic.jpg)

MacOS的虚拟机安装。

<!-- more -->

# Vmware

适用于Lion及以上的系统。对于Lion以上的系统，可直接挂载ISO安装。

安装Vmware Workstation 11/12/14/15，或Vmware Workstation Player 7/12/14/15。Vmware Workstation Player 15下载链接如下。

```
http://www.pc9.com/pc/info-3974.html
```

安装完成后，打开任务管理器，切换到服务选项卡，停止所有与VM有关的服务。

通过以下链接下载Vmware虚拟机苹果破解补丁unlocker，解压后右键以管理员身份运行win-install，完成虚拟机对macOS系统限制的破解。

```
https://github.com/paolo-projects/auto-unlocker
https://github.com/DrDonk/unlocker

// 不可用
https://github.com/theJaxon/unlocker
```

若过程提示错误，可能是由于文件下载不成功。

```
https://softwareupdate.vmware.com/cds/vmw-desktop/fusion/
```

打开以上链接，选择当前最大版本后下载以下两个文件，放置到unlocker/tools。重新运行程序，可能会出现`A backup folder has been found. Do you wish uninstall the previous patch?.....`的提示，输入Y即可。

```
com.vmware.fusion.zip.tar\com.vmware.fusion.zip\payload\VMware Fusion.app\Contents\Library\isoimages\darwin.iso
com.vmware.fusion.zip.tar\com.vmware.fusion.zip\payload\VMware Fusion.app\Contents\Library\isoimages\darwinPre15.iso
```

打开Vmware Workstation Player，创建新虚拟机，系统选择Mac OS X，安装光盘选择Mac OS X的iso或cdr镜像文件，注意不能是dmg格式。cdr格式的可用镜像示例如下。

```
链接 / https://pan.baidu.com/s/13rD1YbYwSKSDVhIxoUVxHw
提取码 / 4nii
```

创建并完成虚拟机内Mac的安装，注意用户密码不能为空。安装完成后，在虚拟机菜单上点击`安装Vmware Tools for Mac`，即可使Mac满屏显示。

点击虚拟机选项-共享文件夹，勾选`总是启用`并设置好共享文件夹，即可完成Windows和Mac的文件互通。

<details>
<summary>【旧版】对于Snow Leopard及以下系统</summary>

不能直接挂载ISO，否则会出现`客户端系统不是Mac OS X Server。`的提示。可通过Clover作为中介。

插入带有Clover引导的U盘，在虚拟机设置好安装光盘，但不要连接到虚拟机。启动虚拟机并将U盘连接到虚拟机，然后重启，直至U盘被引导。然后连接CD，此时Clover识别到光驱，将自动出现相关引导项，点击即可。

如果dmg无法安装，可通过UltraISO转换为ISO镜像文件。
</details>

# VirtualBox

下载Clover的Bootable ISO。新建虚拟机，类型选择macOSX，版本选择Mac镜像对应的版本。新建完成后编辑设置，点击存储，在`控制器: SATA`下点击光盘的+号，添加两个光驱，一个是Clover引导器（SATA端口1），一个是系统安装盘（SATA端口0）。按照顺序，SATA端口1优先级高于SATA端口0。完成后启动虚拟机，通过Clover引导即可。

# KVM

通过Linux下的KVM，可安装Mac到虚拟机并通过直通的方式，实现几乎原生的体验。主板需支持虚拟化，对于Intel为Vt-d和Vt-x，对于AMD为SVM（Secure Virtual Machine）即安全虚拟机。该方式对硬件厂商无硬性要求。若要直通，需有一张独立显卡和一个可热插拔的USB控制器。

首先需要安装Linux，可选用Manjaro，下载链接如下。

```
https://manjaro.org/
```

完成后打开终端并输入以下命令。

```
sudo apt-get install qemu python python-pip git virt-manager
pip install click request
git clone https://github.com/foxlet/macOS-Simple-KVM.git
cd macOS-Simple-KVM

# 执行本句后默认下载Catalina镜像，可通过./jumpstart --high-sierra/--mojave/--catalina选择版本
./jumpstart.sh

qemu-img create -f qcow2 MyDisk.qcow2 64G
```

在basic.sh的末尾添加以下两行。

```
    -drive id=SystemDisk,if=none,file=MyDisk.qcow2 \
    -device ide-hd,bus=sata.4,drive=SystemDisk \
```

同时可修改basic.sh中的第23行以修改MAC地址，避免Apple ID问题。MAC地址可通过以下命令生成。

```
openssl rand -hex 6 | sed 's/\(..\)/\1:/g; s/:$//'
```

也可修改`-m 2G \`以修改内存大小，修改`-smp 4,cores=2 \`为`-smp cpus=X,cores=X,threads=1,sockets=1 \`以修改CPU核数。

然后通过以下命令运行脚本即可启动Mac安装。

```
./basic.sh
```

完成后输入以下命令以将配置导入到Virt-Manager，然后将MyDisk.qcow2添加为虚拟硬盘。注意不要通过Virt-Manager修改虚拟机参数，可通过`virsh edit`修改，但不要修改CPU型号。

```
sudo ./make.sh --add
```

<details>
<summary>【过时】旧方法</summary>

需要的依赖有libvirt、QEMU、OVMF、Virtual Machine Manager。

打开终端并输入以下命令以启用KVM服务。

```
systemctl enable libvirtd
systemctl start libvirtd
```

输入以下命令克隆Hackintosh-KVM仓库。

```
https://github.com/PassthroughPOST/Hackintosh-KVM
```

在以下仓库下载OVMF_CODE.fd和OVMF_VARS.fd，并放置到刚才的目录下。

```
https://github.com/kholia/OSX-KVM
```

打开Example-XML-Files，复制对应平台的配置文件到上一层目录，并更名为Hackintosh.xml。

打开后删除所有的"value=-object"以及相连的"input"行，然后修改loader和nvram为刚才下载的两个文件OVMF_CODE.fd和OVMF_VARS.fd的路径。

在Mac环境下载High Sierra安装镜像，并通过脚本中的create_iso_highsierra.sh创建安装ISO，具体命令如下。

```
cd [脚本所在目录]
chmod +x ./create_iso_highsierra.sh
./create_iso_highsierra.sh
```

将制作好的ISO拷贝到Linux。在终端输入以下命令启用虚拟机。

```
cd Hackintosh-KVM
sudo virsh define Hackintosh.xml
```

打开Virtual Machine Manager，即可看到刚才启用的虚拟机。点击虚拟机设置，添加虚拟硬盘并指向Hackintosh-KVM文件夹中的clover.qcow2文件，添加虚拟光驱并指向制作好的安装ISO。启动即可开始安装Mac。安装完成后需要将Clover安装到硬盘中。
</details>

在Virtual Machine Manager中修改虚拟机设置，点击Add Hareware-PCI Host Device即可直通显卡和USB控制器。完成直通设置后，需要修改grub的启动参数以告诉Linux需要直通的硬件。具体而言，打开/etc/default/grub，修改如下。

```
# amd_iommu=on表示开启设备IOMMU分组，可输入dmesg | grep -i iommu检查IOMMU是否已正确分组，若不正确则需要更换插槽
# vfio-pci.ids=1b81,10de:10f0为要直通的设备，需要更换为直通的设备ID，可通过lspci -nn查看
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on vfio-pci.ids=1b81,10de:10f0"
GRUB_CMDLINE_LINUX="amd_iommu=on vfio-pci.ids=1b81,10de:10f0"
```

# 参考教程

## Windows下VMware Workstations Pro15.5.0安装dmg镜像

```
https://hestyle.blog.csdn.net/article/details/104672651
```

