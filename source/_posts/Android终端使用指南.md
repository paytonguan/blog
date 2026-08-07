---
title: Android 终端使用指南
categories: Android
abbrlink: Android-Terminal-Guide
date: 2020-06-03 00:00:00
tags:
---

![](topic.jpg)

在 Android 上使用 Termux 及 Aid Learning 终端工具。

<!-- more -->

# Termux

无需 Root 权限，打开即可在 Android 上运行 Linux。

```
https://github.com/termux/termux-app
```

## 更换软件源

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

## SSH 连接

输入以下命令。

```
# 安装 SSH 服务
pkg install openssh

# 设置密码
passwd

# 查询手机 IP
ifconfig

# 查询当前用户
whoami

# 开启 SSH
sshd -p 9000

# 确认 SSH 服务的监听端口
netstat -ntlp | grep sshd
```

完成后即可根据信息进行连接。

## 运行程序

SD 卡目录的文件不具有可执行权限，需要拷贝到系统的 /home 文件夹并修改权限才可运行，示例如下。

```
cp Demo /home/Demo
cd /home
chmod 777 Demo

# 运行
./Demo
```

## 优化终端

可用 zsh 代替 bash 作为默认 shell。通过以下命令即可。

```
sh -c "$(curl -fsSL https://github.com/Cabbagec/termux-ohmyzsh/raw/master/install.sh)"
```

可通过修改以下文件更改配色。

```
vi ~/termux-ohmyzsh/install.sh
```

## 软件库

### waifu2x

用于图片放大。下载并直接运行即可。

```
https://github.com/tanyiok1234/waifu2x_srmd-ncnn-vulkan-termux-binary
```

## 常用命令

### 访问 SD 卡目录

```
termux-setup-storage
```

## 环境配置

以下为 Termux 本机（无需安装完整 Linux）的环境配置。

### Java

```
pkg install ecj
pkg install dx
```

## 完整 Linux

Termux 不是完整 Linux，但可配合其它 App 实现完整 Linux 的安装。

### 通过 AnLinux

#### 安装

安装 AnLinux 后打开，选择需要安装的 Linux 发行版。复制指令并在 Termux 的终端中运行即可。以 Debian 为例，完成后在 Termux 可通过以下指令启动系统。

```
./start-debian.sh
```

修改以上脚本，将以下行的注释取消即可访问手机自身的存储文件系统。

```
command+=" -b /sdcard"
```

#### 桌面环境配置

安装前需先启动系统。在 AnLinux 中按照步骤操作即可。

完成安装后需要安装 VNC Viewer，然后在 Termux 中运行以下命令以开启 VNC。

```
vncserver-start

# 若出现错误，需要先输入以下指令停止 VNC
vncserver-stop
```

复制输出的地址到 VNC Viewer，此处以`localhost:1`为例，密码为开启 VNC 时所设置的密码。完成后连接即可。

### 通过 atilo

#### 安装

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

以安装 Debian 为例，命令如下。

```
# 安装
atilo install debian

# 卸载
atilo remove debian
```

注意，如果下载失败，需要先通过以下命令在手机的 tmp 目录中删除安装包，否则该脚本会直接跳过下载。

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

#### Debian 环境配置

以下是 atilo 安装的 Debian 系统内的环境配置。

安装 Java：

```
apt search openjdk
apt install default-jdk
```

安装 Python：

```
apt install python3 python3-pip
```

更换 pip 软件源：打开 ~/.pip/pip.conf，修改为以下内容。

```
[global]
index-url =http://pypi.douban.com/simple  
[install]
trusted-host=pypi.douban.com
```

### 完整 Linux 内环境配置

以下环境配置需要在完整 Linux 中完成。

#### Fortran

输入以下命令以修改软件源。

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

以 gcc-8 为例，安装及检验命令如下。

```
apt install gcc-8
gfortran-8 -v
gcc-8 -v
```

#### OpenCL

运行以下命令以安装编译器。

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

CMakeLists.txt 按照以下格式填写，通过 cmake 和 make 即可生成相应文件。

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

# 首先声明动态库的位置
LINK_DIRECTORIES("${PROJECT_SOURCE_DIR}")

SET(HELLO_SRC libOpenCL.so )

# 指定生成目标
add_executable (testopencldemo ${DIR_SRCS})

# 指定具体的动态库的名称
target_link_libraries (testopencldemo ${HELLO_SRC})
```

#### Jupyter

通过以下命令安装。

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

可通过配置文件，避免输入`http://localhost:8888/`后面的一串密钥。

运行以下命令以生成配置文件和密钥。

```
jupyter notebook --generate-config
ipython
from notebook.auth import passwd
passwd()
```

完成密码输入后复制所得内容，修改 data/data/com.termux/files/home/.jupyter 目录下的 jupyter_notebook_config.py，如下。

```
c.NotebookApp.password = 'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed' # 修改为前面所得的内容
c.NotebookApp.ip = '*' # 允许任何 IP 访问
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8888 # 可自行指定端口，访问时使用
```

完成后保存，启动后即可直接通过以下网站访问。

```
http://localhost:8888/

# 电脑端利用 SSH 登录手机后可输入以下网站
[手机IP地址]:8888
```

可运行以下命令以安装 python 扩展包，使编写 python 时有提示符。安装完成后在 Jupyter notebook 中出现 Nbextensions，选择 Hinterland 即可。

```
apt install libxml2
apt install libxslt
pip3 install jupyter-contrib-nbextensions
jupyter contrib nbextension install --user
pip3 install jupyter_nbextensions_configurator
```

#### VS Code

输入以下命令安装 node、npm 和 cnpm。

```
apt install nodejs
apt install npm
npm install -g cnpm --registry=https://registry.npm.taobao.org
cnpm install -g npm
```

用 cnpm 安装 wcode。

```
cnpm install -g wcode
```

使用时通过以下命令启动。

```
wcode -p 9999 /home
```

通过浏览器访问即可。

```
# 手机浏览器
localhost:9999

# 内网电脑
[手机IP地址]:9999
```

也可通过 code-server。下载 arm64 版本的安装包，复制到手机并解压。

```
https://github.com/cdr/code-server/releases
```

解压完成后切换到该目录，并运行以下命令开启 VS Code。通过浏览器访问即可，同上。

```
export PASSWORD=123456
./code-server --auth password --port 9999
```

# Aid Learning

Termux 的国内高仿版。无需安装 Termux，直接下载即可使用。Aid Learning 已自带 VS Code。

## 环境配置

### gfortran

Aid Learning 已自带 gcc/gfortran。若没有可通过以下命令安装。

```
apt install gcc
apt install gfortran
```

### JAVA

以 Java 8 为例，通过以下命令安装。

```
apt install openjdk-8-jdk
apt install openjdk-8-jre
```

输入以下命令查看是否正确安装。

```
java -version
```

若出现错误，则需要修改环境变量。打开 /etc/profile，添加以下内容后保存。

```
export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-arm64"
export CLASSPATH=.$JAVA_HOME/lib/
export JRE_HOME=$JAVA_HOME/jre
export PATH=$JAVA_HOME/bin:$PATH
```

### Jupyter

Aid Learning 已自带 Jupyter。

#### 基本使用

可使用 Jupyter Lab，通过以下命令安装和启动即可。

```
# 安装
pip3 install jupyterlab

# 启动
jupyter lab
```

也可使用 Jupyter Notebook。直接运行以下命令即可。

```
jupyter notebook
```

#### Java 内核

注意，Jupyter 支持的 JDK 版本为 9.0 及以上，而 Aid Learning 内核支持的 JDK 为 JDK8，因此在安装 Java 时需要留意版本。

可安装 IJava 支持套件。通过以下链接下载，然后复制到手机。

```
https://github.com/SpencerPark/IJava/
```

切换到文件所在位置并运行以下命令。

```
unzip ijava.zip -d ijava # 文件名为 ijava.zip
cd ijava
python3 install.py --sys-prefix
```

安装完成后查看 Jupyter 内核，此时应当有 Java。

```
jupyter kernelspec list
```

# 参考教程

> [极致安卓之—Termux安装完整版Linux](https://zhuanlan.zhihu.com/p/95865982)  
> [把安卓手机性能发挥到极致之-Termux](https://zhuanlan.zhihu.com/p/92664273)  
> [把安卓手机性能发挥到极致之-Aid Learning运行Java及性能测试](https://zhuanlan.zhihu.com/p/92489740)  
> [把安卓手机性能发挥到极致之-Termux安装Python及Jupyter](https://zhuanlan.zhihu.com/p/94203587)  
> [安卓手机C/C++开发平台](https://zhuanlan.zhihu.com/p/97882309)  
> [把安卓手机性能发挥到极致之-Termux/Aid Learning使用Fortran](https://zhuanlan.zhihu.com/p/92280533)  
> [极致安卓之—Aid Learning基于Jupyter开发Java和Python](https://zhuanlan.zhihu.com/p/101147592)  
> [把安卓手机性能发挥到极致之-Termux运行Java及性能测试](https://zhuanlan.zhihu.com/p/92471681)  
> [Termux安装Node.JS及网页版文档查看与修改器wcode](https://zhuanlan.zhihu.com/p/106298311)  
> [极致安卓—Termux/Aid Learning安装宇宙最强VS Code](https://zhuanlan.zhihu.com/p/106593146)  
