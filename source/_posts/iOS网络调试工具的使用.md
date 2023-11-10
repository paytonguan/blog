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

# 基本概念

## 节点

通常代表一个运行着某个协议程序（SS、SSR、V2ray、Trojan等）的代理服务器，通过代理服务器可以实现流量的中转。

## 策略组与规则

策略组可以包含节点或其他策略组，具有多种不同的策略类型。策略组是节点或其它策略组的集合，嵌套后最终必定会指向一个具体的节点。

策略组根据不同策略，分发规则传递过来的请求。规则决定了当一个请求进来时，如何通过匹配类型进行匹配，以及如何选用对应的策略。规则格式如下。

```
[匹配类型],[匹配关键字],[策略名称]
// 如DOMAIN-SUFFIX,twimg.com,PROXY
```

策略组必须绑定规则才能起作用。规则指定使用哪个策略组，而策略组根据具体需求选择其中某一个节点，结果即该规则指定的网站将被转发到该节点。

## 重写

即URL Rewrite。在接收到HTTP请求时，工具会使用请求的URL去寻找是否存在匹配的重写规则，若满足规则，则会替换或修改HTTP请求中的URL，或替换请求响应体。重写格式如下。

```
[正则表达式],[替换内容],[重写类型]
// 如https?://(www.)?g.cn https://www.google.com 302
// 表示匹配到g.cn或www.g.cn的URL会302重定向到https://www.google.com
```

## MITM解密

Man-in-the-MiddleAttack的缩写，即中间人攻击。中间人攻击方式可以解密https的请求。工具会根据配置的hostname和信任的CA证书解密相应的https请求和响应，解密后可以配合Rule和URL Rewrite进行分流。

## Cron语句

格式如下。

```
[M（分钟，0-59）] [H（小时，0-23）] [D（天，1-31）] [m（月，1-12）] [d（星期，0-6，0为星期天)]
```

特殊符号解释如下。

| 符号 | 说明                   |
| ---- | ---------------------- |
| *    | 所有取值范围的数字     |
| /    | 每（*/5代表每5个单位） |
| -    | 从某个数字到某个数字   |
| ,    | 分开几个离散的数字     |

举例如下。

| 语句             | 说明                                            |
| ---------------- | ----------------------------------------------- |
| 0 */2 * * *      | 每两个小时                                      |
| 0 23-7/2,8 * * * | 23点到7点之间每两个小时，8点        |
| 0 11 4 * 1-3     | 每月的4号和每星期周一到周三的11点 |
| 0 4 1 1 *        | 1月1日早上4点                                   |
| * * * * *        | 每分钟                                          |

可通过以下网站编辑cron语句。

```
https://crontab.guru/
https://tooltt.com/quartzcron/
```

## 配置文件

网络调试工具用于配置自身相关设置的文件。一般包括节点、策略组、分流、重写、脚本、MITM解密等。一般而言，所有编辑都将对配置文件生效，分享配置文件即可分享本机的所有配置。

## 正则表达式

正则表达式是大小写敏感的。

| 语法                    | 说明                                                         |
| ----------------------- | ------------------------------------------------------------ |
| .                       | 匹配除换行符以外的任意字符                                   |
| \|                       | 或运算符，匹配符号前或后的字符                                                   |
| \                       | 转义元字符（用于匹配保留字符，如[ ] ( ) { } . * + ? ^ $ \ \|）                                        |
| `\<`                    | 词首定位（`\<love`）                                         |
| `\>`                    | 词首定位（`love\>`）                                         |
| \w                      | 匹配字母或数字或下划线或汉字                                 |
| \W                      | 匹配任意不是字母、数字、下划线、汉字的字符                   |
| \s                      | 匹配任意的空白符                                             |
| \S                      | 匹配任意不是空白符的字符                                     |
| \d                      | 匹配数字                                                     |
| \D                      | 匹配任意非数字的字符                                         |
| \b                      | 匹配单词的开始或结束                                         |
| \B                      | 匹配不是单词开头或结束的位置                                 |
| \f                  |   匹配一个换页符                     |
| \n                  |   匹配一个换行符                     |
| \r                  |   匹配一个回车符                      |
| \t                  |   匹配一个制表符                     |
| \v                  |   匹配一个垂直制表符                     |
| \p                  |   匹配CR/LF（等同于\r\n），用来匹配DOS行终止符    |
| `\(..\)`                | 匹配稍后将要使用的字符的标签（`\(love\)able\1er`）           |
| ^                       | 匹配字符串的开始                                             |
| $                       | 匹配字符串的结束                                             |
| *                       | 重复零次或更多次                                             |
| +                       | 重复一次或更多次                                             |
| ?                       | 重复零次或一次                                               |
| a\|b                    | 匹配a或b                                                     |
| *?                      | 重复任意次，但尽可能少重复                                   |
| +?                      | 重复1次或更多次，但尽可能少重复                              |
| ??                      | 重复0次或1次，但尽可能少重复                                 |
| {n}                     | 重复n次（`x\{m\}`表示字符x重复出现m次）                      |
| {n,}                    | 重复n次或更多次（`x\{m,\}`表示字符x重复出现m次以上）         |
| {n,}?                   | 重复n次以上，但尽可能少重复                                  |
| {n,m}                   | 重复n到m次（`x\{m,n\}`表示字符x重复出现m到n次）              |
| {n,m}?                  | 重复n到m次，但尽可能少重复                                   |
| (exp)                   | 匹配exp，并捕获文本到自动命名的组里（`love(able|rs)`）       |
| (..)(..)\1\2            | 标签匹配字符（`(love)able\1er`）                             |
| (?<name>exp)            | 匹配exp，并捕获文本到名称为name的组里，也可以写成(?'name'exp) |
| (?:exp)                 | 匹配exp，不捕获匹配的文本，也不给此分组分配组号              |
| (?=exp)                 | 匹配exp前面的位置（正先行断言）                                            |
| (?<=exp)                | 匹配exp后面的位置（正后发断言）                                            |
| (?!exp)                 | 匹配后面跟的不是exp的位置（负先行断言）                                    |
| (?<!exp)                | 匹配前面不是exp的位置（负后发断言）                                        |
|  `[ ]`                  |                              匹配方括号内的任意字符（指定字符集）             |
| [.]                    | 匹配英文句号                                     |
| [^x]                    | 匹配除了x以外的任意字符                                      |
| [^aeiou]                | 匹配除了aeiou这几个字母以外的任意字符                        |
| [a-z]                   | 匹配a-z范围内的一个字符                                      |
| [:alnum:]               | 字母与数字字符（`[[:alnum:]]+`）                             |
| [:alpha:]               | 字母字符，包括大小写（`[[:alpha:]]{4}`）                     |
| [:blank:]               | 空格与制表符（`[[:blank:]]*`）                               |
| [:digit:]               | 数字字母（`[[:digit:]]?`）                                   |
| [:lower:]               | 小写字母（`[[:lower:]]{5,}`）                                |
| [:upper:]               | 大写字母（`[[:upper:]]+`）                                   |
| [:punct:]               | 标点符号（`[[:punct:]]`）                                    |
| [:space:]               | 包括换行符，回车等在内的所有空白（`[[:space:]]+`）           |
| IgnoreCase              | 匹配时不区分大小写                                           |
| Multiline               | 多行模式，更改^和`$`的含义，使它们分别在任意一行的行首和行尾匹配，而不仅仅在整个字符串的开头和结尾匹配。(在此模式下,$的精确含意是匹配\n之前的位置以及字符串结束前的位置) |
| Singleline              | 单行模式，更改.的含义，使它与每一个字符匹配（包括换行符\n）  |
| IgnorePatternWhitespace | 忽略空白，忽略表达式中的非转义空白并启用由#标记的注释        |
| ExplicitCapture         | 显式捕获，仅捕获已被显式命名的组                             |
|   i                 |     忽略大小写                   |
|   g                 |     全局搜索                 |
|   m                 |   多行修饰符：锚点元字符^、$工作范围在每行的起始    |

示例如下。

|     示例    |                                  说明                                  |
|-------------|------------------------------------------------------------------------|
| the         | 匹配the                                                                |
| .ar         | 匹配一个任意字符后面跟着是a和r的字符串                                 |
| [Tt]he      | 匹配the和The                                                           |
| ar[.]       | 匹配ar.                                                                |
| [^c]ar      | 匹配一个后面跟着ar的除了c的任意字符                                    |
| [a-z]*      | 匹配一个行中所有以小写字母开头的字符串                                 |
| \s*cat\s*   | 匹配0或更多个空格开头和0或更多个空格结尾的cat字符串                    |
| c.+t        | 匹配以首字母c开头以t结尾，中间跟着至少一个字符的字符串                 |
| [T]?he      | 匹配字符串he和The                                                      |
| [0-9]{2,3}  | 匹配最少2位最多3位0~9的数字                                            |
| [0-9]{2,}   | 匹配至少两位0~9的数字                                                  |
| [0-9]{3}    | 匹配3位数字                                                            |
| (at\.)$     | 匹配以at.结尾的字符串                                                  |
| /The/gi     | 在全局搜索the和The（i表示忽略大小写，g表示全局搜索）                   |
| /.(at)/g    | 搜索任意字符（除了换行）+at，并返回全部结果                            |
| /at(.)?$/gm | 表示小写字符a后跟小写字符t，末尾可选除换行符外任意字符，匹配每行的结尾 |
| /(.*at)/    | 贪婪匹配模式                                                           |
| /(.*?at)/   | 惰性匹配模式                                                           |
| (c\|g\|p)ar  | 匹配car或gar或par                      |
| (T\|t)he\|car |  匹配(T\|t)he或car                    |
|  \.?   |    选择性匹配.                       |
|  ^(T\|t)he    | 匹配以The或the开头的字符串              |
|  (T\|t)he(?=\sfat)   |  匹配The和the，且其后紧跟着（空格）fat   |
|  (T\|t)he(?!\sfat)   |  匹配The和the，且其后不跟着（空格）fat             |
|  (?<=(T\|t)he\s)(fat\|mat)   |     匹配fat和mat，且其前跟着The或the    |
|  (?<!(T\|t)he\s)(cat)   |  匹配cat，且其前不跟着The或the             |

## 响应码

| 响应码 | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| 200    | 请求被正常处理                                               |
| 204    | 请求被受理，但没有资源可以返回                               |
| 206    | 客户端只是请求资源的⼀部分，服务器只对请求的部分资源执⾏GET⽅法 |
| 301    | 永久性重定向                                                 |
| 302    | 临时重定向                                                   |
| 303    | 与302状态码有相似功能，只是它希望客户端在请求⼀个URI的时候，能通过GET⽅法重定向到另⼀个URI上 |
| 304    | 发送附带条件的请求时，条件不满足时返回，与重定向⽆关         |
| 307    | 临时重定向，与302类似，只是强制要求使⽤POST⽅法              |
| 400    | 请求报⽂语法有误，服务器⽆法识别                             |
| 401    | 请求需要认证                                                 |
| 403    | 请求的对应资源禁⽌被访问                                     |
| 404    | 服务器无法找到对应资源                                       |
| 500    | 服务器内部错误                                               |
| 503    | 服务器正忙                                                   |

# Loon

## 懒人配置

### 通过链接

打开软件，选择配置-编辑-从URL下载，导入以下配置。注意需要翻墙环境。

```
https://raw.githubusercontent.com/nzw9314/Loon/master/Loon.conf
```

在MITM生成安装新的CA证书，进入设备设置-通用-描述文件安装证书，在设置-通用-关于本机-证书信任设置中信任证书，然后即可开启连接开关。

### ~~通过捷径~~

已失效。

<details>
<summary></summary>

```
https://www.icloud.com/shortcuts/5ba2ec8d144a4be188b9037498bbdd26
```
</details>

## 节点

### 手动配置

Loon支持Shadowsocks、ShadowsocksR和Vmess。可通过手动填写、扫描二维码或添加订阅的方式导入节点，也可以通过修改配置文件的方式导入，格式如下。

```
[Proxy]
# SS
# 节点名称=协议,服务器地址,服务器端口,加密协议,密码
1 = Shadowsocks, 1.2.3.4, 443, aes-128-gcm, "password"
2 = Shadowsocks, 1.2.3.4, 443, aes-128-gcm, "password"

# SSR
# 节点名称=协议,地址,端口,加密方式,密码,协议类型,{协议参数},混淆类型,{混淆参数}
3 = ShadowsocksR, 1.2.3.4, 443, aes-256-cfb,"password",auth_aes128_md5,{},tls1.2_ticket_auth,{}
4 = ShadowsocksR, 1.2.3.4, 10076, aes-128-cfb,"password",auth_aes128_md5,{},tls1.2_ticket_auth,{}

# vmess
# 节点名称=协议,服务器地址,端口,加密方式,UUID,传输方式:(tcp/ws),path:websocket握手header中的path,host:websocket握手header中的path,over-tls:是否tls,tls-name:远端服务器域名,skip-cert-verify:是否跳过证书校验（默认否）
5 = vmess,1.2.3.4,10086,aes-128-gcm,"uuid",transport:ws,path:/,host:icloud.com,over-tls:true,tls-name:youtTlsServerName.com,skip-cert-verify:false

# http
# 不支持username/password
ProxyHTTP = http, 1.2.3.4, 8888

[Remote Proxy]
# 订阅节点
# 别名=订阅URL
Subs = https://example/server-complete.txt
```

### 节点筛选

筛选类型如下。

|     类型    |    说明    |
|-------------|------------|
| NodeSelect  | 节点选择   |
| NameKeyword | 关键词筛选 |
| NameRegex   | 正则筛选   |

对于正则筛选，一般可采用以下正则表达式。

|         类型        |    说明    |
|---------------------|------------|
| `(?=.*A)^(?=.*B)^.*$` | 既有A又有B |
| (A)\|(B)            | 有A或B     |
| `^((?!A).)*$ `        | 不含A      |
| `(?=.*A)^((?!B).)^*$` | 有A，没有B |

## 策略组

### 基本分类

策略组由策略类型和子策略构成。策略组会根据策略类型选出流量最终使用的节点。一个策略组可以包含任何单个节点、订阅节点、策略组、内置策略。

Loon支持的策略组如下。

| 策略组名称 | 策略组类型 | 策略组类型分组 |                                                        说明                                                       |                               示例                               |
|------------|------------|----------------|-------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
| 手动选择   | select     | 自定义         | 在节点列表手动选择一个节点或策略组                                                                                | PROXY = select,Auto,1,2,3,4,Subs                                 |
| 延迟测试   | url-test   | 自定义         | 向提供的url发出http header请求，根据返回结果选择测速最快的节点，默认间隔 600s，测速超时时间5s，为避免资源浪费，建议节点数不要过多（不支持嵌套策略组，且只支持单个节点和远端节点，其他会被忽略） | Auto=url-test,1,2,3,4,Subs,url=http://bing.com/,interval=600     |
| 可用测试   | fallback   | 自定义         | 向提供的url发出http header请求，根据返回结果选择第一个可用的节点                                                  | Fallback=fallback,1,2,3,4,Subs,url=http://bing.com/,interval=600 |
| 网络类型   | ssid       | 自定义         | 根据网络类型或路由器名称选择节点或策略组                                                                          | SSID=ssid,default=PROXY,cellular=DIRECT,"SSID名称"=PROXY         |
| 直连       | DIRECT     | 内置           | 不进行代理                                                                                                        | 所有流量不通过代理                                               |
| 拒绝       | REJECT     | 内置           | 阻止请求进行                                                                                                      | 通常用作去广告                                                   |

### 手动配置

可通过主界面的策略组添加，也可通过修改配置文件的方式添加。

```
[Proxy Group]
# 基于用户UI选择
PROXY = select,Auto,1,2,3,4,Subs

# url-test
Auto = url-test,1,2,3,4,Subs,url = http://bing.com/,interval = 600

# fallback
Fallback = fallback,1,2,3,4,Subs,url = http://bing.com/,interval = 600

# 别名 = ssid,默认 = 策略组名,蜂窝 = 策略， ssid名称 = 策略组名
SSID = ssid, default = PROXY, cellular = DIRECT, "DivineEngine" = PROXY

# 广告模式
Advertising = select,REJECT,DIRECT

# 白名单模式PROXY，黑名单模式DIRECT
Final = select,PROXY,DIRECT
```

## 规则分流

### 基本分类

Loon支持的规则如下。

|      匹配名称      |    匹配类型    |                               说明                              |
|--------------------|----------------|-----------------------------------------------------------------|
| 基于域名后缀       | DOMAIN-SUFFIX  |                                                                 |
| 基于域名完整匹配   | DOMAIN         |                                                                 |
| 基于域名关键字     | DOMAIN-KEYWORD |                                                                 |
| 基于用户代理串     | USER-AGENT     | 根据 Http 的 user-agent 值来进行匹配，支持带有*，？的通配符匹配 |
| 基于URL正则        | URL-REGEX      |                                                                 |
| 基于请求IP范围     | IP-CIDR        | 通常用作局域网匹配                                              |
| 基于IP定位国家编码 | GEOIP          | CN中国                                                          |
| 兜底匹配           | FINAL          | 如果没有匹配的规则，默认使用的匹配                              |

Loon支持的策略类型如下。

| 策略类型 |                          说明                         |
|----------|-------------------------------------------------------|
| DIRECT   | 直连，即所有流量不通过代理                            |
| Proxy    | 走特定的策略组，可为[Proxy Group]或[Proxy] 中的自定值 |
| REJECT   | 拒绝，通常用作去广告                              |

### 远程配置

通过配置-订阅规则即可。

### 手动配置

通过配置-单个规则进行添加即可，也可在配置文件中手动添加。

```
[Rule]
# 本地规则
# 规则类型,域名(部分类型可省略),策略
# 可选项:force-remote-dns(Default:false),no-resolve
DOMAIN,google.com,PROXY
# GeoIP为中国的走直连
GEOIP,CN,DIRECT
# 兜底策略使用Final策略组
FINAL,Final

[Remote Rule]
# 远程规则
# URL,策略
# 以下地址将使用PROXY策略组
https://raw.githubusercontent.com/Loon0x00/LoonExampleConfig/master/Rule/ExampleRule.list,PROXY
```

### 规则测试

可通过此功能确认某个域名命中了哪个规则。

## 重写

### 基本分类

Loon支持的重写规则如下。

| 重写名称     | 重写类型 | 说明                                       |
| ------------ | -------- | ------------------------------------------ |
| 替换host字段 | header   | 请求头中匹配的host字段将会被替换内容所替换 |
| 拒绝请求     | reject   |                                            |
| 返回302响应  | 302      |                                            |
| 返回307响应  | 307      |                                            |

## 脚本

### 手动配置

Loon原生支持surge脚本。点击本地脚本-`+`即可写脚本。点击所有脚本-`+`，脚本类型选择cron，即可设置自动运行脚本。支持本地脚本或远程脚本。

配置文件中与脚本相关的配置如下。

```
[Script]
enable = true  

# 类型 值（http-request/response为正则表达式，corn为cron表达式） 参数
# http-request ^https?:\/\/(www.)?(example)\.com script-path=localscript.js
# http-response ^https?:\/\/(www.)?(example)\.com script-path=https://example.com/loon.js timeout=10 requires-body = true
# cron "0 8 * * *" script-path=cron.js
```

各参数含义如下。

|      参数     |                               含义                              |
|---------------|-----------------------------------------------------------------|
| script-path   | 脚本路径，可以是本地脚本地址（暂不支持文件夹）或者远程的URL地址 |
| requires-body | true表示允许修改request和response body的值，默认值为false       |
| timeout       | 执行脚本的最长超时时间，单位为秒                                |

### 脚本编写

#### http-request

表示处理请求的脚本，通过该脚本可以修改http请求。该脚本必须使用`$done({参数})`传参结束，参数可以为空，或为修改后的headers/body。示例如下。

```
# 添加本地脚本，命名为hello-request.js
# 脚本中可用变量为
# $request.url
# $request.headers
# $request.body
let headers = $request.headers;
headers['X-Modified-By'] = 'Loon';

$done({headers});
```

#### http-response

与http-request基本相同，主要区别为http-response表示处理请求响应。示例如下。

```
# 脚本中可用变量为
# $request.url
# $request.headers
# $request.body
# $response.headers
# $response.body
let headers = $response.headers;
headers['X-Modified-By'] = 'Loon';

$done({headers});
```

#### cron

表示定期执行脚本。Loon支持两种类型的cron表达式，如下。

|      类型     |                                        格式                                       |          说明          |
|---------------|-----------------------------------------------------------------------------------|------------------------|
| 5位（精确到分） | `<Minute> <Hour> <Day_of_the_Month> <Month_of_the_Year> <Day_of_the_Week>`          | 分、时、日、月、周     |
| 6位（精确到秒） | `<Seconds><Minute> <Hour> <Day_of_the_Month> <Month_of_the_Year> <Day_of_the_Week>` | 秒、分、时、日、月、周 |

示例如下。

```
# 新建一个本地脚本命名为hello-cron.js，复制下方内容
# 配置文件[Script]添加cron "* * * * *" script-path=hello-cron.js
# 启动Loon，每隔一分钟可看到通知栏提醒

$notification.post("这是主标题"，"这是副标题", "hello-cron!");

console.log("【调试一下】hello-cron");
```

### 公共API

目前支持如下API。

|                                  API                                   |             说明             |
|------------------------------------------------------------------------|------------------------------|
| `$httpClient.get(url/options<Object>, callback<Function>)`               |                              |
| `$httpClient.post(url/options<Object>, callback<Function>)`              |                              |
| `$notification.post('title<String>', 'subTitle<String>','body<String>')` | 通知栏提醒                   |
| `console.log('String')`                                                  | 打印日志                     |
| `$persistentStore.write('data<String>', 'key<String>')`                  | 数据持久化写入，只允许字符串 |
| `$persistentStore.read('key<String>')`                                   | 获取存放的数据               |

$httpClient.get传参为URL的示例如下。

```
$httpClient.get('https://example.com/index', function(result){ console.log(result);});
```

$httpClient.get传参为object的示例如下。

```
var params = {
    url:"https://api.example.com/post",
    header: {
        Host:"api.example.com",
        Content-Type: "application/json"
    },
    body:"string"
}

$httpClient.get(param, function(result){ console.log(result);});
```

## 优先级

### 配置文件

最新版本默认从上往下。

### 规则

本地规则高于远程规则。

DOMAIN-SUFFIX、DOMAIN、DOMAIN-KEYWORD、USER-AGENT按照在配置文件中的位置，通过自上而下的顺序匹配，满足优先匹配。URL-REGEX、IP-CIDR、GEOIP在最后校验。

### 策略组

界面上的排序对于策略组功能无影响。

### 重写

本地重写高于远程重写。

根据正则表达式在配置文件中的位置，通过自上而下的顺序匹配，满足优先匹配。

## URL Schemes

Loon支持的URL Schemes如下。

| URL Schemes                                           | 作用                                s                         |
| ----------------------------------------------------- | ------------------------------------------------------------ |
| loon://switch                                         | 开启或关闭Loon                                               |
| loon://editconfig                                     | 打开Loon配置文件                                             |
| loon://requestLists                                   | 打开最近请求                                                 |
| loon://dnsRequests                                    | 打开DNS                                                      |
| loon://LogLists                                       | 打开日志                                                     |
| loon://import?sub=url | 导入配置，url为urlencode之后的字符串（如loon://import?sub=https%3A%2F%2Floon.now.sh%2Flazy-config%2Fdefault） |

# Quantumult

## 基本操作

### 节点选择

点击主页中下部的图标，可选择节点并进行延迟。在访问的网站没有被规则命中，而又需要以代理方式连接时，将选择此处被选中的节点。注意，延迟与网速并无直接关系。

主页右下角显示当前节点的信息。若出现五角星，代表该节点为中转节点。

## 懒人配置

打开软件，选择设置-下载配置文件，导入以下配置。注意需要翻墙环境。

```
https://raw.githubusercontent.com/lhie1/Rules/master/Quantumult/Quantumult.conf
```

点击设置-HTTPS解密，打开开关并按照流程信任证书即可。

## 节点

Quantumult仅支持obfs的简单混淆，包括http和tls，不支持v2ray-plugin的混淆。
可直接点击设置-服务器添加单个节点，或点击设置-订阅添加订阅。

也可通过修改配置文件的方式，如下。

```
Sample Offline 01 = shadowsocks, 204.79.197.200, 80, chacha20, "password", upstream-proxy=false, upstream-proxy-auth=false, obfs=http, obfs-host="bing.com"
Sample Offline 02 = shadowsocksr, 204.79.197.200, 80, aes-128-cfb, "password", protocol=auth_chain_b, obfs=tls1.2_ticket_fastauth, obfs_param="bing.com"
Sample v2ray = vmess, 127.0.0.1, 443, none, "dd56b429-39d5-3984-87ed-6811f34c3fd3", over-tls=false, certificate=1
```

## 规则分流

### 手动配置

通过设置-订阅-`+`即可。保存后向左滑动条目并点击替换或增加以更新。

对于分流类型，可采用以下链接。

```
https://raw.githubusercontent.com/ConnersHua/Profiles/master/Quantumult/Pro.conf
```

对于链接阻止类型，可采用以下链接。

```
https://raw.githubusercontent.com/ConnersHua/Profiles/master/Quantumult/Rejection.conf
```

## 策略组

### 基本分类

策略组类型如下。

|     类型     |                              说明                             |
|--------------|---------------------------------------------------------------|
| 静态策略     | 策略结果由用户确定                                            |
| 延迟策略     | 选择策略组中延迟最低的节点进行连接                            |
| 负载均衡策略 | 同一策略组的各服务器被轮流使用                                |
| 环境策略     | 根据所处Wi-Fi的SSID或蜂窝网络环境，确定使用哪种策略组进行连接 |

### 使用方法

以下例说明策略组的使用方法。

假设有香港、新加坡、美国三个地区的节点组，其中每个地区的节点组可分为直连的当地主机节点组、上海-当地的IPLC节点组以及深圳-当地的IPLC节点组。根据需要，Youtube选用美国节点，HBO选用香港节点，Netflix任意选择，且选择节点组中最快的一个进行连接。此需要可以通过新建策略组来满足。

选择设置-策略，点击+号。由于需要根据速度选择节点，因此类型选择延迟策略。以建立香港直连节点组为例，填写名称并选择所有香港的直连节点，保存。同理建立上海-香港IPLC节点组和深圳-香港IPLC节点组（第三类节点组）。完成后建立静态策略的策略组，选择刚才新建好的三个策略组，由此可得到一个包含三个节点组的大节点组（第二类节点组）。

对于美国和新加坡也进行此操作，得到三个代表不同地区的大节点组。新建一个静态策略的策略组，选择这三个大节点组，从而得到一个综合所有节点组的超大节点组（第一类节点组）。示意如下。

```
└── 总策略组（静态策略）
    ├── 香港（静态策略）
    │   ├── 直连节点（延迟策略）
    │   ├── 上海-香港IPLC节点（延迟策略）
    │   └── 深圳-香港IPLC节点（延迟策略）
    ├── 新加坡（静态策略）
    │   ├── 直连节点（延迟策略）
    │   ├── 上海-新加坡IPLC节点（延迟策略）
    │   └── 深圳-新加坡IPLC节点（延迟策略）
    └── 美国（静态策略）
        ├── 直连节点（延迟策略）
        ├── 上海-美国IPLC节点（延迟策略）
        └── 深圳-美国IPLC节点（延迟策略）
```

在设置-订阅中左滑分流规则，选择替换。下滑找到Youtube专用节点，选择第二类的美国节点，找到HBO NOW&GO专用节点，选择第二类的香港节点，找到Netflix专用节点，选择第一类的节点，保存即可。

## 其它

### 手机连接WiFi时停止分流

编辑配置文件，在`[SUSPEND-SSID]`一项下输入Wi-Fi名称即可。

# Shadowrocket

可在手机上打开以下链接下载。

```
https://ltribe.lanzous.com/igUjnm6m6de
```

## 基本操作

### 开启代理共享

选择设置-代理-代理共享-启用共享。其它设备与该手机连接到同一Wi-Fi下后，在系统设置中进入Wi-Fi详情，开启Wi-Fi的HTTP代理，类型选择手动，地址和端口填写Shadowrocket中提供的值即可。

### 场景使用

场景配置无法写入配置文件。

场景的使用可以使网络环境变化时（更改Wi-Fi或改成蜂窝数据）使用代理或直连模式。在主界面点击全局路由-设置-场景，新建一个场景，填写Wi-Fi的SSID并选择路由模式即可。

### 自动选择最快节点

点击全局路由-分组，新增分组并选择服务器。完成后返回，打开`启用回退`即可。

## 懒人配置

打开软件，选择配置-添加配置，导入以下配置。注意需要翻墙环境。

```
https://raw.githubusercontent.com/w37fhy/QuantumultX/master/shadowrocket_diy.conf
```

点击远程配置中导入的链接，选择使用配置，等待文件下载到本地。下载到本地后点击本地文件中对应的配置，选择编辑配置-HTTPS解密，打开并按照流程信任证书即可。

## 节点

与其它软件不同的是，Shadowrocket的配置文件中不包含节点信息，节点信息被独立为一个json文件。

通过首页的+号即可添加配置，支持Shadowsocks、ShadowsocksR、Vmess等，也可添加订阅。配置好节点后，可通过数据-导出节点生成json文件。需要导入节点信息时，点击数据-导入节点，选择json文件即可。

## 规则分流

### 手动配置

进入配置-`+`即可配置。

对于分流类型，可采用以下链接。

```
https://raw.githubusercontent.com/ConnersHua/Profiles/master/Shadow/Pro.conf
```

对于去广告类型，可采用以下链接。

```
https://raw.githubusercontent.com/h2y/Shadowrocket-ADBlock-Rules/master/sr_top500_whitelist_ad.conf
```


# Kitsunebi

## 节点

在服务器-`+`即可添加，支持Vmess、Shadowsocks和SOCKS。

## 规则分流

### 手动配置

点击高级-规则集，点击右边的i，修改URL为远程规则集并存储即可。

可使用以下链接。

```
https://raw.githubusercontent.com/aileiys/Kitsunebi/master/Kitsunebi.conf
```

# Surge

## 破解

越狱后下载以下插件并安装即可。

```
https://wws.lanzous.com/tp/i0Ge2dh3oeb
```

## 懒人配置

点击左上角下拉菜单，选择从URL下载配置，复制以下链接并确定即可。完成后若菜单栏出现策略组，则成功。

```
https://raw.githubusercontent.com/nzw9314/Surge/master/Surge_Basic.conf
```

## 节点

### 手动配置

在主界面点击代理服务器，选择添加代理即可添加单个节点，选择新的策略组并勾选使用外部代理列表则可添加订阅。也可直接修改配置文件，在`[proxy]`模块下填写即可。

完成后点击外部资源，下拉到底部并点击立即更新。

对于订阅，Surge无法直接添加订阅节点到proxy模块，应当在现有策略组中添加外部代理列表以添加订阅，在需要引用订阅节点的策略组后添加`policy-path=[订阅链接]`参数即可。注意每个策略组只能绑定一个订阅，对于多订阅则需要先整合为一个订阅链接，或通过一个策略组包含多个订阅策略组的形式。

订阅文件格式与`[proxy]`模块下的写法一致，不包含`[proxy]`本身。注意必须是明文，不能进行base64加密。

### 切换

点击策略组，进入相应策略组，选择需要切换的节点即可。

## 策略组

新建策略组时，在策略组类型中可选择模式，类型和含义与Loon基本一致。

## 规则分流

### 匹配过程

如果一个使用域名的请求遇到了一个IP类型规则（IP-CIDR/IP-CIDR6/GEOIP）且该规则不带有 no-resolve选项，那么将触发DNS查询。

所以应将所有非IP类规则置于规则的顶部，将IP类规则置于尾部，避免并不需要进行本地DNS查询的请求产生额外的DNS查询。

## 脚本

可通过分析-脚本编辑器打开已有脚本进行编辑。

## DNS

在主页面点击DNS服务器即可编辑。

如果经常使用的网络没有DNS劫持问题，配置为使用系统DNS配置并追加223.5.5.5和114.114.114.114作为冗余。如果经常使用的网络存在DNS劫持问题，配置为仅使用223.5.5.5和114.114.114.114。

# Quantumult X

## 正版判断

Quantumult X引入了防共享机制，版本号右侧有绿色√的即为正版。若为问号，则代表被检测到有共享账号行为。

若显示绿色问号，先删除TestFlight测试版，安装App Store最新版并运行，待版本信息显示为绿色√后再安装TestFlight测试版。

若显示红色问号，则证明为盗版。若为非正版，Rewrite和MitM功能将被限制。

关于共享验证，若不开启iCloud，则只能验证八台设备。若开启iCloud，则将有三个iCloud辅助验证名额。使用iCloud验证并无设备数量限制，以首次使用Quantumult X并开启iCloud功能的iCloud为准，注意App Store中的Apple ID与iCloud可以不一致。因此，若所有设备都使用同一个iCloud账号，则无数量限制。

## 基本操作

### 三菱按钮

长按三菱按钮可切换模式，左侧出现的更新按钮可一键更新所有节点。

### 延迟测试

延迟测试可在主页面展开订阅，下拉即可。

也可以长按节点名，选择`网页响应测试`来测对应节点/订阅的延迟，或者点按策略组图标。

### 双排图标

在主界面中，向右滑动策略组图标至最左端后，再次向右滑可变为小图标，再次向右滑可变为双排。

### 隐藏VPN图标

点击三菱图标，选择其他设置，在VPN下开启`排除路由0.0.0.0/31`即可。

### 网络活动日志模块

在主界面中即可看到。该模块可以看到iOS所有网络请求。绿色小锁代表MitM命中域名，链接颜色变红则说明rewrite即重写规则规则生效。

可通过该模块完成抓包行为，也可检查节点、策略、分流、重写、MitM的配置是否起作用。

### 注释符号

`;`、`//`、`#`在配置文件中为注释符号。

### 开启iCloud

点击三菱图标，选择其他设置，开启iCloud即可。

若未开启iCloud，则相关文件将存到我的iPhone/Quantumult X目录，其中脚本将被放于Scripts文件夹。若开启，则变更为iCloud Drive/Quantumult X目录。

### 白色环形标志

若主界面的策略组模块上有白色环形标志，则代表可以长按以添加或删除其中的节点。

### 配置文件快速定位

点击右上角的方向图标按钮，可快速定位到配置文件的不同部分。

## 懒人配置

### 通过链接

点击三菱图标，选择配置文件中的导入，复制以下链接，点击确定。

找到MinM模块，点击生成证书，提示生成成功。点击安装证书并按照流程完成信任即可。

```
https://raw.githubusercontent.com/nzw9314/QuantumultX/master/QuantumultX.conf
```

### 通过托管绑定

托管绑定的配置会自动与远端保持同步，更新时本地配置会被远端配置覆盖。

支持绑定远程、本地、iCloud配置文件。若绑定iCloud配置文件，则APP内所做的修改会立即生效到配置文件本身。

点击三菱图标，选择右上角的关联图标，填入配置链接或文件名。若引用iCloud配置文件，则需要将文件先放到iCloud Drive/QuantumultX/Profiles中，假设为QX01.txt，然后直接填写QX01.txt即可。

### 通过URL Scheme

按照以下JSON方式填写，其中server_remote填写节点订阅链接，filter_remote填写分流规则，rewrite_remote填写复写规则链接。

```
{
    "server_remote": [
        "https://raw.githubusercontent.com/crossutility/Quantumult-X/master/server.txt, tag=Sample-01",
        "https://raw.githubusercontent.com/crossutility/Quantumult-X/master/server-complete.txt, tag=Sample-02"
    ],
    "filter_remote": [
        "https://raw.githubusercontent.com/ConnersHua/Profiles/master/Quantumult/X/Filter/Advertising.list, tag=去广告, enabled=true",
        "https://raw.githubusercontent.com/ConnersHua/Profiles/master/Quantumult/X/Filter/Apple.list, tag=Apple服务, enabled=true",
        "https://raw.githubusercontent.com/ConnersHua/Profiles/master/Quantumult/X/Filter/China.list, tag=国内网站, enabled=true",
        "https://raw.githubusercontent.com/ConnersHua/Profiles/master/Quantumult/X/Filter/DomesticMedia.list, tag=国内视频, enabled=true",
        "http://cloudcompute.lbyczf.com/x-rule-set?url=https://raw.githubusercontent.com/ConnersHua/Profiles/master/Surge/Ruleset/Media/Netflix.list, tag=📺Netflix, force-policy=📺 Netflix, enabled=true",
        "http://cloudcompute.lbyczf.com/x-rule-set?url=https://raw.githubusercontent.com/ConnersHua/Profiles/master/Surge/Ruleset/Media/YouTube.list, tag=🎬Youtube, force-policy=🎬 Youtube, enabled=true",
        "https://raw.githubusercontent.com/ConnersHua/Profiles/master/Quantumult/X/Filter/ForeignMedia.list, tag=国外视频, enabled=true",
        "https://raw.githubusercontent.com/ConnersHua/Profiles/master/Quantumult/X/Filter/Global.list, tag=国外网站, enabled=true",
        "https://raw.githubusercontent.com/ConnersHua/Profiles/master/Quantumult/X/Filter/Hijacking.list, tag=运营商劫持, enabled=true"
    ],
    "rewrite_remote": [
        " http://cloudcompute.lbyczf.com/quanx-rewrite, tag=Lhie1复写"
    ]
}
```

打开以下网站，复制以上代码并点击Encode，复制结果。

```
https://www.urlencoder.org/
```

将复制的结果加到如下格式，并在Safari打开该链接即可。

```
quantumult-x:///update-configuration?remote-resource=[结果]
```

## 通用

### SSID挂起

指定在特定Wi-Fi环境下Quantumult X停止工作，只有`[task]`模块生效。示例如下。

```
[general]
ssid_suspended_list = [Wi-Fi的SSID]
```

### 运行模式切换

可根据网络自动切换分流/直连/全局代理等模式，示例如下。其表示4G网络与普通Wi-Fi下运行filter分流模式，asus-5g 则切换为全局直连模式，asus则为全局代理模式。该模式与手动切换直连/全局代理等效，rewrite和task模块始终生效。

```
[general]
running_mode_trigger=filter, filter, asus-5g:all_direct, asus: all_proxy
```

### 资源解析器

资源解析器可将任何格式的服务器订阅、规则、复写等资源转换成Quantumult X所支持的格式。

#### 添加

在编辑文件中加入以下代码即可。

若提示没有自定义解析器，则长按右下角图标后点击左侧刷新按钮，更新资源，后台退出APP，直到出现解析器说明。

```
[general]
resource_parser_url=https://raw.githubusercontent.com/KOP-XIAO/QuantumultX/master/Scripts/resource-parser.js
```

#### 参数说明

| 参数         | 说明                                                         | 示例                       |
| ------------ | ------------------------------------------------------------ | -------------------------- |
| in/out       | 保留/排除，多参数（逻辑或）用`+`连接，逻辑与用`.`连接        |                            |
| inhn/outhn   | 主机名筛选                                                   |                            |
| regex        | 利用正则表达式以筛选节点                                     |                            |
| emoji=1/2/-1 | 添加或删除节点名中的emoji旗帜（国行设备用 emoji=2）          |                            |
| udp=1, tfo=1 | 开启udp-relay及fast-open（默认关闭，此参数对源类型为QuanX/Surge的链接无效） |                            |
| rename       | rename=旧名@新名，以及`前缀@`，`@后缀`， 用`+`连接           | rename=香港@HK+[SS]@+@[1X] |
| delreg       | 利用正则表达式以删除节点名中的字段                           |                            |
| replace      | 正则替换server中的内容，可用于重命名、更改加密方式等需求     |                            |
| cert=0       | 跳过证书验证（vmess/trojan），即强制tls-verification=false   |                            |
| tls13=1      | 开启tls13=true（vmess/trojan）                               |                            |
| sort=1/-1/ x | 根据节点名正序/逆序/随机排列                                 |                            |
| info=1       | 开启通知提示流量信息（前提是原订阅链接有返回该信息），默认关闭 |                            |
| policy=xx    | 分流规则专用，用于直接指定策略组或为Surge格式的rule-set生成策略组（默认Shawn策略组） |                            |
| ntf=1        | 打开资源解析器的提示通知（默认关闭），rewrite/filter类型则会强制在out参数时开启通知提示被删除（禁用）的内容，以防止规则误删除 |                            |

#### 示例

示例如下。

```
// 远程订阅
// 保留参数in=tls+ss，过滤参数out=http+2
https://raw.githubusercontent.com/crossutility/Quantumult-X/master/server-complete.txt#in=tls+ss&out=http+2

// 分流规则
https://Advertising.list#policy=Shawn&out=aweme
```

### 查询节点信息

可通过JS脚本实现在主界面查询节点信息，在点击相应的节点时出现节点相关内容。代码放到配置文件的`[general]`下即可。

```
[general]
;geo_location_checker用于节点页面的信息展示
geo_location_checker=http://ip-api.com/json/?lang=zh-CN, https://raw.githubusercontent.com/KOP-XIAO/QuantumultX/master/Scripts/IP_API.js
;geo_location_checker=http://api.ipstack.com/check?access_key=1c24147fb534e1a71cb35ff84de2d153&language=zh&output=json, https://raw.githubusercontent.com/KOP-XIAO/QuantumultX/master/Scripts/IPInfo.js
;geo_location_checker=http://ifconfig.co/json,https://raw.githubusercontent.com/KOP-XIAO/QuantumultX/master/Scripts/IPConfig.js
;geo_location_checker=http://extreme-ip-lookup.com/json/,https://raw.githubusercontent.com/KOP-XIAO/QuantumultX/master/Scripts/IPCheck.js
```

### 设置关联图标

设置profile_img_url参数可使关联成功后原关联图标出现图案。图片为108*108的png格式，PNG与png大小写敏感，地址可远程或本地。

```
[general]
profile_img_url= https://raw.githubusercontent.com/KOP-XIAO/QuantumultX/master/img/dragonball/1.PNG
```

### 节点测试

指定用于测试节点速度的网站。

```
[general]
server_check_url=http://www.qualcomm.cn/generate_204
```

### UDP端口白名单

留空则默认为所有端口。不在白名单的端口将被丢弃。

```
[general]
udp_whitelist=53, 123, 1900, 80-443
```

### 其它

```
[general]
;dns exclusion list中的域名将不使用fake-ip方式. 其它域名则全部采用fake-ip 及远程解析的模式
;dns_exclusion_list=*.qq.com, qq.com

;下列表中的内容将不经过QuantumultX的处理
;excluded_routes=192.168.0.0/16, 172.16.0.0/12, 100.64.0.0/10, 10.0.0.0/8
;icmp_auto_reply=true
```

## 节点

对于订阅链接，Quantumult X支持流量使用统计。

### 手动配置

点击三菱图标，选择节点中的添加即可手动输入Shadowsocks节点。选择扫码或URI可导入Shadowsocks、ShadowsocksR或Quantumult X格式的Vmess、Trojan和Https节点。

选择节点中的引用，即可添加订阅。可使用远程订阅链接，也可以使用本地/iCloud文件路径进行引用。

订阅文件格式与`[server_local]`模块下的写法一致，不包含`[server_local]`本身。注意必须是明文，不能进行base64加密。

也可通过修改配置文件的方式导入，格式如下。

```
[server_local]
# []表示可选项
# SS
shadowsocks=example.com:80, method=chacha20, password=pwd, obfs=http, obfs-host=bing.com, [obfs-uri=/resource/file], fast-open=false, udp-relay=false, [server_check_url=http://www.apple.com/generate_204], tag=Sample-A

# SS+WS
shadowsocks=example.com:80, method=aes-128-gcm, password=pwd, obfs=ws, [obfs-uri=/ws], fast-open=false, udp-relay=false, tag=Sample-E

# SS+WS+TLS
shadowsocks=example.com:443, method=aes-128-gcm, password=pwd, obfs=wss, obfs-uri=/ws, fast-open=false, udp-relay=false, tag=Sample-G

# SSR
shadowsocks=example.com:443, method=chacha20, password=pwd, ssr-protocol=auth_chain_b, ssr-protocol-param=def, obfs=tls1.2_ticket_fastauth, obfs-host=bing.com, tag=Sample-D

# Vmess
vmess=example.com:80, method=aes-128-gcm, password=23ad6b10-8d1a-40f7-8ad0-e3e35cd32291, fast-open=false, udp-relay=false, tag=Sample-J

# Vmess+WS
vmess=example.com:80, method=chacha20-ietf-poly1305, password= 23ad6b10-8d1a-40f7-8ad0-e3e35cd32291, obfs-host=ws-c.example.com, obfs=ws, obfs-uri=/ws, fast-open=false, udp-relay=false, tag=Sample-H

# Vmess+WS+TLS
vmess=ws-tls-b.example.com:443, method=chacha20-ietf-poly1305, password= 23ad6b10-8d1a-40f7-8ad0-e3e35cd32291, obfs-host=ws-tls-b.example.com, obfs=wss, obfs-uri=/ws, tls-verification=true,fast-open=false, udp-relay=false, tag=Sample-I

# Vmess+over TLS
vmess=example.com:443, method=none, password=23ad6b10-8d1a-40f7-8ad0-e3e35cd32291, obfs-host=vmess-over-tls.example.com, obfs=over-tls, tls-verification=true, fast-open=false, udp-relay=false, tag=Sample-L

# Http
http=http.example.com:80, username=name, password=pwd, fast-open=false, udp-relay=false, tag=http

# Https
http=https.example.com:443, username=name, password=pwd, over-tls=true, tls-verification=true, tls-host=example.com, fast-open=false, udp-relay=false, tag=http-tls

# Trojan+TLS
trojan=192.168.1.1:443, password=pwd, over-tls=true, [tls-host=example.com], tls-verification=true, fast-open=false, udp-relay=false, tag=trojan-tls

[server_remote]
# 远程订阅
https://raw.githubusercontent.com/crossutility/Quantumult-X/master/server.txt, tag=example1, enabled=true
# 本地文件
server.txt, tag=example2, enabled=true
```

### 分组图标

在配置文件中找到要添加图标的订阅链接，添加img-url参数即可，示例如下。注意图标格式必须为png，且分辨率必须为`108*108`像素。

```
img-url=https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/IPLC.png
```

## 策略组

### 基本分类

Quantumult X支持的策略组类型如下。

|     名称    |      类型      |                                           说明                                           |
|-------------|----------------|------------------------------------------------------------------------------------------|
| Static      | 静态策略组     | 可以嵌套其它所有类型的策略组，需手动选择路线/子策略组（需至少配置一个节点）          |
| Available   | 健康检查策略组 | 只可直接套用节点，不可嵌套其它策略组，自动选择第一个可用的节点（需至少配置两个节点）     |
| Round-Robin | 轮询策略组     | 只能直接套用节点，不可以嵌套其它策略组，按网络请求轮流使用所有节点（需至少配置两个节点） |
| SSID        | SSID策略组     | 可以套用其它类型的策略组，根据网络/Wi-Fi切换节点/策略                                    |

### 手动配置

在节点订阅列表中右滑，选择更多，即可将该订阅链接内所有节点直接绑定生成一个新策略组。支持生成static静态策略、available健康检查策略、round-robin负载均衡策略。该方式生成的策略组将与订阅链接绑定，节点也跟随订阅链接改变。也可通过配置文件的方式生成，在`[server-remote]`中填写即可，示例如下。

```
[server-remote]
# 生成Hong Kong的策略组, 类型是static, 由as-policy决定
https://xxxx.server.com, tag=Hong Kong, as-policy=static, enabled=true
```

若需要直接创建策略组，可直接编辑配置文件。示例如下。

```
[policy]
# 静态策略组
static=policy-name-1, Sample-A, Sample-B, Sample-C

# 可用性策略组
available=policy-name-2, Sample-A, Sample-B, Sample-C

# 轮询策略组
round-robin=policy-name-3, Sample-A, Sample-B, Sample-C

# SSID策略组
# ssid=组名,4g下默认策略,Wi-Fi下默认策略,wifi-A:策略A,wifi-B:策略B
ssid = SSID Group, HK Group, HK Group, ASUS_5G:MO Group, AMG-5G:direct
```

直接创建策略组时，还可以通过正则表达式参数，将订阅/节点的一部分添加到策略组中，支持static静态策略、available健康检查策略、round-robin负载均衡策略。当筛选结果为空时，将自动设为直连模式。注意，若生成available健康检查策略组，则仅可包含一个节点，不可再混搭节点或其余策略组。

正则表达式参数说明如下。

|        参数        |             说明            |
|--------------------|-----------------------------|
| resource-tag-regex | 根据订阅名（tag）来筛选节点 |
| server-tag-regex   | 根据节点名来筛选节点        |

常用正则表达式如下。

|         类型        |    说明    |
|---------------------|------------|
| `(A).*(B)` | 既有A又有B |
| (A)\|(B)            | 有A或B     |
| `^((?!A).)*$`        | 不含A      |
| `(?!.*(A)).*(B)` | 没有A，有B |

示例如下。

```
[policy]
static=policy-name, resource-tag-regex=^sample, server-tag-regex=^example
```

### 分组图标

配置文件中相关配置末尾添加`img-url=`参数即可，与节点添加分组图标方法一致。

## 规则分流

### 基本分类

Quantumult X支持的分流策略如下。

| 类型         | 说明               | 示例                                                       |
| ------------ | ------------------ | ---------------------------------------------------------- |
| HOST         | 完整域名匹配       | limbopro.xyz                                               |
| HOST-SUFFIX  | 域名后缀匹配       |                                                            |
| HOST-KEYWORD | 域名关键字匹配     | limboppro                                                  |
| USER-AGENT   | 浏览器用户代理匹配 | *abc?                                                      |
| IP-CIDR      | 无类别域间路由     | 192.168.x.x                                                |
| GEOIP        | GeoIP数据库IP匹配  | 参数填US，则为美国IP数据库匹配，所有美国IP匹配该规则则执行 |

### 手动配置

对于单条分流规则，可点击三菱按钮，选择分流中的添加即可。也可以在网络请求记录中找到对应记录，点击右上角漏斗符号直接添加。

对于订阅，可点击三菱按钮，选择分流中的引用，点击+号即可添加。注意，对于同一请求的不同规则，在配置文件中位置较上的分流规则优先生效。

对于规则类型，可采用以下链接。

```
https://raw.githubusercontent.com/lhie1/Rules/master/Quantumult/QuantumultX.conf
https://raw.githubusercontent.com/shigalin/Config/master/QuantumultX.conf
```

对于规则类型，可采用以下链接。注意需开启策略偏好，并选择REJECT，否则将不会替换原有策略组，而自动创建新的策略组。

```
https://raw.githubusercontent.com/NobyDa/Script/master/QuantumultX/AdRule.list
https://raw.githubusercontent.com/NobyDa/Script/master/QuantumultX/AdRuleTest.list
```

配置文件的相关内容如下。

```
[filter_remote]
# 可使用force-policy来强制使用策略偏好，参数为要替换的策略组名
# 远程订阅
https://raw.githubusercontent.com/ConnersHua/Profiles/master/Quantumult/X/Filter/Advertising.list, tag=去广告, enabled=true

# 本地文件
advertising.txt, tag=🚦去广告，forcr-policy=reject, enabled=true
```

## 重写

### 手动配置

对于本地重写，点击三菱图标，选择重写-添加即可。对于订阅，则选择重写-引用-`+`，复制链接并点击确定即可。

对于脚本类型，可采用以下链接。

```
https://raw.githubusercontent.com/NobyDa/Script/master/QuantumultX/Js.conf
```

对于去广告类型，可采用以下链接。

```
https://raw.githubusercontent.com/NobyDa/Script/master/QuantumultX/Rewrite_lhie1.conf
```

配置文件的相关内容如下。

```
[rewrite_remote]
# 远程链接
https://raw.githubusercontent.com/ConnersHua/Profiles/master/Quantumult/X/Rewrite.conf, tag=神机复写规则，enabled=true

# 本地/iCloud引用
rewrite.txt, tag=本地复写, enabled=true

[rewrite_local]
^http://example\.com/resource1/1/ url reject
^http://example\.com/resource1/2/ url reject-img
```

### 脚本类型说明

#### 配置

重写功能可对特定链接调用脚本，从而实现去广告、破解VIP等功能。配置文件的相关内容如下。

```
[rewrite_remote]
# 远程链接
# 匹配网址 url 复写类型 脚本文件
http://example\.com/resource5/ url script-response-body https://raw.githubusercontent.com/crossutility/Quantumult-X/master/sample-rewrite-with-script.js

[rewrite_local]
# 本地文件
http://example\.com/resource5/ url script-response-body script_name.js
```

#### 查看

在主界面选择重写规则，搜索response-body即可查看所有脚本类型的重写。


## 任务

### 手动配置

Quantumult X暂时不支持远程订阅任务脚本，即没有`[task_remote]`模块。因此需要将每一条任务脚本写到配置文件，但脚本的路径可以为本地或远程。

在配置文件中手动添加`[task_local]`模块，并按照格式填写即可，示例如下。

```
[task_local]
# 远程脚本
# * * * * *为cron语句
* * * * * http://example.com/name.js, tag=京东签到

# 本地文件
* * * * * name.js, tag=京东签到
```

如果是签到类task任务，一般还需要先获取对应的cookie。脚本内一般会有使用说明。

### 批量添加

点击三菱按钮，进入`构造请求`，点击右上角的第一个按钮后点击`+`号，输入仓库地址后即可选择要添加的任务。

### 分组图标

配置文件中相关配置末尾添加`img-url=`参数即可，与节点添加分组图标方法一致。

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

# 附加功能
## temperJS

temperJS可以使油猴脚本在手机端运行。

### 安装

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

### 使用

在手机端的浏览器打开greasyfork的脚本页，点击安装即可自动转换。

## BoxJS

BoxJS可帮助网络调试工具运行脚本时配置相关参数。

### 安装

对于Surge，可添加模块，地址如下。

```
// 稳定版
https://gitee.com/chavyleung/scripts/raw/master/box/rewrite/boxjs.rewrite.surge.sgmodule

// 测试版
https://gitee.com/chavyleung/scripts/raw/master/box/rewrite/boxjs.rewrite.surge.tf.sgmodule
```

对于Quantumult X，可添加重写，地址如下。

```
// 稳定版
https://gitee.com/chavyleung/scripts/raw/master/box/rewrite/boxjs.rewrite.quanx.conf

// 测试版
https://gitee.com/chavyleung/scripts/raw/master/box/rewrite/boxjs.rewrite.quanx.tf.conf
```

对于Loon，可添加插件，地址如下。

```
// 稳定版
https://gitee.com/chavyleung/scripts/raw/master/box/rewrite/boxjs.rewrite.loon.plugin

// 测试版
https://gitee.com/chavyleung/scripts/raw/master/box/rewrite/boxjs.rewrite.loon.tf.plugin
```

若需要更新BoxJS，更新调试工具的配置即可。

### 使用

用手机打开以下链接，即可进入BoxJS页面。打开后点击分享-添加到主屏幕，可供以后快速访问。

```
http://boxjs.com/
```

#### 添加订阅

点击`订阅`，添加以下订阅。

```
https://raw.githubusercontent.com/toulanboy/scripts/master/toulanboy.boxjs.json
https://raw.githubusercontent.com/Peng-YM/QuanX/master/Tasks/box.js.json
https://raw.githubusercontent.com/evilbutcher/Quantumult_X/master/evilbutcher.boxjs.json
https://raw.githubusercontent.com/zZPiglet/Task/master/zZPiglet.boxjs.json
https://jdsharedresourcescdn.azureedge.net/jdresource/lxk0301.boxjs.json
https://ooxx.be/js/box.json
https://raw.githubusercontent.com/NobyDa/Script/master/NobyDa_BoxJs.json
https://raw.githubusercontent.com/Sunert/Scripts/master/Task/sunert.boxjs.json
https://raw.githubusercontent.com/chavyleung/scripts/master/box/chavy.boxjs.json
https://raw.githubusercontent.com/chouchoui/QuanX/master/vei.boxjs.json
https://raw.githubusercontent.com/lowking/Scripts/master/lowking.boxjs.json
https://raw.githubusercontent.com/songyangzz/QuantumultX/master/syzzzf.box.json
https://raw.githubusercontent.com/toulanboy/scripts/master/toulanboy.boxjs.json
https://raw.githubusercontent.com/id77/QuantumultX/master/box.json
https://raw.githubusercontent.com/dompling/Script/master/dompling.boxjs.json
https://raw.githubusercontent.com/CenBoMin/GithubSync/main/cenbomin.box.json
https://raw.githubusercontent.com/VirgilClyne/GetSomeFries/main/box/fries.boxjs.json
https://raw.githubusercontent.com/DualSubs/DualSubs/main/box/DualSubs.box.json
https://raw.githubusercontent.com/VirgilClyne/iRingo/main/box/iRingo.boxjs.json
```

#### 脚本配置

当网络调试工具中运行的脚本要求配置BoxJs时，在BoxJs的搜索框输入脚本名称，并进行相关配置即可。

#### 会话管理

BoxJS可保存多个会话，每个会话可存储相应的Cookie信息。通过切换会话，可使同一脚本使用不同配置，从而实现多账号签到等功能。

以京东多合一签到脚本为例，在BoxJS中打开该脚本配置，点击保存会话后即可保存当前的Cookie。删除当前Cookie并重新获取，完成后再次点击保存会话。通过应用不同的会话，即可对不同的账号进行签到。

#### 配置备份

可通过备份配置，完成配置在网络调试工具之间的迁移，如从Loon迁移到Quantumult X。

点击我的-备份，生成全局变量，复制该值。需要导入配置时，点击我的-导入，复制刚才的值，点击还原即可。

#### 壁纸

点击应用-内置应用-偏好设置，在`背景图片清单`中复制以下内容即可。图片可根据需求替换。

```
无背景,
跟随系统,跟随系统
明亮,https://64.media.tumblr.com/451bca19ad0b695c08b54b4287e4f935/tumblr_nb70h5f6XN1rnbw6mo2_r1_1280.gifv
暗黑,https://i.pinimg.com/originals/94/4a/85/944a85804c97622973fbae194ddc27ac.gif
随机,https://uploadbeta.com/api/pictures/random
推女郎,https://uploadbeta.com/api/pictures/random/?key=推女郎
性感,https://uploadbeta.com/api/pictures/random/?key=性感
车模,https://uploadbeta.com/api/pictures/random/?key=车模
美腿,https://uploadbeta.com/api/pictures/random/?key=美腿
美女,https://uploadbeta.com/api/pictures/random/?key=美女
手机妹子,http://api.btstu.cn/sjbz/zsy.php
手机美女,http://api.btstu.cn/sjbz/?m_lx=suiji
```

## Sub-store

Sub-store可用于订阅转换。仓库如下。

```
https://github.com/Peng-YM/Sub-Store
```

### 安装

对于Loon，可点击配置-插件-`+`，添加以下插件。确保PROXY栏右侧区域不出现任何文字，点击保存即可。

```
https://raw.githubusercontent.com/Peng-YM/Sub-Store/master/config/Loon.plugin
```

也可在配置文件添加以下内容。

```
[MITM]
hostname=sub.store

[Script]
http-request https?:\/\/sub\.store script-path=https://raw.githubusercontent.com/Peng-YM/Sub-Store/master/backend/sub-store.js, requires-body=true, timeout=120, tag=Sub-Store
```

对于Surge，可添加以下模块订阅。

```
https://raw.githubusercontent.com/Peng-YM/Sub-Store/master/config/Surge.sgmodule
```

对于Quantumult X，可在配置文件添加以下内容。

```
[http_backend]
https://raw.githubusercontent.com/Peng-YM/Sub-Store/master/backend/sub-store.js, tag=Sub-Store, path=/, enabled=true
```

### 使用

对于Loon和Surge，打开以下网页即可。

```
https://sub-store.vercel.app
```

对于Quantumult X，需要从JSBox的Erots商店获取Sub-store脚本，然后运行JSBox版本。点击设置，查看环境是否为Quantumult X，若不是则需要切换为Quantumult X。

#### 添加订阅

点击订阅-`+`，填写订阅名称和地址，设置好后点击该条目可以看到订阅中包含的节点。

#### 复制订阅

可将Sub-store中配置好的的订阅同步到网络调试工具。以Loon为例，点击订阅，选择需要使用的订阅，并点击复制，回到Loon并添加该订阅即可。

#### 组合订阅

可以通过点击订阅-组合订阅-`+`，将多个订阅合并成一个订阅。

#### 本地解析

选择需要修改的订阅，点击编辑，即可对节点进行筛选。除常用选项外，还可以添加节点操作，可以添加多个同一类型的过滤模式，将会按照顺序执行。

##### 正则过滤器

对于正则过滤器，在保留模式下，关键词过滤只能单个匹配，多关键词则需要添加多个过滤器，而过滤模式则可匹配多个关键词。

##### 正则重命名

可以一次操作多个关键词。

##### 脚本操作

脚本操作可以为远程脚本或者本地脚本，脚本库如下。

```
https://github.com/Peng-YM/Sub-Store/tree/master/scripts
```

#### 云备份

需要获取Github Token，其中Token的权限为repo和gist。

#### 同步配置

点击同步-`+`以添加配置。配置后点击复制，即可在对应的网络调试工具中使用该链接导入配置文件。

# 相关说明

## 延迟测试

Surge、Shadowrocket、Quantumult的测试方式不同，因此结果差距较大。

Surge测试的是从目标policy返回http response header数据包的时间。Shadowrocket支持ICMP/TCP两种测速方式，默认为ICMP模式，即Ping。Quantumult采用SSH测速模式（22端口）。

## Q-Search-All-in-One复写

开启该复写后，将Safari默认搜索引擎改为DuckDuckGo，然后在浏览器地址栏输入`gm [关键词]`可以直接Google搜图，输入`yd [关键词]`可以有道翻译。所有指令如下。

|      指令     |          作用          |
|---------------|------------------------|
| gh [关键词]   | GitHub搜索             |
| gm [关键词]   | Google图片搜索         |
| sof [关键词]  | Stack Overflow搜索     |
| se [关键词]   | StackExchange搜索      |
| wiki [关键词] | 维基百科搜索           |
| wk [关键词]   | 维基中文搜索           |
| mg [关键词]   | Magi搜索               |
| tf [关键词]   | Google搜索TestFlight   |
| yd [关键词]   | 有道词典搜索           |
| trc [关键词]  | Google译至中           |
| tre [关键词]  | Google译至英           |
| trj [关键词]  | Google译至日           |
| db [关键词]   | 豆瓣搜索               |
| zh [关键词]   | 知乎搜索               |
| wb [关键词]   | 微博搜索               |
| wx [关键词]   | 微信搜索               |
| rd [关键词]   | Reddit搜索             |
| ssp [关键词]  | 少数派搜索             |
| csdn [关键词] | CSDN搜索               |
| zdm [关键词]  | 什么值得买搜索         |
| amz [关键词]  | 亚马逊搜索             |
| jd [关键词]   | 京东搜索               |
| tb [关键词]   | 淘宝搜索               |
| tm [关键词]   | 天猫搜索               |
| ac [关键词]   | Acfun搜索              |
| bli [关键词]  | 哔哩哔哩搜索           |
| ytb [关键词]  | YouTube搜索            |
| ph [关键词]   | PornHub搜索            |
| gd [关键词]   | Google Drive资源       |
| tgd [关键词]  | TG搜索Google Drive资源 |
| bi [关键词]   | 必应搜索               |
| bd [关键词]   | 百度搜索               |
| ddg [关键词]  | DuckDuckGo搜索         |
| [关键词]      | Google搜索             |

# 相关资源

## 配置文件与规则

Loon与Surge 3的配置文件一般通用。进入并挑选好所需规则集后，点开并点击Raw，复制链接到Loon并导入即可。

### Loon

```
https://github.com/nzw9314/Loon
```

### Shadowrocket

```
https://github.com/h2y/Shadowrocket-ADBlock-Rules
```

### Quantumult X

```
https://github.com/nzw9314/QuantumultX
https://github.com/Orz-3/QuantumultX
https://github.com/evilbutcher/Quantumult_X
https://github.com/KOP-XIAO/QuantumultX/
https://github.com/solikethis/QuantumultX-demo/
```

### Surge

```
https://github.com/voneo/conf/tree/master/rule
https://github.com/nzw9314/Surge
https://github.com/Choler/Surge
https://github.com/Blankwonder/surge-list
https://github.com/KOP-XIAO/Surge-Rules
https://github.com/nzw9314/surge-2/
```

### 全平台

```
https://github.com/lhie1/Rules/tree/master
https://github.com/wlaotou/Profiles
https://github.com/limbopro/Profiles/tree/master
https://github.com/yxiaocai/DivineEngine
https://github.com/NavePnow/Profiles
https://github.com/DivineEngine/Profiles/tree/master
```

## 脚本

### Quantumult X

```
https://github.com/jpcnmm/Scripting
https://github.com/elecV2/QuantumultX-Tools
```

### Surge

```
https://github.com/yichahucha/surge/tree/master
https://github.com/MeetaGit/MeetaRules
https://github.com/onewayticket255/Surge-Script
```

### 全平台

```
https://github.com/jpcnmm/Scripting
https://github.com/chavyleung/scripts
https://github.com/Peng-YM/QuanX
https://github.com/NobyDa/Script/tree/master
https://github.com/zZPiglet/Task/tree/master
```

## 任务

### Quantumult X

```
https://github.com/Orz-3/QuantumultX/tree/master/Task
```

### Surge

```
https://github.com/MeetaGit/MeetaRules/tree/m%CE%B1ster
```

## 模块

### Surge

```
https://github.com/onewayticket255/Surge-Script
```

## 图标组

### Quantumult X

```
https://github.com/elecV2/QuantumultX-ICON
https://github.com/Koolson/Qure
```

### 全平台

```
https://github.com/Orz-3/mini
https://github.com/Orz-3/task
https://github.com/Orz-3/face
https://github.com/58xinian/icon
```

# 参考教程

## Shadowrocket使用教程

```
http://laob.me/2300/
```

## Loon不完全教程

```
https://www.notion.so/Loon-f0a98c39f5224c09b281c79837380431
```

## Quantumult的使用方法

```
https://xingkaixin.me/2019/05/01/How-to-use-Quantumult/
```

## Surge使用教程

```
https://merlinblog.xyz/wiki/surge.html
```

## Quantumult X不完全教程

```
https://www.notion.so/Quantumult-X-1d32ddc6e61c4892ad2ec5ea47f00917#8c6f0c7cc5174f34903b1aa7c010fb64
https://merlinblog.xyz/wiki/quanx.html
https://limbopro.xyz/archives/11115.html
https://limbopro.xyz/archives/3846.html
```

## 关于策略组的理解

```
https://github.com/Fndroid/jsbox_script/wiki/%E5%85%B3%E4%BA%8E%E7%AD%96%E7%95%A5%E7%BB%84%E7%9A%84%E7%90%86%E8%A7%A3
```

## 你们碎碎念的解锁VIP会员的教程来了

```
https://mp.weixin.qq.com/s/ociRI8wQtRimseAYR1XONw
```

## 网球版解锁某个App永久内购的教程来了

```
https://mp.weixin.qq.com/s/gcJw46HrmJCfBcLSw4s3NA
```
