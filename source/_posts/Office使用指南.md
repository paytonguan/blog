---
title: Office使用指南
categories: Skill
abbrlink: Instructions-Of-Office
date: 2020-05-01 08:54:29
tags:
---

![](topic.jpg)

Office使用指南。

<!-- more -->

# TODOS

```
# 操作教程
http://www.wordlm.com/
```

# 下载

Windows可通过Office Tool。

```
https://otp.landian.vip/
```

Mac可通过以下链接下载。

```
https://github.com/alsyundawy/Microsoft-Office-For-MacOS
```

# 激活

## 方法

Office安装目录对于Office 2016为C:\Program Files (x86)\Microsoft Office\Office16，对于Office 2013为Office15，对于Office 2010为Office14。该目录下应当有一个文件为OSPP.VBS。

主流的激活工具均为KMS激活，会安装KMS密钥，需要VOL密钥。

### 升级为VL版

如果Office安装完成后带有Retail密钥，可先通过Office Tool删除该密钥，再安装新的密钥。也可在命令提示符输入以下命令。

```
# 以Office 2016为例
cd "C:\Program Files (x86)\Microsoft Office\Office16"

for /f %x in ('dir /b ..\root\Licenses16\proplusvl_kms*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%x"
for /f %x in ('dir /b ..\root\Licenses16\proplusvl_mak*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%x"

# 该密钥为Office专业增强版2016的VOL密钥
cscript ospp.vbs /inpkey:XQNVK-8JYDB-WJ9W3-YJ8YR-WFG99
```

VOL密钥列表如下。

```
# Office 2016-2021
https://docs.microsoft.com/zh-cn/DeployOffice/vlactivation/gvlks

# Office 2013
https://docs.microsoft.com/zh-cn/previous-versions/office/dn385360(v=office.15)

# Office 2010
https://docs.microsoft.com/zh-cn/previous-versions/office/office-2010/ee624355(v=office.14)
```

### 激活方法

通过Kmspico或Microsoft Toolkit可以激活Office 2010。也可用KMS Activator、microKMS等。

```
# KMSpico
https://www.pcsoft.com.cn/soft/41721.html

# KMS Activator
https://www.jb51.net/softs/116555.html

# Microsoft Toolkit
http://www.downza.cn/soft/187399.html

# microKMS
https://www.jb51.net/softs/451557.html

# HEU_KMS
https://ltribe.lanzoui.com/iaIcUvc61xi
```

也可使用命令行激活。以管理员身份运行命令提示符，并输入以下命令。

```
# 以Office 2016为例
cd "C:\Program Files (x86)\Microsoft Office\Office16"

# 以下服务器选一个
cscript ospp.vbs /sethst:kms.03k.org
# cscript ospp.vbs /sethst:sunpma.com

cscript ospp.vbs /act
```

注意，Office 365本身是没有所谓专业版的，转换激活后的Office 365会显示为使用的密钥对应的版本。事实上它会变成一个混合版，既有Office 365的特性，又有密钥对应版本的特性。

可通过以下命令查看激活详情，注意仍需要在Office安装目录下执行。

```
cscript ospp.vbs /dstatus
```

<details>
<summary>【进阶】卸载KMS激活</summary>

以管理员身份打开命令提示符，输入以下命令，注意仍需要在Office安装目录下执行。

```
# 查询激活密钥后五位
cscript ospp.vbs /dstatus

cscript ospp.vbs /unpkey:[密钥后五位]
cscript ospp.vbs /remhst
cscript ospp.vbs /rearm
```
</details>

## 密钥

### Office 97

```
4657-1931151
1921701-05354
8089-3069692
9990-1456507
9978-1234567
4156-0287065
415-0151383
2978-0029987
425-1921701
103-0145657
102-0128344
14080-000-4203561
28779-051-0101444
27716-102-0128344
111-1111111
167649998411
4657-1931151
1921701-05354
123-1111111
123-444444
```

### Office 2003

```
GWH28-DGCMP-P6RC4-6J4MT-3HFDY
XVG79-Q2WK3-JRPMD-9H26V-7TBYT
BY3TG-48BRP-VTT2Y-YH84X-TYJ97
J2MV9-JYYQ6-JM44K-QMYTH-8RB2W
```

# 插件

## Word

### 小恐龙公文排版助手

解决各种公文排版问题。

```
https://gw.xkonglong.com/
```

### Word必备工具箱

为Word增加大体4类功能。

```
http://www.ahzll.top/HELP/PAGE/blog_5488e3a90100u8ux.html
```

### 慧办公

```
http://www.hbg666.com/
```

## Excel

### Excel必备工具箱

```
http://www.ahzll.top/
```

### 方方格子

```
http://www.ffcell.com/home/products.aspx
```

### Excel催化剂

```
https://www.yuque.com/excelcuihuajihome/helpdocument
```

### Excel易用宝

```
https://yyb.excelhome.net/
```

### Excel精灵

```
http://www.excelbbx.net/Eling.htm
```

### 慧办公&巧办公

```
http://www.hbg666.com/
http://www.hbg666.com/q.php
```

### AudTool

审计常用，免费试用期30天。

```
http://www.ffcell.com/home/AuditOrder.aspx
```

### EasyShu

图表插件。

```
https://www.yuque.com/excelcuihuajihome/helpdocument/ckw03e
```

## PowerPoint

### OneKeyTools

形状、调色、三维、图片处理、演示辅助、表格处理、音频处理等。

```
http://oktools.xyz/
```

### Lvyh Tools

PPT转Word、字体收藏、字体导出、顶点编辑、线条编辑、形状编辑、位置分布等。

```
https://addins.cn/yhtools/
```

### PA口袋动画

动画制作。

```
http://www.papocket.com/
```

### iSlide

```
https://www.islide.cc/download
```

### PPT精灵

支持PowerPoint 2007-2016。

```
http://excelbbx.net/PPT/Index.htm
```

# 使用技巧

## 分屏浏览

选择视图-拆分窗口即可。

## 特殊字符

|   特殊字符  |      含义      |
|-------------|----------------|
| ^?          | 任意单个字符   |
| ^#          | 任意单个数字   |
| ^$          | 任意英文字母   |
| ^p          | 段落标记       |
| ^l          | 手动换行符     |
| ^g或^1      | 图形           |
| ^+          | 1/4长划线      |
| ^j          | 长划线         |
| ^q          | 短划线         |
| ^t          | 制表符         |
| ^           | 脱字号         |
| ^v          | 分栏符         |
| ^b          | 分节符         |
| ^n          | 省略号         |
| ^i          | 全角省略号     |
| ^z          | 无宽非分隔符   |
| ^x          | 无宽可选分隔符 |
| ^s          | 不间断空格     |
| ^~          | 不间断连字符   |
| ^%          | 段落符号       |
| ^           | 分节符（§）    |
| ^f或^2      | 脚注标记       |
| ^-          | 可选连字符     |
| ^w          | 空白区域       |
| ^m          | 手动分页符     |
| ^e          | 尾注标记       |
| ^d          | 域             |
| ^Unnnn      | Unicode字符    |
| ^u8195      | 全角空格       |
| ^32或^u8194 | 半角空格       |
| ^a或^5      | 批注           |

## 通配符

要查找已被定义为通配符的字符，需该字符前键入反斜杠`\`。

如果使用了通配符，在查找文字时会大小写敏感。如果希望查找大写和小写字母的任意组合，需要使用方括号通配符。如输入`[Hh]*[Tt]`可找到heat、Hat或HAT，而用`H*t`找不到heat。

使用通配符时，Word只查找整个单词。如搜索e*r可找到enter，但不会找到entertain。如输入`<(e*r)`可找到enter和entertain。

在查找图形时，Word只查找嵌入图形，而不能查找浮动图形。在默认情况下，Word会将导入的图形以嵌入图形的方式插入到文档中。

如果包含可选连字符代码，Word只会找到在指定位置带有可选连字符的文字。如果省略可选连字符代码，Word将找到所有匹配的文字，包括带有可选连字符的文字。

### 列表

|      通配符      |           含义          |
|------------------|-------------------------|
| ?                | 任意单个字符            |
| [ - ]            | 指定范围内任意单个字符  |
| [0-9]            | 任意单个数字            |
| [.0-9]           | 任意单个带小数点数字    |
| [!0-9]           | 所有非数字字符          |
| [a-zA-Z]         | 任意英文字母            |
| [a-z]            | 任意全小写英文字母      |
| [A-Z]            | 任意全大写英文字母      |
| [^1-^127]        | 所有西文字符            |
| [!^1-^127]       | 所有中文汉字和中文标点  |
| [一-龥]或[一-﨩] | 所有中文汉字            |
| [!一-龥^1-^127]  | 所有中文标点            |
| [!x-z]           | 指定范围外任意单个字符  |
| *                | 任意字符串              |
| @                | 1个以上前一字符或表达式 |
| { n }            | n个前一字符或表达式     |
| { n, }           | n个以上前一字符或表达式 |
| { n,m }          | n到m个前一字符或表达式  |
| ^t               | 制表符                  |
| ^s               | 不间断空格              |
| ^13              | 段落标记                |
| ^l或^11          | 手动换行符              |
| ( )              | 表达式                  |
| <                | 单词开头                |
| < >              | 单词开头/结尾           |
| ^g               | 图形                    |
| ^q               | 1/4长划线               |
| ^+               | 长划线                  |
| ^=               | 短划线                  |
| ^^               | 脱字号                  |
| ^n或^14          | 分栏符                  |
| ^m               | 分节符/分页符           |
| ^i               | 省略号                  |
| ^j               | 全角省略号              |
| ^z               | 无宽非分隔符            |
| ^x               | 无宽可选分隔符          |
| ^~               | 不间断连字符            |
| ^=               | 短划线                  |
| ^%               | 分节符（§）             |

### 常用示例

#### 选取包含特定字符的一行

将手动换行符变为换行符，即^l全部替换为^p。查找与替换中勾选使用通配符，查找内容填写`[内容](*)^13`，搜索范围为全文档即可。

#### 选取同时包含两个关键词的内容

```
(?=.*[关键词1])(?=.*[关键词2])^.*$
```

# 参考教程

## Word查找替换详细用法及通配符一览表

```
https://www.cnblogs.com/whchensir/p/5768030.html
```

## 正则同时包含两个关键字

```
https://www.cnblogs.com/fsqsec/p/7132815.html
```
