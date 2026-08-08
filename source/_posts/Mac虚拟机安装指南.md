---
title: Mac 虚拟机安装指南
categories: Mac
abbrlink: Mac-VirtualBox-Installation
date: 2019-12-11 00:00:00
tags:
---

![](topic.jpg)

在 Vmware、VirtualBox、KVM 中安装 MacOS 的指南。

<!-- more -->

# Vmware

适用于 Lion 及以上的系统。对于 Lion 以上的系统，可直接挂载 ISO 安装。

安装Vmware Workstation 11/12/14/15，或Vmware Workstation Player 7/12/14/15。Vmware Workstation Player 15下载链接如下。

```
http://www.pc9.com/pc/info-3974.html
```

安装完成后，打开任务管理器，切换到服务选项卡，停止所有与 VM 有关的服务。

通过以下链接下载 Vmware 虚拟机苹果破解补丁 unlocker，解压后右键以管理员身份运行 win-install，完成虚拟机对 macOS 系统限制的破解。

```
https://github.com/paolo-projects/auto-unlocker
https://github.com/DrDonk/unlocker

# 不可用
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

打开 Vmware Workstation Player，创建新虚拟机，系统选择 Mac OS X，安装光盘选择 Mac OS X 的 iso 或 cdr 镜像文件，注意不能是 dmg 格式。cdr 格式的可用镜像示例如下。

```
链接 / https://pan.baidu.com/s/13rD1YbYwSKSDVhIxoUVxHw
提取码 / 4nii
```

创建并完成虚拟机内 Mac 的安装，注意用户密码不能为空。安装完成后，在虚拟机菜单上点击`安装 Vmware Tools for Mac`，即可使 Mac 满屏显示。

点击虚拟机选项-共享文件夹，勾选`总是启用`并设置好共享文件夹，即可完成 Windows 和 Mac 的文件互通。

<details>
<summary>【旧版】对于Snow Leopard及以下系统</summary>

不能直接挂载 ISO，否则会出现`客户端系统不是 Mac OS X Server。`的提示。可通过 Clover 作为中介。

插入带有 Clover 引导的 U 盘，在虚拟机设置好安装光盘，但不要连接到虚拟机。启动虚拟机并将 U 盘连接到虚拟机，然后重启，直至 U 盘被引导。然后连接 CD，此时 Clover 识别到光驱，将自动出现相关引导项，点击即可。

如果 dmg 无法安装，可通过 UltraISO 转换为 ISO 镜像文件。
</details>

# VirtualBox

下载 Clover 的 Bootable ISO。新建虚拟机，类型选择 macOSX，版本选择 Mac 镜像对应的版本。新建完成后编辑设置，点击存储，在`控制器: SATA`下点击光盘的+号，添加两个光驱，一个是 Clover 引导器（SATA 端口 1），一个是系统安装盘（SATA 端口 0）。按照顺序，SATA 端口 1 优先级高于 SATA 端口 0。完成后启动虚拟机，通过 Clover 引导即可。

# KVM

通过 Linux 下的 KVM，可安装 Mac 到虚拟机并通过直通的方式，实现几乎原生的体验。主板需支持虚拟化，对于 Intel 为 Vt-d 和 Vt-x，对于 AMD 为 SVM（Secure Virtual Machine）即安全虚拟机。该方式对硬件厂商无硬性要求。若要直通，需有一张独立显卡和一个可热插拔的 USB 控制器。

首先需要安装 Linux，可选用 Manjaro，下载链接如下。

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

在 basic.sh 的末尾添加以下两行。

```
    -drive id=SystemDisk,if=none,file=MyDisk.qcow2 \
    -device ide-hd,bus=sata.4,drive=SystemDisk \
```

同时可修改 basic.sh 中的第 23 行以修改 MAC 地址，避免 Apple ID 问题。MAC 地址可通过以下命令生成。

```
openssl rand -hex 6 | sed 's/\(..\)/\1:/g; s/:$//'
```

也可修改`-m 2G \`以修改内存大小，修改`-smp 4,cores=2 \`为`-smp cpus=X,cores=X,threads=1,sockets=1 \`以修改 CPU 核数。

然后通过以下命令运行脚本即可启动 Mac 安装。

```
./basic.sh
```

完成后输入以下命令以将配置导入到 Virt-Manager，然后将 MyDisk.qcow2 添加为虚拟硬盘。注意不要通过 Virt-Manager 修改虚拟机参数，可通过`virsh edit`修改，但不要修改 CPU 型号。

```
sudo ./make.sh --add
```

<details>
<summary>【过时】旧方法</summary>

需要的依赖有 libvirt、QEMU、OVMF、Virtual Machine Manager。

打开终端并输入以下命令以启用 KVM 服务。

```
systemctl enable libvirtd
systemctl start libvirtd
```

输入以下命令克隆 Hackintosh-KVM 仓库。

```
https://github.com/PassthroughPOST/Hackintosh-KVM
```

在以下仓库下载 OVMF_CODE.fd 和 OVMF_VARS.fd，并放置到刚才的目录下。

```
https://github.com/kholia/OSX-KVM
```

打开 Example-XML-Files，复制对应平台的配置文件到上一层目录，并更名为 Hackintosh.xml。

打开后删除所有的"value=-object"以及相连的"input"行，然后修改 loader 和 nvram 为刚才下载的两个文件 OVMF_CODE.fd 和 OVMF_VARS.fd 的路径。

在 Mac 环境下载 High Sierra 安装镜像，并通过脚本中的 create_iso_highsierra.sh 创建安装 ISO，具体命令如下。

```
cd [脚本所在目录]
chmod +x ./create_iso_highsierra.sh
./create_iso_highsierra.sh
```

将制作好的 ISO 拷贝到 Linux。在终端输入以下命令启用虚拟机。

```
cd Hackintosh-KVM
sudo virsh define Hackintosh.xml
```

打开 Virtual Machine Manager，即可看到刚才启用的虚拟机。点击虚拟机设置，添加虚拟硬盘并指向 Hackintosh-KVM 文件夹中的 clover.qcow2 文件，添加虚拟光驱并指向制作好的安装 ISO。启动即可开始安装 Mac。安装完成后需要将 Clover 安装到硬盘中。
</details>

在Virtual Machine Manager中修改虚拟机设置，点击Add Hareware-PCI Host Device即可直通显卡和USB控制器。完成直通设置后，需要修改grub的启动参数以告诉Linux需要直通的硬件。具体而言，打开/etc/default/grub，修改如下。

```
# amd_iommu=on表示开启设备IOMMU分组，可输入dmesg | grep -i iommu检查IOMMU是否已正确分组，若不正确则需要更换插槽
# vfio-pci.ids=1b81,10de:10f0为要直通的设备，需要更换为直通的设备ID，可通过lspci -nn查看
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on vfio-pci.ids=1b81,10de:10f0"
GRUB_CMDLINE_LINUX="amd_iommu=on vfio-pci.ids=1b81,10de:10f0"
```

# 参考教程

> [Windows下VMware Workstations Pro15.5.0安装dmg镜像](https://hestyle.blog.csdn.net/article/details/104672651)  

