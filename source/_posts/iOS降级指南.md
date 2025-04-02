---
title: iOS降级指南
categories: iOS
abbrlink: iOS-Downgrade-Guide
date: 2023-11-10 14:01:29
tags:

---

![](topic.jpg)

iOS降级指南。

<!-- more -->

# TODOS

```
# OTA延迟升级
# 如果手机受监督，可推迟更新，适用于公司设备的情况。可参考以下内容延迟升级到iOS 14.8（方法已经失效）
https://mp.weixin.qq.com/s/x4aeH2gp3J7E0HHX7s1Zng
https://mp.weixin.qq.com/s/49wiD-MKZg8qM2ejbcvEmA
https://supervise.me/
```

# SHSH2备份

SHSH2是高版本降级必备的文件。SHSH2仅能备份仍然开放验证的系统固件，因此建议每次更新完系统都备份SHSH2。

## 越狱设备

安装System Info插件，然后进入设置-通用-关于本机，在ECID上右滑后点击Save SHSH2即可。

也可用TSSSaver插件，打开后点击`+`-Verify&Save即可保存。通过点击Saved Blobs-here可查看保存的SHSH2。注意该插件的使用需要翻墙，作者源如下。

```
http://repo.nullpixel.uk/
```

## 非越狱设备

通过以下网站即可，设备的ECID可以在iTunes中查看。注意SHSH Host网站备份的SHSH2的G值为0x1111111111111111。

```
https://tsssaver.1conan.com/
https://shsh.host/
```

# 工具

## Futurerestore

适用于高版本系统，需要有SHSH2备份。可在Mac/Windows使用。下载链接如下。

```
https://github.com/marijuanARM/futurerestore
https://github.com/tihmstar/futurerestore
```

### 固定G值

即Generator值。G值在每次手机重启时会重置，所以需保持全程手机未关机。

用记事本打开备份的SHSH2，搜索Generator即可看到文件中的G值。若使用的SHSH2为noapnonce，则G值默认为0x1111111111111111。如果使用的是apnonce，需要将设备G值固定为SHSH2中的值。

以下方法均需确保手机处于越狱状态。

#### 通过越狱软件

若为unc0ver/Chimera/Electra越狱，可直接在手机上打开越狱软件，点击Set nonce设置G值。

#### 通过dimentio

安装Dimentio和NewTerm插件。打开NewTerm，输入以下命令，此处的0x1111111111111111即为G值。

```
su // 密码为alpine
dimentio 0x1111111111111111
```

#### 通过Generator Auto Setter

该插件可自动将设备G值设为特定值。安装插件后设置即可。

### 配置环境

在电脑端需配置好环境。以Mac端为例，可在终端执行以下命令。

```
brew install automake autoconf libtool pkg-config
git clone https://github.com/tihmstar/libirecovery && cd ./libirecovery && bash autogen.sh && make install
git clone https://github.com/tihmstar/libcrippy && cd ./libcrippy && bash autogen.sh && make install
git clone https://github.com/tihmstar/libfragmentzip && cd ./libfragmentzip && bash autogen.sh && make install
cd /usr/local && sudo mkdir ssl && sudo chmod 777 /usr/local/ssl
cd
git clone https://github.com/openssl/openssl.git && cd openssl && ./config && make && sudo make install
brew install curl
brew install openssl
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/Cellar/openssl/1.0.2j/bin/openssl openssl
```

### 固件下载

下载要降级的固件和当前固件，链接如下。

```
https://www.i4.cn/firmware.html
https://act.feng.com/wetools/index.php?r=iosRom/index
https://www.efreelife.com/ipsw/
https://ipsw.me/
```

### 固件降级

将要降级到的固件重命名为restore.ipsw，对应版本的SHSH2文件重命名为blob.shsh2。打开终端并执行以下命令即可。

```
# 带基带文件
./futurerestore -t shsh.shsh2 --latest-baseband --latest-sep restore.ipsw

# 不带基带文件
./futurerestore -t shsh.shsh2 --no-baseband --latest-sep restore.ipsw
```

如果提示找不到SEP Path，则需要手动从当前固件中提取。

用WinRAR打开当前固件，解压出BuildManifest.plist文件，并改名为build.plist。

在Firmware文件夹中找到以bbfw为后缀的基带文件。可能有多个文件符合要求，进入iPhone设置，点击通用-关于本机-调制解调器固件-查看版本号，选择版本号接近的即可。解压出该文件，并改名为bbfw.bbfw。

在all_flash文件夹中找到以sep开头、im4p结尾的SEP文件，将符合手机主板型号的文件解压出，并改名为im4p.im4p，其中主板型号可以在爱思助手中找到，也可通过以下网站的Internal Name找到。

```
https://www.theiphonewiki.com/wiki/Models#iPhone
```

SEP主要负责处理Touch ID、Face ID认证与各种密码的核对工作，一般来说，相同大版本系统的SEP很大几率兼容，跨版本系统的SEP不兼容。

然后使用以下命令即可刷机。

```
./futurerestore -t blob.shsh2 -b bbfw.bbfw -p build.plist -s im4p.im4p -m build.plist -w restore.ipsw
```

若为iPad Wifi版或iPod，由于没有基带文件，命令如下。

```
./futurerestore -t blob.shsh2 --no-baseband -p build.plist -s im4p.im4p -m build.plist -w restore.ipsw
```

<details>
<summary>【进阶】使用图形化界面</summary>

除了用命令行，也可用GUI界面。不需要先下载futurerestore，可直接使用GUI界面下载。

```
https://github.com/CoocooFroggy/FutureRestore-GUI/releases
```
</details>

### 常见问题

#### ERROR: Unable to send iBEC to device.，Failed with errorcode=-8

使用最新版Futurerestore工具。

#### ERROR: Could not open ZIP archive '/var/tmp/ffffffffffffffffffffffffffffffff00000036FhdfWC': 19，Failed with errorcode=-11

尝试更换工具和电脑。

#### Getting SepNonce failed ERROR，Device is in an invalid state

使用Mac版Futurerestore工具。

#### ERROR Could not open ZIP archive (errorcode=-11)

尝试更换电脑。

#### Unable to place device into recovery mode from Normal mode，Failed with errorcode=-2

设置随机数后重新启动或手动进入恢复模式后再尝试刷机，若问题依旧，则完全卸载iTunes所有组件重装iTunes重试。

#### APTicket can't be used for restoring this device，Failed with errorcode=-45

SHSH2无效。

#### APTicket can't be used for this restore，Failed with errorcode=-44

可能是SHSH2文件名错误。若无效，则将后缀名从shsh2改为plist，命令中的对应位置也进行修改。若仍然报错，则SHSH2无效。

#### Unable to enter recovery mode，Failed with errorcode=-2

iTunes驱动或USB数据线问题。更换数据线，完全卸载iTunes所有组件，重启电脑安装最新版iTunes。

#### unsupported devicemode, please put device in recovery mode or normal mode，Failed with errorcode=-3

进入恢复模式重试。

#### ERROR: SEP does not match sepmanifest，Failed with errorcode=-67

SEP文件错误，更换后重试。

#### sep firmware isn't signed，Failed with errorcode=-3

选择了关闭验证的固件中的SEP。选择iOS12.3或以上固件。

#### Unable to place device into recovery mode from Normal mode，Failed with errorcode=-2

iTunes驱动未启动或未正常工作。完全卸载iTunes后安装最新版iTunes重试。

#### can't init, no device found，Failed with errorcode=-3

重新插拔数据线。

#### ERROR: Unable to find any build identities for SEP，Failed with errorcode=-5

buildmanifest文件或SEP文件出现问题。

#### Unable to place device into recovery mode from Normal mode (errorcode=8978449)

设备无法进入正确的恢复模式。手机连接电脑后，电脑端使用爱思助手，手动将设备进入恢复模式，再重新输入指令开始刷机即可。

#### SEP does not match sepmanifest (errorcode=54394897)

SEP不兼容，无法进行刷机。手动提取基带、SEP等文件后重试。

#### failed to reconnect to device in recovery <iBEC> mode (errorcode=65863697)

无法在恢复模式下重新连接到设备，表示设备与电脑断开连接。使用苹果官方原装数据线，更换USB接口，保证良好连接的情况下，再进行刷机。

#### Device ApNonce does not match APTicket nonce

G值不匹配。设备的G值和备份SHSH2文件中的不一致，重新固定G值即可。

#### SEP firmware is Not being signed

所选SEP的固件已经关闭验证，尝试重新从开放验证的固件中提取SEP文件。

## restoreM8

restoreM8是Futurerestore的图形化版本，Futurerestore是用于供iPhone进行刷机操作的命令行软件。下载地址如下，降级方法同上。

```
https://github.com/80036nd/RestoreM8
```

## Pluvia

适用于低版本系统，可在Mac使用。

以iPhone 4为例说明其使用方法。下载Pluvia仓库并解压，准备好要降级的固件，放入Pluvia文件夹中。

```
https://github.com/parrotgeek1/Pluvia
```

保持翻墙条件，打开终端并切换到Pluvia所在目录，然后输入以下命令制作固件。

```
./make_ipsw.sh [固件名称]
```

让手机进入DFU模式，连接手机并输入以下命令进行刷机。

```
./restore.sh [自动生成的固件名称]
```

## ReRa1n

适用于Linux。在终端输入以下命令即可。

```
git clone https://github.com/AidanGamzer/ReRa1n.git
cd ReRa1n
sudo chmod +x install.sh
sudo chmod +x rera1n.sh
./rera1n.sh
```

## Vieux

仅支持部分设备。

```
https://github.com/MatthewPierson/Vieux
```

## turdus merula

支持A9(X)和A10(X)设备。

```
https://sep.lol/
```

# 参考教程

## 利用Futurerestore进行iOS13的升、降级、平刷

```
https://therealketo.github.io/futurerestore-guide/
https://iphoneba.cn/archives/1008.html
```

## Downgrade iOS Firmware using ReRa1n

```
https://fulltip.net/downgrade-ios-firmware-using-rera1n.html
```

## iphone4 8G国行完美降级

```
https://www.feng.com/post/12857525
```

## 降级 iOS10.3.3 方法？并不是想象中容易

```
https://mp.weixin.qq.com/s/VjyE55Vsn5OTISzg4eaeoA
```

## futurerestore ，iOS 系统刷机到指定版本

```
https://mp.weixin.qq.com/s/L3vNBWcWWDUEPuziO0KdWA
```

## 利用Futurerestore进行iOS13的升、降级、平刷

```
https://mp.weixin.qq.com/s/h5EqUG9xxo-_nz9K5b9F-w
```

## futurerestore 指南 ，iOS 系统刷机到指定版本

```
https://mp.weixin.qq.com/s/_faR7J2tvxUunr1ZijdIig
```


## iOS 系统，完美降级工具，终于发布！

```
https://mp.weixin.qq.com/s/1hq7Rlak8SPAUYak4vz1AA
```
