---
title: Sublime 3的使用
categories: Technology
abbrlink: Sublime-Installation
date: 2019-12-24 08:16:29
tags:
---

![](https://tva1.sinaimg.cn/large/006tNbRwly1ga8x7npp86j30v50u0h1g.jpg)

Sublime 3是一款好用的文本编辑软件。

<!-- more -->

# 安装Sublime 3

## 直接安装破解版本

Mac版本可从以下链接获得。

```
https://xclient.info/s/sublime-text.html
```

安装后选择菜单Preferences->settings，在`{}`之间添加以下行以阻止自动更新。

```
"update_check":false
```

## 安装最新版后破解

本方法针对Windows系统。

打开系统hosts文件并添加以下行。hosts文件路径为C:\Windows\System32\drivers\etc。

```
#sublimetext　
127.0.0.1 www.sublimetext.com
127.0.0.1 sublimetext.com
127.0.0.1 sublimehq.com
127.0.0.1 telemetry.sublimehq.com
127.0.0.1 license.sublimehq.com
127.0.0.1 45.55.255.55
127.0.0.1 45.55.41.223
```

关闭sublime text3，打开安装目录，找到sublime_text.exe，备份一份，然后用二进制编辑器打开exe文件，搜索十六进制，输入97940D，替换为000000。将完成后的文件保存，覆盖原来的sublime_text.exe。

然后打开程序，点击菜单Help-Enter Lisence，输入以下注册码。若显示Thanks for Purchase，则成功。

```
----- BEGIN LICENSE -----
TwitterInc
200 User License
EA7E-890007
1D77F72E 390CDD93 4DCBA022 FAF60790
61AA12C0 A37081C5 D0316412 4584D136
94D7F7D4 95BC8C1C 527DA828 560BB037
D1EDDD8C AE7B379F 50C9D69D B35179EF
2FE898C4 8E4277A8 555CE714 E1FB0E43
D5D52613 C3D12E98 BC49967F 7652EED2
9D2D2E61 67610860 6D338B72 5CF95C69
E36B85CC 84991F19 7575D828 470A92AB
------ END LICENSE ------
```

# Package Control的使用与配置

Package Control用于在Sublime 3平台管理插件，项目地址如下。

```
https://packagecontrol.io/installation
```

## 安装

### 新版本安装

新版本在菜单上直接有Install Package Control的选项，点击即可。注意可能要翻墙环境。

### 自动安装

在Sublime 3主界面用Ctrl+`或者通过View-Show Console菜单打开命令行，粘贴以下代码即可。

```
import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

### 手动安装

点击Preferences-Browse Packages菜单，进入打开的目录的上层目录，然后再进入Installed Packages目录。从以下链接下载Package Control.sublime-package并复制到Installed Packages目录，重启Sublime Text。

```
https://sublime.wbond.net/Package Control.sublime-package
```

## 使用

在Sublime 3主界面点击Command/Ctrl+Shift+P，输入install后选中Install Package，回车即可安装插件。

## 常用插件

### x86 and x86_64 Assembly

80×86汇编.asm文件的语法高亮插件。

### ChineseLocalizations

Sublime的语言包。

### VBScript

添加VB语言高亮支持。

### MarkDown Editing

Markdown语法高亮。

### TableEditor

Markdown表格。需先按`Ctrl+Shift+P`，然后打开`Table Editor: Enable for current syntax`、`Table Editor: Enable for current view`和`Table Editor: Set table syntax ... for current view`。

## 常见问题

### 无法显示插件目录

需要翻墙。或者将以下链接的文件下载到本地，然后在Sublime Text中点击Preference-Package Setting-Package Control-User Setting。

```
https://packagecontrol.io/channel_v3.json
```

修改以下代码即可。

```
"channels": [
    "/Users/..." // 以上文件的路径
]
```

### 下载报错

Github采用https协议，而sublime使用urllib，故需对Package Control进行配置。点击Preference-Package Setting-Package Control-User Setting，添加以下代码即可。

```
"downloader_precedence":
{
    "linux":
    [
        "curl",
        "urllib",
        "wget"
    ],
    "osx":
    [
        "curl",
        "wget"
    ],
    "windows":
    [
        "wininet"
    ]
},
```

# 将Sublime作为编译器

Sublime自带编译器，能够对C、C++、Java、Python等语言源代码进行编译。在菜单`工具`-`编译系统`中可以看到当前选择的编译器，一般程序会根据文件后缀自动做出正确的选择。

使用`Ctrl/Command+Shift+B`即跳出编译选项，按照提示进行即可。

# 参考教程

## Sublime text3 Version 3.2.2, Build 3211破解

```
https://www.cnblogs.com/Fluorescence-tjy/p/11910345.html
```
