---
title: Node环境的搭建与使用
categories: Skill
abbrlink: Node-Installation
date: 2019-11-25 21:10:29
tags:
---

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g9as31waswj31c00u013e.jpg)

Node是一个JavaScript运行环境，很多模块都需要依赖node。npm是随同node安装的包管理工具，node安装完成后，npm也安装完成。

<!-- more -->

# 安装

## 通过包管理器

适用于Linux。由于系统默认包含的nodejs版本较低，因此可通过以下命令更新为较新的版本。

```
# 设置为安装14.x版本的nodejs
# 更改14为其它数字（如12）即可安装其它版本的nodejs
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -

# 安装nodejs
sudo apt install nodejs
```

## 通过nvm

仓库如下。

```
// Mac
https://github.com/nvm-sh/nvm

// Windows
https://github.com/coreybutler/nvm-windows
```

以Mac为例，在终端输入以下命令安装`nvm`。

```
// bash用户
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash

// zsh用户
curl -o- [https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh](https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh) | bash [[ -s $HOME/.nvm/nvm.sh ]] && . $HOME/.nvm/nvm.sh
```

安装完成后在终端输入下列命令以安装`node`。

```
// 安装长期支持版本（建议）
nvm install --lts

// 安装最新版本
nvm install node
```

## 通过官方安装包

打开下面官方网站，下载安装包进行安装。

```
https://nodejs.org/en/
```

# 使用

## 更换下载源

### 直接更换为淘宝源

在终端执行以下命令。

```
npm config set -g registry https://registry.npm.taobao.org
```

### 用nrm切换下载源

在终端执行以下命令安装`nrm`，并查看下载镜像源。

```
npm install -g nrm
nrm ls
```

用以下命令切换淘宝源。

```
nrm use taobao
```

## 更新主程序

### 对于nvm安装

```
nvm install --lts --latest-npm --reinstall-packages-from=node
```

### 对于独立程序

```
npm install -g npm
npm install -g node
```

## 更新下载包

```
npm install -g npm-check     # 安装npm-check
npm-check                    # 查看系统插件是否需要升级
npm install -g npm-upgrade   # 安装npm-upgrade
npm-upgrade        # 更新package.json
# 在执行npm-upgrade命令后会要求输入yes或者no，直接输入Yes或Y即可
npm update -g      # 更新全局插件
npm update --save  # 更新系统插件
```

## 查看安装过的模块

```
// 查看当前项目的依赖模块
npm ls --depth 0

// 查看全局依赖模块命令
npm ls -g --depth 0
```

## 删除项目缓存

```
npm cache verify
// 或npm cache clean --force
```

## 删除项目所有依赖

```
npm uninstall *
```

## 代码基础

### 逻辑运算符

==和!=仅判断外在的值，而不判断类型。===和!==会判断外在的值和类型。

```
var a = 1, b = "1";
console.log(a == b);    // 输出true
console.log(a === b);   // 输出false
```

## 常用包

### nodejieba

jieba中文分词库的Nodejs版本。

#### 安装

```
npm install nodejieba
```

#### 使用

##### 基本分词

```
var nodejieba = require("nodejieba");
var result = nodejieba.cut("南京市长江大桥");
console.log(result);
```

##### 自定义词典

```
nodejieba.load({
  dict: nodejieba.DEFAULT_DICT,
  hmmDict: nodejieba.DEFAULT_HMM_DICT,
  userDict: './test/testdata/userdict.utf8',
  idfDict: nodejieba.DEFAULT_IDF_DICT,
  stopWordDict: nodejieba.DEFAULT_STOP_WORD_DICT,
});
```

##### 词性标注

```
var nodejieba = require("nodejieba");
console.log(nodejieba.tag("红掌拨清波"));
```

词性表如下。

| 符号 |     词性     |
|------|--------------|
| Ag   | 形容词语素   |
| a    | 形容词       |
| ad   | 副形词       |
| an   | 形名词       |
| b    | 区别词       |
| c    | 连词         |
| dg   | 副词语素     |
| d    | 副词         |
| e    | 叹词         |
| f    | 方位词       |
| g    | 语素         |
| h    | 前接成分     |
| i    | 成语         |
| j    | 简称略语     |
| k    | 后接成分     |
| l    | 习用语       |
| m    | 数词         |
| Ng   | 名词语素     |
| n    | 名词         |
| nr   | 人名         |
| ns   | 地名         |
| nt   | 机构团体     |
| nz   | 其它专有名词 |
| o    | 拟声词       |
| p    | 介词         |
| q    | 量词         |
| r    | 代词         |
| s    | 处所词       |
| tg   | 时间语素     |
| t    | 时间词       |
| u    | 助词         |
| vg   | 动词语素     |
| v    | 动词         |
| vd   | 副动词       |
| vn   | 名动词       |
| w    | 标点符号     |
| x    | 非语素字     |
| y    | 语气词       |
| z    | 状态词       |
| un   | 未知词       |

##### 关键词抽取

```
var nodejieba = require("nodejieba");
var topN = 4;
console.log(nodejieba.extract("升职加薪，当上CEO，走上人生巅峰。", topN));
```

# 卸载

## 卸载nvm

在终端输入以下命令，移除nvm。

```
cd ~
rm -rf .nvm
```

移除掉`~/.profile`，`~/.bash_profile`， `~/.zshrc`，`~/.bashrc`文件中关于nvm的配置后输入以下命令，让配置文件生效。

```
source ~/.zshrc
source ~/.bash_profile
```

## 卸载node和npm

### 对于nvm安装

```
nvm uninstall [版本号]

# 版本通过以下命令查看
nvm list
```

### 对于独立程序

在终端执行以下命令以卸载。

```
sudo npm uninstall npm -g
sudo rm -rf /usr/local/lib/node /usr/local/lib/node_modules /var/db/receipts/org.nodejs.*
sudo rm -rf /usr/local/include/node /Users/$USER/.npm
sudo rm /usr/local/bin/node
sudo rm /usr/local/share/man/man1/node.1
sudo rm /usr/local/lib/dtrace/node.d
```

输入以下命令以确认卸载是否已完成。

```
node
npm
```

# 常见问题

## 提示package.json不存在

package.json不存在时，可运行以下命令自动创建。

```
npm init
```

package.json存在时，运行以下命令以将package.json的模块安装到node-modules文件夹下。

```
npm install
// 或 npm install –save-dev
```

打开package.json，在最后大括号前添加以下语句，保存并退出。该语句可修复`warning no description / no repository field`的错误。

```
"private": true
```

## 提示找不到<CoreFoundation/CoreFoundation.h>或其它库文件

对于Mac，在终端输入以下命令即可。

```
csrutil disable
sudo mount -uw /
sudo ln -s "$(xcrun --show-sdk-path)/usr/include" /usr/include
export SDKROOT="$(xcrun --show-sdk-path)"
echo "export SDKROOT=\"\$(xcrun --show-sdk-path)\"" >> ~/.bash_profile
sudo DevToolsSecurity -enable
```

# 参考教程

## How to Install Node.js and npm on CentOS 7

```
https://linuxize.com/post/how-to-install-node-js-on-centos-7/
```

## 如何在 Ubuntu 20.04 上安装 Node.js 和 npm

```
https://developer.aliyun.com/article/760687
```

## yanyiwu/nodejieba

```
https://github.com/yanyiwu/nodejieba
```

## nodejs中的==、===、!=、!==的区别

```
https://blog.csdn.net/gisredevelopment/article/details/12979479
```

## jiaba 分词词性标注含义

```
https://blog.csdn.net/weixin_30521649/article/details/95919189
```
