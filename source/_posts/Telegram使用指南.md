---
title: Telegram使用指南
categories: Skill
abbrlink: Instructions-Of-Telegram
date: 2020-04-24 22:24:29
tags:
---

![](topic.jpg)

Telegram使用指南。

<!-- more -->

# TODOS

```
Sitoi 搭建 Telegram Bot 所写的 Cloudflare 其实就是反向代理了一下 api.telegram.org
本质就是调用 https://api.telegram.org/bot（token）/sendMessage
（https://zhuanlan.zhihu.com/p/59228574）
这个接口的介绍：
https://core.telegram.org/bots/api#sendmessage
chat_id 和 text 是 POST 的必填字段，chat_id 就是对方是谁（一个群组的话就是群组id，一个人的话就是这个用户的id）

因为已经通过 Cloudflare 来 CDN 一下访问 api.telegram.org 了，因此不需要用 proxy 代理来访问，TG_PROXY 就是空了
（TG_PROXY：用一个服务器来访问给定的 TG_API_HOST，也就是开代理
https://www.cheshirex.com/6686.html）


Sitoi 发消息的原理是这样的：
向 api.telegram.org 发一个 POST 请求，调用 sendMessage，请求里面有chat_id 和 text
```

# Telegram Bot

## 基本配置

### 新建空白Bot

在Telegram中搜索BotFather，或直接打开以下链接跳转。

```
https://t.me/botfather
```

点击开始并输入`/newbot`，返回信息后分别输入要新建的Bot名称和用户名，注意用户名最后需要包含Bot或bot。完成后将会生成API，该API将会在后面用到。

### 配置Bot

向BotFather发送以下指令即可。

|       指令      |    作用    |
|-----------------|------------|
| /setuserpic     | 设置头像   |
| /setdescription | 设置描述   |
| /setabouttext   | 设置介绍   |
| /mybots         | 查看机器人 |

### 获取Telegram ID

在Telegram中搜索get_id_bot，或直接打开以下链接跳转。

```
https://t.me/get_id_bot
```

点击start即可获取账号ID。

## 机器人部署

以下需在Telegram新建Bot，并要有一个VPS后再进行。

### 接管微信信息

VPS系统选择Ubuntu 16.04。连接到VPS后输入以下命令。

```
# 安装Python3
sudo add-apt-repository ppa:jonathonf/python-3.6
sudo apt-get update
sudo apt-get install python3.6

# 切换Python3版本
update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.5 1 
update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 2 
update-alternatives --config python3

# 防止出现No module named "apt_pkg"错误
cd /usr/lib/python3/dist-packages/  
sudo cp apt_pkg.cpython-35m-x86_64-linux-gnu.so apt_pkg.cpython-36m-x86_64-linux-gnu.so 

# 安装EFB全套工具需要的依赖
sudo apt-get install python3.6-gdbm python3-pip
sudo apt-get install libtiff5-dev libjpeg8-dev zlib1g-dev libfreetype6-dev liblcms2-dev libwebp-dev tcl8.5-dev tk8.5-dev 
sudo apt-get install git libmagic-dev ffmpeg gcc make autoconf automake libtool python-setuptools python-pip git screen
pip3 install pillow

# 编译安装libwebp
git clone https://github.com/webmproject/libwebp.git
./configure
make
make install

# 安装EFB-ETM-EWS
pip3 install ehforwarderbot 
pip3 install efb-telegram-master
pip3 install efb-wechat-slave

# 创建配置信息存放目录
mkdir -p ~/.ehforwarderbot/profiles/default
mkdir -p ~/.ehforwarderbot/profiles/default/blueset.telegram
```

输入以下命令以新建配置信息。

```
vim ~/.ehforwarderbot/profiles/default/config.yaml
```

内容如下。

```
# token为Bot的API，admins为Telegram账号ID
token: "12345678:1a2b3c4d5e6g7h8i9j"
admins: 
- 102938475
 
master_channel: "blueset.telegram" 
slave_channels: 
- "blueset.wechat" 
```

保存并退出后继续输入以下命令。

```
vim ~/.ehforwarderbot/profiles/default/blueset.telegram/config.yaml 
```

内容如下。

```
# token为Bot的API，admins为Telegram账号ID
token: "12345678:1a2b3c4d5e6g7h8i9j"
admins: 
- 102938475
 
# speech_api为语音功能，不开启则直接删除该部分即可
speech_api:
    # Microsoft (Bing) speech recognition token
    # API key can be obtained from
    # https://azure.microsoft.com/en-us/try/cognitive-services/
    bing: "VOICE_RECOGNITION_TOKEN"
    # Baidu speech recognition token
    # API key can be obtained from
    # http://yuyin.baidu.com/
    baidu:
        app_id: 123456
        api_key: "API_KEY_GOES_HERE"
        secret_key: "SECRET_KEY_GOES_HERE"
        
flags:
    option_one: 10
    option_two: false
    option_three: "foobar"
```

保存并退出后输入以下命令以启动。

```
ehforwarderbot
python3 -m ehforwarderbot
```

完成后在Telegram的Bot中输入`\start`即可开始接受信息。

### 网站RSS订阅机器人

该机器人用于在Telegram群/频道内订阅网站或博客。

VPS系统选择CentOS 7。连接到VPS后输入以下命令。

```
# 更新系统
yum -y update && yum -y install gcc make openssl* pkg* libssl* screen curl

# 安装Rust Nightly以及Cargo
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# 下载编译RSSBot
wget https://github.com/iovxw/rssbot/archive/v1.4.4.tar.gz
tar xvf v1.4.4.tar.gz
cd rssbot-1.4.4
cargo build --release

# 启动rssbot
# 利用screen来维持服务在后台运行
# <token>为机器人的token
cd ./target/release
screen -S rssbot
./target/release/rssbot DATAFILE <token>
```

按Ctrl+A+D退出screen，然后输入以下命令。

```
screen -R rssbot
```

在频道或群中添加用户，添加该Bot，权限为Post Messages。向机器人发送以下命令即可。

```
# 给频道订阅博客
/sub [频道名] [订阅地址]

# 取消频道对应博客的订阅  
/unsub [频道名] [订阅地址]
 
# 查看频道订阅列表
/rss [频道名]
```

# 搜索

```
https://www.sssoou.com/
```

# 参考教程

## 『技术流』Telegram Bot接管WeChat信息

```
https://51.ruyo.net/8054.html
```

## Linux VPS搭建Telegram RSS中文订阅机器人教程

```
https://www.moerats.com/archives/566/
```
