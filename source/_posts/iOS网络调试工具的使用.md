---
title: iOS网络调试工具的使用
categories: iOS
abbrlink: iOS-Network-Tool-Usage
date: 2023-11-10 11:34:29
tags:

---

![](topic.jpg)

QuantumultX、Loon等。

<!-- more -->

# Thor

应当使用Thor 1.3.4，以保证脚本能够正常使用。

## 安装

在电脑下载以下IPA并用同步推安装到手机。

```
https://code.aliyun.com/zwxsa/iOSbuy/raw/3501cba392d61d91d10b84f0a2524bf179d57ff3/Thor%201.3.4.ipa
```

安装完成后打开，点击闪电按钮以安装证书。若提示需要设备共存验证，则修改系统时间至验证后的时间，再打开Thor即可通过验证。

如果不能通过认证，则卸载1.3.4版本的Thor并安装1.2.0版本的Thor，打开后安装证书和VPN。VPN安装完成后再安装Thor 1.3.4，重新安装证书并删除原来的证书即可，注意不要删除VPN。

```
https://ngxz.baklib.com/3a93/cfbe
```

在使用时不能直接从Thor的主界面打开抓包，需要在主界面中选择好规则后，进入系统设置-VPN，列表中选择Thor条目并开启VPN按钮即可。

## 抓包
### IPA

开启全局抓包后，在助手软件（如爱思助手）下载要抓包的应用，完成下载后回到Thor，打开抓包记录，搜索`ipa`，点击后缀名为ipa的记录，复制链接后打开Shu下载即可。

也可以添加一个过滤器，在`包括关键字`一栏输入`ipa`并存储，以后使用该过滤器抓包即只抓到ipa记录。

### 翻墙节点

开启全局抓包后，打开翻墙软件并进行连接，然后停止抓包，点击抓包记录，找到含有`connect`的记录并进入，切换到`概览`选项卡，点击`消息体`即可。

### 付费音乐

开启全局抓包后，打开音乐软件（如QQ音乐），点击下载后回到Thor。选择抓包记录，后缀为`flac`的即为无损音乐格式。

## 过滤器

过滤器通过更改本地数据进⾏交互欺骗，从⽽达到APP破解等效果。

### 获取

可通过以下捷径。

```
https://www.icloud.com/shortcuts/9097fa1569b6485b816d6fce8f7c8f4c
```

### 制作

以爱字幕APP为例说明制作方法。

#### 查找修改数据

开启Thor全局抓包，打开爱字幕APP，停留一段时间后即可关闭，然后关闭Thor抓包。

进入Thor的抓包记录，寻找含有VIP数据的包，此处为`api/v1.user/info`这一数据。一般而言可寻找user或者info相关的路径。点击响应-application/json，即可进入消息体，摘录如下。复制该消息体的内容。

```
{
    "status":1,
    "msg":"success",
    "result":{
        "id":1641729,
        "token":
        "mobile":"",
        "is_vip":0,
        "nickname":"  ",
        "photorul":"https://www.qlogo.cn",
        "left_times":1,
        "vip_end_time":0,
        "permanent_vip":0,
        "is_enterprise":0,
        "is_new":0,
        "new_user":1
    }
}
```

则目标为将is_vip改为1，且将vip_end_time改为一个非常大的时间戳。

#### 完成修改

退回抓包记录界面，在刚才的那条数据上左滑，点击更多-提取到过滤器，点击+号，点击添加过滤器，填写过滤器名称，并进入`包括域名`，点击+号，选择下面提供的域名。`包括关键字`同理。

点击`挂载断点`，选择编辑-新建-响应消息体回传前-编辑-新建-判断条件，在操作对象里点击`@rep.api`。然后手打以下内容并保存，表示匹配到这条响应。注意`@rep.api`与`CONTAINS[cd]`间要有空格。

```
CONTAINS[cd] "api/v1.user/info"
```

返回上一页，点击编辑-添加表达式（条件满足时）-运算语法，选择`^替换/插入`-`@rsp.bodyText`（表示重写响应消息体），然后点击调试-调试表达式#1。将刚才消息体的内容复制到待匹配文本中，然后在正则表达式中输入需要匹配的修改位置，其中`\d`表示匹配一个数字，`\d+`表示匹配一串数字。

现需匹配`"is_vip":0`，则在正则表达式中输入`"is_vip":\d`，点击匹配，即可找到该位置。然后点击设置替换值，输入`"is_vip":1`，点击替换即可。

`"vip_end_time":0`同理，注意需要退出到匹配动作页，重新执行编辑-添加表达式（条件满足时）的步骤。其中时间戳可用Anubis中的日期格式化工具获得。

制作完成后保存过滤器，选择开启即可。

### 修改

选择一个过滤器并点击`i`-编辑，点击`挂载断点`下的断点，选择查看断点并修改即可。

如果查看断点不可用，说明作者不允许修改该过滤器。为去除限制，点击`i`-导出，导出到Shu后解压到任意位置。打开解压好的文件，点击文件夹内的info.json，打开后选择导出文本-添加到备忘录。打开刚才保存的备忘录，将rights_protected改为0，然后将备忘录导出到Shu，重命名为info.json并覆盖原来的info.json即可。

# Http Catcher

## 安装

1.1.11版本无内购。打开以下链接，选择Http Catcher 1.1.11进行安装。

```
https://ngxz.baklib.com/3a93/cfbe
```

安装完成后打开Http Catcher，打开HTTPS抓包，按照提示操作即可。

## 重写

Http Catcher的重写与Thor的过滤器功能类似。

### 导入

直接将规则文件用Http Catcher打开即可。也可进入设置-重写，点击`+`号，选择`在文本编辑器中编辑`，复制规则内容即可。

### 制作

以泼辣修图APP为例说明制作方法。

#### 查找修改数据

开启Http Catcher全局抓包，进入泼辣修图APP，点击恢复购买，完成后回到Http Catcher停止抓包。

在刚抓到的数据里面找到路径为/v1/payments/appleiap/receipts/confirmation的数据，在响应里面可以看到响应消息体。分析购买前和购买后消息体的不同，可知需将响应状态码从400改为200，并修改`"isUnlimited":true`。

#### 完成修改

复制购买后的数据响应消息体，在需要重写的数据上左滑，点击更多-新建重写，填写名称。点击`位置`，删除PORT中的数值。

点击`添加规则`，类型选择响应，行为选择Body，开启正则表达式，输入正则法则`.*\}`（表示全匹配），在`替换`中输入刚才复制的消息体。正则表达式可通过Anubis进行校验，以确认填写的表达式正确。

完成后继续添加规则，类型选择响应，行为选择响应状态码，查找400并替换为200。保存重写规则并打开即可。




### 重写库

```
https://github.com/pm936/httpcatcher
```

# temperJS

temperJS可以使油猴脚本在手机端运行。

## 安装

可通过以下捷径安装。

```
https://www.icloud.com/shortcuts/7a21b797d6c34a59a3978683dabc6298
```

也可编辑网络调试工具的配置文件。对于Quantumult X，可添加以下重写。

```
hostname=greasyfork.org, openuserjs.org

https:\/\/greasyfork\.org\/scripts\/.*\.user\.js url script-response-body https://raw.githubusercontent.com/Peng-YM/QuanX/master/Rewrites/GreasyFork/greasy-fork.js
```

对于Loon，可添加以下重写。

```
hostname=greasyfork.org, openuserjs.org

http-response ^https:\/\/(greasyfork|openuserjs)\.org\/.*\/.*\.user\.js script-path=https://raw.githubusercontent.com/Peng-YM/QuanX/master/Rewrites/GreasyFork/greasy-fork.js, requires-body=true
```

对于Surge，可添加以下脚本。

```
[MITM]
hostname=greasyfork.org, openuserjs.org

[Script]
Greasy Fork=type=http-response, pattern=^https:\/\/(greasyfork|openuserjs)\.org\/.*\/.*\.user\.js, script-path=https://raw.githubusercontent.com/Peng-YM/QuanX/master/Rewrites/GreasyFork/greasy-fork.js, requires-body=true
```

也可直接添加以下模块。

```
https://raw.githubusercontent.com/Peng-YM/QuanX/master/Rewrites/GreasyFork/Surge.sgmodule
```

## 使用

在手机端的浏览器打开greasyfork的脚本页，点击安装即可自动转换。

# 参考教程

## 你们碎碎念的解锁VIP会员的教程来了

```
https://mp.weixin.qq.com/s/ociRI8wQtRimseAYR1XONw
```

## 网球版解锁某个App永久内购的教程来了

```
https://mp.weixin.qq.com/s/gcJw46HrmJCfBcLSw4s3NA
```
