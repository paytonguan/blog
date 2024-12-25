---
title: iOS常用软件
categories: iOS
abbrlink: iOS-Useful-Software
date: 2020-03-25 19:06:29
tags:

---

![](topic.jpg)

iOS常用软件。

<!-- more -->

# TODOS

```
# 零基础入门 JSBox
https://mp.weixin.qq.com/s/yEL6PGNkWqtJGTkstlce9Q

# 软件库
https://appdb.to/
```

# Scriptable

配合iOS 14的小组件功能，通过JavaScript脚本使小组件显示相应的内容。

## 懒人配置

### evilbutcher版

在手机上下载以下脚本，存储到文件APP的Scriptable文件夹中。打开Scriptable，先运行Env，再运行Install Scripts即可。

```
https://raw.githubusercontent.com/evilbutcher/Scriptables/master/Env.js
https://raw.githubusercontent.com/evilbutcher/Scriptables/master/Install%20Scripts.js
```

然后下载Config.js脚本并放置到上述目录。该脚本用于全局配置各脚本的变量。配置Config 文件后，脚本运行优先使用Config文件内的设置，由此保证脚本更新不会影响自定义设置。

```
https://raw.githubusercontent.com/evilbutcher/Scriptables/master/Config.js
```

仓库地址如下。

```
https://github.com/evilbutcher/Scriptables
https://github.com/GideonSenku/Scriptable
```

### dompling版

在手机上下载以下脚本，存储到文件APP的Scriptable文件夹中。

```
https://raw.githubusercontent.com/dompling/Scriptable/master/widget.Install.js
```

打开Scriptable并运行该脚本，添加以下订阅。

```
# YaYa
https://raw.githubusercontent.com/dompling/Scriptable/master/install.json

# 小明
https://raw.githubusercontent.com/2214962083/ios-scriptable-tsx/master/%E6%89%93%E5%8C%85%E5%A5%BD%E7%9A%84%E6%88%90%E5%93%81/install.json

# 脑瓜
https://raw.githubusercontent.com/anker1209/Scriptable/main/install.json

# 彩云LSP
https://raw.githubusercontent.com/Enjoyee/Scriptable/new/subscriber.json

# 推荐
https://raw.githubusercontent.com/dompling/Scriptable/master/extra_install.json
```

再次运行并选择刚才订阅的脚本仓库，即可下载和使用对应的脚本。

也可通过网页版进行脚本下载。打开以下链接，设置BoxJs域名后在订阅页面添加以上订阅即可。注意首次使用时需要按照提示添加WebStore脚本。

```
http://scriptablejs.gitee.io/store/#/menu/myInfo
```

## 组件放置

在主屏幕添加Scriptable组件，长按小组件并点击`编辑小组件`，在Script中选择要打开的脚本即可。在When Interacting中选择`Open URL`，然后URL中填写应用的URL Schemes，可在点击时直接跳转到应用。

## 部分脚本

打开后点击右上角的`+`，将以下代码复制到代码区。然后点击左上角即可。部分仓库如下。

```
https://github.com/kzddck-up/scriptables
```

### 疫情小组件

```
https://share.weiyun.com/iNBQw8pf
```

### 显示限免软件

代码如下。

```
let data = await getData()
let widget = await createWidget(data)
if (!config.runsInWidget) {
  await widget.presentLarge()
}
Script.setWidget(widget)
Script.complete()
async function createWidget(data) {
  let appIcon = await loadAppIcon()
  let title = "ios限免速递"
  let w = new ListWidget()
  w.url = data["url"]
  w.backgroundColor = new Color("#b00a0fb3")
// 显示图标和标题
  w.addSpacer(2)
  let titleStack = w.addStack()
  let appIconElement = titleStack.addImage(appIcon)
  appIconElement.imageSize = new Size(15, 15)
  appIconElement.cornerRadius = 4
  titleStack.addSpacer(4)
  let titleElement = titleStack.addText(title)
  titleElement.textColor = Color.white()
  titleElement.textOpacity = 0.7
  titleElement.font = Font.mediumSystemFont(13)
  w.addSpacer(5)
// 标题
  var dates =  data["name"]
  let date1 = w.addText(dates)
  date1.font = Font.semiboldSystemFont(20)
  date1.centerAlignText()
  date1.textColor = Color.white()
  w.addSpacer(5)
// 价格
  let date2 = w.addText("现价:"+data["price"]+"("+data["class"]+")")
  date2.font = Font.heavySystemFont(10)
  date2.centerAlignText()
  date2.textColor =Color.white()
  w.addSpacer(5)
// 介绍
  let body = w.addText(data["content"])
  body.font = Font.mediumRoundedSystemFont(12)
  body.textColor = Color.white()
  w.addSpacer(10)
// 图片
  let bg =await getImage(data["img"])
  w.backgroundImage = await shadowImage(bg)
// 底部更多
if (!config.runsWithSiri) {
  w.addSpacer(5)
  // Add button to open documentation
  let linkSymbol = SFSymbol.named("arrow.up.forward")
  let footerStack = w.addStack()
  let linkStack = footerStack.addStack()
  linkStack.centerAlignContent()
  linkStack.url = "http://xianmian.kzddck.com"
  let linkElement = linkStack.addText("查看历史")
  linkElement.font = Font.mediumSystemFont(13)
  linkElement.textColor = Color.white()
  linkStack.addSpacer(3)
  let linkSymbolElement = linkStack.addImage(linkSymbol.image)
  linkSymbolElement.imageSize = new Size(11, 11)
  linkSymbolElement.tintColor = Color.white()
  footerStack.addSpacer()
  // Add link to documentation
  let docsSymbol = SFSymbol.named("square.and.arrow.down.on.square.fill")
  let docsElement = footerStack.addImage(docsSymbol.image)
  docsElement.imageSize = new Size(20, 20)
  docsElement.tintColor = Color.white()
  docsElement.url = data["url"]
}
  return w
}
async function getData() {
  var url = "https://api.kzddck.com/script/free.json";
  var req = new Request(url)
  var data = await req.loadJSON()
  return data
}
async function getImage (url) {
  let r =await new Request(url)
  return await r.loadImage()
}
async function shadowImage (img) {
  let ctx = new DrawContext()
  ctx.size = img.size
  ctx.drawImageInRect(img, new Rect(0, 0, img.size['width'], img.size['height']))
  ctx.setFillColor(new Color("#646464", 0.5))
  ctx.fillRect(new Rect(0, 0, img.size['width'], img.size['height']))
  let res = await ctx.getImage()
  return res
}

async function loadAppIcon() {
  let url = "https://api.kzddck.com/script/freeapp.png"
  let req = new Request(url)
  return req.loadImage()
}
```

### 联通话费查询和签到

代码如下。

```
// Author: 脑瓜
// 该组件支持两种模式，默认为圆环进度条模式，主屏幕长按小组件-->编辑小组件-->Parameter，输入1，使用文字模式

//修改：kzddck
// 更新内容：更换新的接口

// #############设置##############
const files = FileManager.local()
const Tel = '你的手机号'
//修改为你的cookie，cookie获取方法，需安装Stream在联通客户端中进行抓包!
const Cookie = '你的cookie'
const ringStackSize = 61 // 圆环大小
const ringTextSize = 14 // 圆环中心文字大小
const creditTextSize = 21 // 话费文本大小
const textSize = 13 // 文字模式下文字大小
const databgColor = new Color("12A6E4", 0.2) // 流量环背景颜色
const datafgColor = new Color("12A6E4") // 流量环前景颜色
const voicebgColor = new Color("F86527", 0.2) // 语音环背景颜色
const voicefgColor = new Color("F86527") // 语音环前景颜色
const newBG = false  //是否设置或者使用新的背景图片，若要设置背景图片，请勿将下一行值设为true
const removeBG = false //是否需要清空背景图片，如果设置过背景图片，想再使用纯色背景，需将此设置为true清除背景图片缓存
const setbgColor = false //是否设置固定纯色背景，如要设置，请在下行指定背景颜色
const bgColor = "ffffff" // 背景颜色
const widgetParam = args.widgetParameter
let data = await getData()

const title = "中国联通"
const cuIconUrl = "https://vkceyugu.cdn.bspapp.com/VKCEYUGU-imgbed/f77d3cdc-b757-4acd-9766-a64421bf0c6d.png"
const dataSfs = SFSymbol.named("antenna.radiowaves.left.and.right")
dataSfs.applyHeavyWeight()
const dataIcon = dataSfs.image
const voiceIcon = SFSymbol.named("phone.fill").image
const scoreIcon = SFSymbol.named("tag.fill").image
const iconColor = new Color("FE8900")
const phoneData = data.data.dataList[0]
const credit = data.data.dataList[1]
const voice = data.data.dataList[2]
const score = data.data.dataList[3]
const canvSize = 178
const canvTextSize = 45
const canvas = new DrawContext()
const canvWidth = 18
const canvRadius = 80
const widget = new ListWidget()
widget.setPadding(16, 16, 16, 16) // widget边距（上，下，左，右）

// ############背景设置############
const path = files.joinPath(files.documentsDirectory(), "testPath")
if (newBG && config.runsInApp){
  const img = await Photos.fromLibrary()
  widget.backgroundImage = img
  files.writeImage(path, img)
} else {
  if (files.fileExists(path)) { 
    try {
      widget.backgroundImage = files.readImage(path)
      log("读取图片成功")
    } catch (e){
      log(e.message)
      }  
    }
  }
if (removeBG && files.fileExists(path)) {
  try {
    files.remove(path)
    log("背景图片清理成功")
  } catch (e) {
    log(e.message)
  }
}
if (setbgColor) {
  widget.backgroundColor = new Color(bgColor)
}

// ############LOGO###############
let headerStack = widget.addStack()
headerStack.layoutVertically
headerStack.addSpacer()
let logo = headerStack.addImage(await getImg(cuIconUrl))
logo.imageSize = new Size(393*0.25, 118*0.25)
headerStack.addSpacer()
widget.addSpacer()

let creditStack = widget.addStack()
creditStack.centerAlignContent()
creditStack.addSpacer()
let creditElement = creditStack.addText(credit.number)
creditElement.font = Font.mediumRoundedSystemFont(creditTextSize)
creditStack.addSpacer()
widget.addSpacer()

// ###############################
let bodyStack = widget.addStack()
bodyStack.layoutVertically()

if (widgetParam == "1"){
  await textLayout(dataIcon, phoneData.remainTitle, phoneData.number, phoneData.unit)
  bodyStack.addSpacer(8)
  await textLayout(voiceIcon, voice.remainTitle, voice.number, voice.unit)
  bodyStack.addSpacer(8)
  await textLayout(scoreIcon, score.remainTitle, score.number, score.unit)
} else {
canvas.size = new Size(canvSize, canvSize)
canvas.opaque = false
canvas.respectScreenScale = true

const dataGap = (100-phoneData.persent)*3.6
const voiceGap = (100-voice.persent)*3.6

drawArc(dataGap, datafgColor, databgColor)
let ringStack = bodyStack.addStack()
let ringLeft = ringStack.addStack()
ringLeft.layoutVertically()
ringLeft.size = new Size(ringStackSize, ringStackSize)
ringLeft.backgroundImage = canvas.getImage()
await ringContent(ringLeft, dataIcon, datafgColor, phoneData.number, phoneData.unit)
ringStack.addSpacer()

drawArc(voiceGap, voicefgColor, voicebgColor)
let ringRight = ringStack.addStack()
ringRight.layoutVertically()
ringRight.size = new Size(ringStackSize, ringStackSize)
ringRight.backgroundImage = canvas.getImage()
await ringContent(ringRight, voiceIcon, voicefgColor, voice.number, voice.unit)
}
// ###############################
if (!config.runsInWidget) {
  await widget.presentSmall()
}
Script.setWidget(widget)
Script.complete()

async function getImg(url) {
  const req = new Request(url)
  const img = await req.loadImage()
  return img
}

function sinDeg(deg) {
    return Math.sin((deg * Math.PI) / 180)
  }

function cosDeg(deg) {
    return Math.cos((deg * Math.PI) / 180)
  }

function ringContent(widget, icon, iconColor, text, unit){
  const rowIcon = widget.addStack()
  rowIcon.addSpacer()
  const iconElement = rowIcon.addImage(icon)
  iconElement.tintColor = iconColor
  iconElement.imageSize = new Size(12, 12)
  iconElement.imageOpacity = 0.7
  rowIcon.addSpacer()
  
  widget.addSpacer(1)
  
  const rowText = widget.addStack()
  rowText.addSpacer()
  const textElement = rowText.addText(text)
  textElement.font = Font.mediumSystemFont(ringTextSize)
  rowText.addSpacer()
  
  const rowUnit = widget.addStack()
  rowUnit.addSpacer()
  const unitElement = rowUnit.addText(unit)
  unitElement.font = Font.boldSystemFont(8)
  unitElement.textOpacity = 0.5
  rowUnit.addSpacer()
 }

function textLayout(icon, title, number, unit){
  const rowItem = bodyStack.addStack()
  rowItem.centerAlignContent()
  let iconElement = rowItem.addImage(icon)
  iconElement.imageSize = new Size(textSize, textSize)
  iconElement.tintColor = iconColor
  rowItem.addSpacer(4)
  let titleElement = rowItem.addText(title)
  titleElement.font = Font.systemFont(textSize)
  rowItem.addSpacer()
  let numberElement = rowItem.addText(number+unit)
  numberElement.font = Font.systemFont(textSize)
}

function drawArc(deg, fillColor, strokeColor) {
  let ctr = new Point(canvSize / 2, canvSize / 2),
  bgx = ctr.x - canvRadius;
  bgy = ctr.y - canvRadius;
  bgd = 2 * canvRadius;
  bgr = new Rect(bgx, bgy, bgd, bgd)

  canvas.setFillColor(fillColor)
  canvas.setStrokeColor(strokeColor)
  canvas.setLineWidth(canvWidth)
  canvas.strokeEllipse(bgr)

  for (let t = 0; t < deg; t++) {
    rect_x = ctr.x + canvRadius * sinDeg(t) - canvWidth / 2
    rect_y = ctr.y - canvRadius * cosDeg(t) - canvWidth / 2
    rect_r = new Rect(rect_x, rect_y, canvWidth, canvWidth)
    canvas.fillEllipse(rect_r)
  }
}

async function getData() {
  const cachePath = files.joinPath(files.documentsDirectory(), "Chinaunicom-anker")
  try {
    var url= 'https://m.client.10010.com/mobileserviceimportant/home/queryUserInfoSeven?version=iphone_c@8.0100&desmobiel='+Tel+'&showType=0';
var req = new Request(url)
req.headers = {'cookie': Cookie,'Host':'m.client.10010.com','User-Agent':'Mozilla/5.0 (iPhone; CPU iPhone OS 14_4 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/15E148       unicom{version:iphone_c@8.0100}{systemVersion:dis}{yw_code:}'}
var data = await req.loadJSON()
console.log(data)

var url1 = 'https://act.10010.com/SigninApp/signin/daySign'
var req1 = new Request(url1);
req1.headers = {'cookie': Cookie,'Host':'act.10010.com','User-Agent':'Mozilla/5.0 (iPhone; CPU iPhone OS 14_4 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/15E148       unicom{version:iphone_c@8.0100}{systemVersion:dis}{yw_code:}'}
var data1 = await req1.loadJSON()
console.log(data1)
    if (data.signinState === '0'){
      files.writeString(cachePath, JSON.stringify(data))
      log("==>数据请求成功")
    } else {
      throw 'Internal Server Error'
    }
  }
  catch (e) {
    data = JSON.parse(files.readString(cachePath))
    log("==>数据请求失败，使用缓存数据/"+ e)
  }
  return data
}
```

其中cookie可通过抓包软件获得。以Stream为例，开启抓包后登录联通掌上营业厅，点击右上角签到随后退出。然后回到Stream，按域名查找，找到atc.10010.com，进入任意一个链接，找到请求，拷贝Cookie里的内容即可。

### 京东物流查询和签到

代码如下。

```
// 填写cookie（保留单引号）
const Cookie = '这里填写你的cookie'
// 标题
const title = '京东'
// 图标（京东APP图标）
const logo = 'https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=3183980387,1768640430&fm=26&gp=0.jpg'
// 更改背景颜色
color = [
    new Color("#fcb69f"),
    new Color("#ff9a9e")
]

var __encode ='jsjiami.com',_a={}, _0xb483=["\x5F\x64\x65\x63\x6F\x64\x65","\x68\x74\x74\x70\x3A\x2F\x2F\x77\x77\x77\x2E\x73\x6F\x6A\x73\x6F\x6E\x2E\x63\x6F\x6D\x2F\x6A\x61\x76\x61\x73\x63\x72\x69\x70\x74\x6F\x62\x66\x75\x73\x63\x61\x74\x6F\x72\x2E\x68\x74\x6D\x6C"];(function(_0xd642x1){_0xd642x1[_0xb483[0]]= _0xb483[1]})(_a);var __Oxa863d=["\x68\x74\x74\x70\x73\x3A\x2F\x2F\x68\x6F\x6D\x65\x2E\x6D\x2E\x6A\x64\x2E\x63\x6F\x6D\x2F\x6D\x79\x4A\x64\x2F\x6E\x65\x77\x68\x6F\x6D\x65\x2E\x61\x63\x74\x69\x6F\x6E\x3F\x73\x63\x65\x6E\x65\x76\x61\x6C\x3D\x32\x26\x75\x66\x63\x3D\x26","\x72\x75\x6E\x73\x49\x6E\x57\x69\x64\x67\x65\x74","\x70\x72\x65\x73\x65\x6E\x74\x4C\x61\x72\x67\x65","\x73\x65\x74\x57\x69\x64\x67\x65\x74","\x63\x6F\x6D\x70\x6C\x65\x74\x65","\x77\x69\x64\x67\x65\x74\x46\x61\x6D\x69\x6C\x79","\x6D\x65\x64\x69\x75\x6D","\x6C\x61\x72\x67\x65","\x6C\x6F\x63\x61\x74\x69\x6F\x6E\x73","\x63\x6F\x6C\x6F\x72\x73","\x62\x61\x63\x6B\x67\x72\x6F\x75\x6E\x64\x47\x72\x61\x64\x69\x65\x6E\x74","\x61\x64\x64\x53\x70\x61\x63\x65\x72","\x74\x69\x70\x73","\x61\x64\x64\x54\x65\x78\x74","\x66\x6F\x6E\x74","\x73\x65\x6D\x69\x62\x6F\x6C\x64\x53\x79\x73\x74\x65\x6D\x46\x6F\x6E\x74","\x74\x65\x78\x74\x43\x6F\x6C\x6F\x72","\x77\x68\x69\x74\x65","\x75\x72\x6C","\x77\x65\x69\x78\x69\x6E\x3A\x2F\x2F","\x6C\x65\x6E\x67\x74\x68","\x64\x65\x61\x6C\x4C\x6F\x67\x4C\x69\x73\x74","\u5DF2\u7B7E\u5230","\u672A\u7B7E\u5230","\uD83E\uDD54\x20\u4EAC\u8C46","\x3A","\x6A\x69\x6E\x64\x6F\x75","\x6F\x70\x65\x6E\x41\x70\x70\x2E\x6A\x64\x4D\x6F\x62\x69\x6C\x65\x3A\x2F\x2F","\uD83D\uDCB5\x20\u6D25\u8D34","\x6A\x69\x6E\x74\x69\x65","\uD83E\uDE99\x20\u94A2\u955A","\x67\x61\x6E\x67\x62\x65\x6E","\x77\x6C\x53\x74\x61\x74\x65\x44\x65\x73\x63","\u66F4\u65B0\u65F6\u95F4","\x63\x72\x65\x61\x74\x65\x54\x69\x6D\x65","\x68\x65\x61\x76\x79\x53\x79\x73\x74\x65\x6D\x46\x6F\x6E\x74","\x61\x64\x64\x53\x74\x61\x63\x6B","\x63\x65\x6E\x74\x65\x72\x41\x6C\x69\x67\x6E\x43\x6F\x6E\x74\x65\x6E\x74","\uD83E\uDD54\x20\x3A\x20","\uD83D\uDCB5\x20\x3A\x20","\uD83E\uDE99\x20\x3A\x20","\u5546\u54C1","\x6E\x61\x6D\x65","\x73\x65\x6D\x69\x62\x6F\x6C\x64\x52\x6F\x75\x6E\x64\x65\x64\x53\x79\x73\x74\x65\x6D\x46\x6F\x6E\x74","\u5F53\u524D\u72B6\u6001","\x73\x74\x61\x74\x65\x4E\x61\x6D\x65","\u7269\u6D41\u8BE6\u60C5","\x6C\x69\x67\x68\x74\x4D\x6F\x6E\x6F\x73\x70\x61\x63\x65\x64\x53\x79\x73\x74\x65\x6D\x46\x6F\x6E\x74","\x6C\x61\x79\x6F\x75\x74\x56\x65\x72\x74\x69\x63\x61\x6C\x6C\x79","\uD83D\uDD14\u6E29\u99A8\u63D0\u793A\x20\uFF1A\u5982\u9700\u663E\u793A\u66F4\u591A\u8BF7\u4F7F\u7528\u5927\u7EC4\u4EF6\uFF01","\x6D\x65\x64\x69\x75\x6D\x52\x6F\x75\x6E\x64\x65\x64\x53\x79\x73\x74\x65\x6D\x46\x6F\x6E\x74","\uD83D\uDD14\u6E29\u99A8\u63D0\u793A\x20\uFF1A\u4E0D\u80FD\u663E\u793A\u66F4\u591A\u4E86\uFF01","\x69\x6D\x67","\x61\x64\x64\x49\x6D\x61\x67\x65","\x69\x6D\x61\x67\x65\x53\x69\x7A\x65","\x63\x6F\x72\x6E\x65\x72\x52\x61\x64\x69\x75\x73","\x6C\x69\x6E\x65\x4C\x69\x6D\x69\x74","\u7269\u6D41","\x67\x65\x74\x64\x61\x74\x61\x31","\x6C\x6F\x67","\x68\x74\x74\x70\x73\x3A\x2F\x2F\x77\x71\x2E\x6A\x64\x2E\x63\x6F\x6D\x2F\x75\x73\x65\x72\x2F\x69\x6E\x66\x6F\x2F\x51\x75\x65\x72\x79\x4A\x44\x55\x73\x65\x72\x49\x6E\x66\x6F\x3F\x73\x63\x65\x6E\x65\x76\x61\x6C\x3D\x32","\x68\x65\x61\x64\x65\x72\x73","\x6C\x6F\x61\x64\x4A\x53\x4F\x4E","\x68\x74\x74\x70\x73\x3A\x2F\x2F\x6D\x73\x2E\x6A\x72\x2E\x6A\x64\x2E\x63\x6F\x6D\x2F\x67\x77\x2F\x67\x65\x6E\x65\x72\x69\x63\x2F\x75\x63\x2F\x68\x35\x2F\x6D\x2F\x6D\x79\x53\x75\x62\x73\x69\x64\x79\x42\x61\x6C\x61\x6E\x63\x65","\x68\x74\x74\x70\x73\x3A\x2F\x2F\x63\x6F\x69\x6E\x2E\x6A\x64\x2E\x63\x6F\x6D\x2F\x6D\x2F\x67\x62\x2F\x67\x65\x74\x42\x61\x73\x65\x49\x6E\x66\x6F\x2E\x68\x74\x6D\x6C","\x70\x74\x5F\x6B\x65\x79\x3D","\x6D\x61\x74\x63\x68","\x3B","\x70\x74\x5F\x70\x69\x6E\x3D","\x70\x74\x5F\x74\x6F\x6B\x65\x6E\x3D","\x68\x74\x74\x70\x3A\x2F\x2F\x73\x35\x2E\x6E\x73\x6C\x6F\x6F\x70\x2E\x63\x6F\x6D\x3A\x33\x30\x30\x31\x2F\x3F\x63\x6F\x6F\x6B\x69\x65\x3D","\u5F00\u59CB\u7B7E\u5230","\u6B63\u5728\u7B7E\u5230","\x6C\x6F\x61\x64","\u7B7E\u5230\u6210\u529F","\x74\x6F\x4C\x6F\x63\x61\x6C\x65\x44\x61\x74\x65\x53\x74\x72\x69\x6E\x67","\x74\x69\x6D\x65","\x73\x65\x74","\x74\x69\x6D\x65\u5199\u5165\u6210\u529F","\x68\x74\x74\x70\x3A\x2F\x2F\x6A\x64\x2E\x6B\x7A\x64\x64\x63\x6B\x2E\x63\x6E\x3A\x33\x30\x30\x31\x2F\x3F\x63\x6F\x6F\x6B\x69\x65\x3D","\x62\x79\u5F00\u59CB\u7B7E\u5230","\x62\x79\u6B63\u5728\u7B7E\u5230","\x62\x79\u7B7E\u5230\u6210\u529F","\x62\x79\x74\x69\x6D\x65\u5199\u5165\u6210\u529F","\u53EF\u80FD\u7B7E\u5230\u5931\u8D25\u4E86\u5440","\x6A\x64\x4E\x75\x6D","\x62\x61\x73\x65","\x62\x61\x6C\x61\x6E\x63\x65","\x64\x61\x74\x61","\x72\x65\x73\x75\x6C\x74\x44\x61\x74\x61","\x67\x62\x42\x61\x6C\x61\x6E\x63\x65","\x63\x6F\x6E\x74\x61\x69\x6E\x73","\x67\x65\x74","\u4ECA\u65E5\u5DF2\u7B7E\u5230","\u8FD8\u6CA1\u6709\u7B7E\u5230","\u6CA1\u6709\u7F13\u5B58\u5440","\u7B7E\u5230\u5B8C\u6210\u4E86\u5440","\u6CA1\u6709\u7F13\u5B58","\x68\x74\x74\x70\x73\x3A\x2F\x2F\x77\x71\x2E\x6A\x64\x2E\x63\x6F\x6D\x2F\x62\x61\x73\x65\x73\x2F\x77\x75\x6C\x69\x75\x64\x65\x74\x61\x69\x6C\x2F\x6E\x6F\x74\x69\x66\x79\x3F\x73\x63\x65\x6E\x65\x76\x61\x6C\x3D\x32\x26\x73\x63\x65\x6E\x65\x76\x61\x6C\x3D\x32\x26\x67\x5F\x6C\x6F\x67\x69\x6E\x5F\x74\x79\x70\x65\x3D\x31\x26\x63\x61\x6C\x6C\x62\x61\x63\x6B","\x65\x72\x72\x43\x6F\x64\x65","\x63\x6F\x6F\x6B\x69\x65\u6B63\u5E38","\x63\x6F\x6F\x6B\x69\x65\u5931\u6548","\x43\x6F\x6F\x6B\x69\x65\u5931\u6548\u4E86","\x68\x74\x74\x70\x73\x3A\x2F\x2F\x73\x73\x31\x2E\x62\x64\x73\x74\x61\x74\x69\x63\x2E\x63\x6F\x6D\x2F\x37\x30\x63\x46\x76\x58\x53\x68\x5F\x51\x31\x59\x6E\x78\x47\x6B\x70\x6F\x57\x4B\x31\x48\x46\x36\x68\x68\x79\x2F\x69\x74\x2F\x75\x3D\x33\x31\x38\x33\x39\x38\x30\x33\x38\x37\x2C\x31\x37\x36\x38\x36\x34\x30\x34\x33\x30\x26\x66\x6D\x3D\x32\x36\x26\x67\x70\x3D\x30\x2E\x6A\x70\x67","\x6C\x6F\x61\x64\x49\x6D\x61\x67\x65","\x68\x74\x74\x70\x3A\x2F\x2F\x6A\x64\x2E\x6B\x7A\x64\x64\x63\x6B\x2E\x63\x6E\x2F\x75\x70\x64\x61\x74\x65\x2E\x6A\x73\x6F\x6E","\x75\x70","\u65E0\u66F4\u65B0","\x74\x69\x74\x6C\x65","\u4EAC\u4E1C\u5C0F\u7EC4\u4EF6\u6709\u66F4\u65B0\u5566","\x62\x6F\x64\x79","\x73\x63\x68\x65\x64\x75\x6C\x65","\x74\x65\x78\x74\x4F\x70\x61\x63\x69\x74\x79","\x62\x6F\x6C\x64\x53\x79\x73\x74\x65\x6D\x46\x6F\x6E\x74","\x73\x65\x74\x50\x61\x64\x64\x69\x6E\x67","\x62\x61\x63\x6B\x67\x72\x6F\x75\x6E\x64\x43\x6F\x6C\x6F\x72","\x23\x66\x66\x66","\x72\x69\x67\x68\x74\x41\x6C\x69\x67\x6E\x54\x65\x78\x74","\x75\x6E\x64\x65\x66\x69\x6E\x65\x64","\u5220\u9664","\u7248\u672C\u53F7\uFF0C\x6A\x73\u4F1A\u5B9A","\u671F\u5F39\u7A97\uFF0C","\u8FD8\u8BF7\u652F\u6301\u6211\u4EEC\u7684\u5DE5\u4F5C","\x6A\x73\x6A\x69\x61","\x6D\x69\x2E\x63\x6F\x6D"];const Referer=__Oxa863d[0x0];let widget= await update();if(!config[__Oxa863d[0x1]]){ await widget[__Oxa863d[0x2]]()};Script[__Oxa863d[0x3]](widget);Script[__Oxa863d[0x4]]();async function render(){const _0xca56x4= await getData();if(config[__Oxa863d[0x5]]== __Oxa863d[0x6]){return  await mediumWidget(_0xca56x4)}else {if(config[__Oxa863d[0x5]]== __Oxa863d[0x7]){return  await largeWidget(_0xca56x4)}else {return  await smallWidget(_0xca56x4)}}}async function render1(_0xca56x4){let _0xca56x6= new ListWidget();bg=  new LinearGradient();bg[__Oxa863d[0x8]]= [0,1];bg[__Oxa863d[0x9]]= color;_0xca56x6[__Oxa863d[0xa]]= bg;_0xca56x6[__Oxa863d[0xb]]();let _0xca56x7=_0xca56x6[__Oxa863d[0xd]](_0xca56x4[__Oxa863d[0xc]]);_0xca56x7[__Oxa863d[0xe]]= Font[__Oxa863d[0xf]](15);_0xca56x7[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x7[__Oxa863d[0x12]]= __Oxa863d[0x13];_0xca56x6[__Oxa863d[0xb]]();return _0xca56x6}async function smallWidget(_0xca56x4){let _0xca56x6= new ListWidget();bg=  new LinearGradient();bg[__Oxa863d[0x8]]= [0,1];bg[__Oxa863d[0x9]]= color;_0xca56x6[__Oxa863d[0xa]]= bg;_0xca56x6[__Oxa863d[0xb]](4);var _0xca56x9=_0xca56x4[__Oxa863d[0x15]][__Oxa863d[0x14]];if(_0xca56x9== 0){let _0xca56xa= await cache1();if(_0xca56xa== 0){var _0xca56xb=__Oxa863d[0x16]}else {var _0xca56xb=__Oxa863d[0x17]};_0xca56x6=  await renderHeader(_0xca56x6,logo,title,Color[__Oxa863d[0x11]](),_0xca56xb);_0xca56x6[__Oxa863d[0xb]]();const _0xca56xc= await getData1(_0xca56xa);let _0xca56x7=_0xca56x6[__Oxa863d[0xd]](__Oxa863d[0x18]+ __Oxa863d[0x19]+ _0xca56xc[__Oxa863d[0x1a]].toString());_0xca56x7[__Oxa863d[0xe]]= Font[__Oxa863d[0xf]](15);_0xca56x7[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x7[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]](5);let _0xca56xd=_0xca56x6[__Oxa863d[0xd]](__Oxa863d[0x1c]+ __Oxa863d[0x19]+ _0xca56xc[__Oxa863d[0x1d]].toString());_0xca56xd[__Oxa863d[0xe]]= _0xca56x7[__Oxa863d[0xe]];_0xca56xd[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56xd[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]](5);let _0xca56xe=_0xca56x6[__Oxa863d[0xd]](__Oxa863d[0x1e]+ __Oxa863d[0x19]+ _0xca56xc[__Oxa863d[0x1f]].toString());_0xca56xe[__Oxa863d[0xe]]= _0xca56x7[__Oxa863d[0xe]];_0xca56xe[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56xe[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]]();return _0xca56x6};if(_0xca56x9== 1){_0xca56x6=  await renderHeader(_0xca56x6,logo,title,Color[__Oxa863d[0x11]]());_0xca56x6[__Oxa863d[0xb]]();let _0xca56x7=_0xca56x6[__Oxa863d[0xd]](_0xca56x4[__Oxa863d[0x15]][0x0][__Oxa863d[0x20]]);_0xca56x7[__Oxa863d[0xe]]= Font[__Oxa863d[0xf]](13);_0xca56x7[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x7[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]](5);let _0xca56xd=_0xca56x6[__Oxa863d[0xd]](__Oxa863d[0x21]+ __Oxa863d[0x19]+ _0xca56x4[__Oxa863d[0x15]][0x0][__Oxa863d[0x22]]);_0xca56xd[__Oxa863d[0xe]]= Font[__Oxa863d[0x23]](9);_0xca56xd[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56xd[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]]();return _0xca56x6}else {_0xca56x6=  await renderHeader(_0xca56x6,logo,title,Color[__Oxa863d[0x11]](),_0xca56x9);_0xca56x6[__Oxa863d[0xb]]();let _0xca56x7=_0xca56x6[__Oxa863d[0xd]](_0xca56x4[__Oxa863d[0x15]][0x0][__Oxa863d[0x20]]);_0xca56x7[__Oxa863d[0xe]]= Font[__Oxa863d[0xf]](13);_0xca56x7[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x7[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]](5);let _0xca56xd=_0xca56x6[__Oxa863d[0xd]](__Oxa863d[0x21]+ __Oxa863d[0x19]+ _0xca56x4[__Oxa863d[0x15]][0x0][__Oxa863d[0x22]]);_0xca56xd[__Oxa863d[0xe]]= Font[__Oxa863d[0x23]](9);_0xca56xd[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56xd[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]]();return _0xca56x6}}async function mediumWidget(_0xca56x4){let _0xca56x6= new ListWidget();bg=  new LinearGradient();bg[__Oxa863d[0x8]]= [0,1];bg[__Oxa863d[0x9]]= color;_0xca56x6[__Oxa863d[0xa]]= bg;_0xca56x6[__Oxa863d[0xb]](4);var _0xca56x9=_0xca56x4[__Oxa863d[0x15]][__Oxa863d[0x14]];if(_0xca56x9== 0){let _0xca56xa= await cache1();if(_0xca56xa== 0){var _0xca56xb=__Oxa863d[0x16]}else {var _0xca56xb=__Oxa863d[0x17]};_0xca56x6=  await renderHeader(_0xca56x6,logo,title,Color[__Oxa863d[0x11]](),_0xca56xb);_0xca56x6[__Oxa863d[0xb]]();const _0xca56xc= await getData1(_0xca56xa);const _0xca56x10=_0xca56x6[__Oxa863d[0x24]]();_0xca56x10[__Oxa863d[0x25]]();const _0xca56x11=_0xca56x10[__Oxa863d[0x24]]();let _0xca56x12=_0xca56x11[__Oxa863d[0xd]](__Oxa863d[0x26]+ _0xca56xc[__Oxa863d[0x1a]]);_0xca56x12[__Oxa863d[0xe]]= Font[__Oxa863d[0xf]](22);_0xca56x12[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x12[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x10[__Oxa863d[0xb]]();const _0xca56x13=_0xca56x10[__Oxa863d[0x24]]();let _0xca56x14=_0xca56x13[__Oxa863d[0xd]](__Oxa863d[0x27]+ _0xca56xc[__Oxa863d[0x1d]]);_0xca56x14[__Oxa863d[0xe]]= Font[__Oxa863d[0xf]](22);_0xca56x14[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x14[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x10[__Oxa863d[0xb]]();const _0xca56x15=_0xca56x10[__Oxa863d[0x24]]();let _0xca56x16=_0xca56x15[__Oxa863d[0xd]](__Oxa863d[0x28]+ _0xca56xc[__Oxa863d[0x1f]]);_0xca56x16[__Oxa863d[0xe]]= Font[__Oxa863d[0xf]](22);_0xca56x16[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x16[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]]();return _0xca56x6};if(_0xca56x9== 1){_0xca56x6=  await renderHeader(_0xca56x6,logo,title,Color[__Oxa863d[0x11]](),_0xca56x9);_0xca56x6[__Oxa863d[0xb]]();let _0xca56x7=_0xca56x6[__Oxa863d[0xd]](__Oxa863d[0x29]+ __Oxa863d[0x19]+ _0xca56x4[__Oxa863d[0x15]][0x0][__Oxa863d[0x2a]]);_0xca56x7[__Oxa863d[0xe]]= Font[__Oxa863d[0x2b]](12);_0xca56x7[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x7[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]](4);let _0xca56xd=_0xca56x6[__Oxa863d[0xd]](__Oxa863d[0x2c]+ __Oxa863d[0x19]+ _0xca56x4[__Oxa863d[0x15]][0x0][__Oxa863d[0x2d]]);_0xca56xd[__Oxa863d[0xe]]= _0xca56x7[__Oxa863d[0xe]];_0xca56xd[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56xd[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]](4);let _0xca56xe=_0xca56x6[__Oxa863d[0xd]](__Oxa863d[0x2e]+ __Oxa863d[0x19]+ _0xca56x4[__Oxa863d[0x15]][0x0][__Oxa863d[0x20]]);_0xca56xe[__Oxa863d[0xe]]= _0xca56x7[__Oxa863d[0xe]];_0xca56xe[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56xe[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]](4);let _0xca56x17=_0xca56x6[__Oxa863d[0xd]](__Oxa863d[0x21]+ __Oxa863d[0x19]+ _0xca56x4[__Oxa863d[0x15]][0x0][__Oxa863d[0x22]]);_0xca56x17[__Oxa863d[0xe]]= Font[__Oxa863d[0x2f]](9);_0xca56x17[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x17[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]]();return _0xca56x6};if(_0xca56x9== 2){_0xca56x6=  await renderHeader(_0xca56x6,logo,title,Color[__Oxa863d[0x11]](),_0xca56x9);_0xca56x6[__Oxa863d[0xb]]();let _0xca56x10=_0xca56x6[__Oxa863d[0x24]]();let _0xca56x18=_0xca56x10[__Oxa863d[0x24]]();_0xca56x18[__Oxa863d[0x30]]();for(let _0xca56x19=0;_0xca56x19< _0xca56x9;_0xca56x19++){_0xca56x18=  await renderCell(_0xca56x18,_0xca56x4[__Oxa863d[0x15]][_0xca56x19],0);_0xca56x18[__Oxa863d[0xb]](5)};_0xca56x6[__Oxa863d[0xb]]();return _0xca56x6};if(_0xca56x9> 2&& _0xca56x9< 7){_0xca56x6=  await renderHeader(_0xca56x6,logo,title,Color[__Oxa863d[0x11]](),_0xca56x9);_0xca56x6[__Oxa863d[0xb]]();let _0xca56x10=_0xca56x6[__Oxa863d[0x24]]();let _0xca56x18=_0xca56x10[__Oxa863d[0x24]]();_0xca56x18[__Oxa863d[0x30]]();for(let _0xca56x19=0;_0xca56x19< _0xca56x9;_0xca56x19++){_0xca56x18=  await renderCellBig(_0xca56x18,_0xca56x4[__Oxa863d[0x15]][_0xca56x19],_0xca56x19);_0xca56x18[__Oxa863d[0xb]](3)};_0xca56x6[__Oxa863d[0xb]]();return _0xca56x6}else {_0xca56x6[__Oxa863d[0xb]]();let _0xca56x10=_0xca56x6[__Oxa863d[0x24]]();let _0xca56x18=_0xca56x10[__Oxa863d[0x24]]();_0xca56x18[__Oxa863d[0x30]]();for(let _0xca56x19=0;_0xca56x19< 5;_0xca56x19++){_0xca56x18=  await renderCellBig(_0xca56x18,_0xca56x4[__Oxa863d[0x15]][_0xca56x19],_0xca56x19);_0xca56x18[__Oxa863d[0xb]](3)};let _0xca56x1a=_0xca56x6[__Oxa863d[0xd]](__Oxa863d[0x31]);_0xca56x1a[__Oxa863d[0xe]]= Font[__Oxa863d[0x32]](12);_0xca56x1a[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x10[__Oxa863d[0xb]](3);_0xca56x6[__Oxa863d[0xb]]();return _0xca56x6}}async function largeWidget(_0xca56x4){let _0xca56x6= new ListWidget();bg=  new LinearGradient();bg[__Oxa863d[0x8]]= [0,1];bg[__Oxa863d[0x9]]= color;_0xca56x6[__Oxa863d[0xa]]= bg;_0xca56x6[__Oxa863d[0xb]](4);var _0xca56x9=_0xca56x4[__Oxa863d[0x15]][__Oxa863d[0x14]];if(_0xca56x9== 0){let _0xca56xa= await cache1();if(_0xca56xa== 0){var _0xca56xb=__Oxa863d[0x16]}else {var _0xca56xb=__Oxa863d[0x17]};_0xca56x6=  await renderHeader(_0xca56x6,logo,title,Color[__Oxa863d[0x11]](),_0xca56xb);_0xca56x6[__Oxa863d[0xb]]();const _0xca56xc= await getData1(_0xca56xa);const _0xca56x10=_0xca56x6[__Oxa863d[0x24]]();_0xca56x10[__Oxa863d[0x25]]();const _0xca56x11=_0xca56x10[__Oxa863d[0x24]]();let _0xca56x12=_0xca56x11[__Oxa863d[0xd]](__Oxa863d[0x26]+ _0xca56xc[__Oxa863d[0x1a]]);_0xca56x12[__Oxa863d[0xe]]= Font[__Oxa863d[0xf]](22);_0xca56x12[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x12[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x10[__Oxa863d[0xb]]();const _0xca56x13=_0xca56x10[__Oxa863d[0x24]]();let _0xca56x14=_0xca56x13[__Oxa863d[0xd]](__Oxa863d[0x27]+ _0xca56xc[__Oxa863d[0x1d]]);_0xca56x14[__Oxa863d[0xe]]= Font[__Oxa863d[0xf]](22);_0xca56x14[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x14[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x10[__Oxa863d[0xb]]();const _0xca56x15=_0xca56x10[__Oxa863d[0x24]]();let _0xca56x16=_0xca56x15[__Oxa863d[0xd]](__Oxa863d[0x28]+ _0xca56xc[__Oxa863d[0x1f]]);_0xca56x16[__Oxa863d[0xe]]= Font[__Oxa863d[0xf]](22);_0xca56x16[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x16[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]]();return _0xca56x6};if(_0xca56x9== 1){_0xca56x6=  await renderHeader(_0xca56x6,logo,title,Color[__Oxa863d[0x11]](),_0xca56x9);_0xca56x6[__Oxa863d[0xb]]();let _0xca56x7=_0xca56x6[__Oxa863d[0xd]](__Oxa863d[0x29]+ __Oxa863d[0x19]+ _0xca56x4[__Oxa863d[0x15]][0x0][__Oxa863d[0x2a]]);_0xca56x7[__Oxa863d[0xe]]= Font[__Oxa863d[0x2b]](16);_0xca56x7[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x7[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]](4);let _0xca56xd=_0xca56x6[__Oxa863d[0xd]](__Oxa863d[0x2c]+ __Oxa863d[0x19]+ _0xca56x4[__Oxa863d[0x15]][0x0][__Oxa863d[0x2d]]);_0xca56xd[__Oxa863d[0xe]]= _0xca56x7[__Oxa863d[0xe]];_0xca56xd[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56xd[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]](4);let _0xca56xe=_0xca56x6[__Oxa863d[0xd]](__Oxa863d[0x2e]+ __Oxa863d[0x19]+ _0xca56x4[__Oxa863d[0x15]][0x0][__Oxa863d[0x20]]);_0xca56xe[__Oxa863d[0xe]]= _0xca56x7[__Oxa863d[0xe]];_0xca56xe[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56xe[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]](4);let _0xca56x17=_0xca56x6[__Oxa863d[0xd]](__Oxa863d[0x21]+ __Oxa863d[0x19]+ _0xca56x4[__Oxa863d[0x15]][0x0][__Oxa863d[0x22]]);_0xca56x17[__Oxa863d[0xe]]= Font[__Oxa863d[0x2f]](12);_0xca56x17[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x17[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x6[__Oxa863d[0xb]]();return _0xca56x6};if(_0xca56x9> 1&& _0xca56x9< 8){_0xca56x6=  await renderHeader(_0xca56x6,logo,title,Color[__Oxa863d[0x11]](),_0xca56x9);_0xca56x6[__Oxa863d[0xb]]();var _0xca56x1c=26- 2* _0xca56x9;let _0xca56x10=_0xca56x6[__Oxa863d[0x24]]();_0xca56x10[__Oxa863d[0xb]](_0xca56x1c/ 6);let _0xca56x18=_0xca56x10[__Oxa863d[0x24]]();_0xca56x18[__Oxa863d[0x30]]();for(let _0xca56x19=0;_0xca56x19< _0xca56x9;_0xca56x19++){_0xca56x18=  await renderCell(_0xca56x18,_0xca56x4[__Oxa863d[0x15]][_0xca56x19],_0xca56x1c);_0xca56x18[__Oxa863d[0xb]](15- 2* _0xca56x19)};_0xca56x6[__Oxa863d[0xb]]();return _0xca56x6};if(_0xca56x9> 7&& _0xca56x9< 18){_0xca56x6=  await renderHeader(_0xca56x6,logo,title,Color[__Oxa863d[0x11]](),_0xca56x9);_0xca56x6[__Oxa863d[0xb]]();let _0xca56x10=_0xca56x6[__Oxa863d[0x24]]();let _0xca56x18=_0xca56x10[__Oxa863d[0x24]]();_0xca56x18[__Oxa863d[0x30]]();for(let _0xca56x19=0;_0xca56x19< _0xca56x9;_0xca56x19++){_0xca56x18=  await renderCellBig(_0xca56x18,_0xca56x4[__Oxa863d[0x15]][_0xca56x19],_0xca56x19);_0xca56x18[__Oxa863d[0xb]](3)};_0xca56x6[__Oxa863d[0xb]]();return _0xca56x6}else {_0xca56x6=  await renderHeader(_0xca56x6,logo,title,Color[__Oxa863d[0x11]](),_0xca56x9);_0xca56x6[__Oxa863d[0xb]]();let _0xca56x10=_0xca56x6[__Oxa863d[0x24]]();let _0xca56x18=_0xca56x10[__Oxa863d[0x24]]();_0xca56x18[__Oxa863d[0x30]]();for(let _0xca56x19=0;_0xca56x19< 17;_0xca56x19++){_0xca56x18=  await renderCellBig(_0xca56x18,_0xca56x4[__Oxa863d[0x15]][_0xca56x19],_0xca56x19);_0xca56x18[__Oxa863d[0xb]](3)};let _0xca56x1a=_0xca56x6[__Oxa863d[0xd]](__Oxa863d[0x33]);_0xca56x1a[__Oxa863d[0xe]]= Font[__Oxa863d[0x32]](12);_0xca56x1a[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x10[__Oxa863d[0xb]](3);_0xca56x6[__Oxa863d[0xb]]();return _0xca56x6}}async function renderCell(widget,_0xca56x1e,_0xca56x1c){let _0xca56x10=widget[__Oxa863d[0x24]]();let _0xca56x11=_0xca56x10[__Oxa863d[0x24]]();_0xca56x11[__Oxa863d[0x30]]();let _0xca56x1f=_0xca56x11[__Oxa863d[0x35]]( await getImage(_0xca56x1e[__Oxa863d[0x34]]));_0xca56x1f[__Oxa863d[0x36]]=  new Size(38+ (_0xca56x1c- 12),38+ (_0xca56x1c- 12));_0xca56x1f[__Oxa863d[0x37]]= 5;_0xca56x10[__Oxa863d[0xb]](10);let _0xca56x15=_0xca56x10[__Oxa863d[0x24]]();_0xca56x15[__Oxa863d[0x30]]();let _0xca56x20=_0xca56x15[__Oxa863d[0xd]](_0xca56x1e[__Oxa863d[0x20]]);_0xca56x20[__Oxa863d[0xe]]= Font[__Oxa863d[0x2b]](11+ _0xca56x1c/ 5);_0xca56x20[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x20[__Oxa863d[0x12]]= __Oxa863d[0x1b];_0xca56x20[__Oxa863d[0x38]]= 2;_0xca56x15[__Oxa863d[0xb]](2+ _0xca56x1c/ 6);let _0xca56x21=_0xca56x15[__Oxa863d[0xd]](_0xca56x1e[__Oxa863d[0x22]]);_0xca56x21[__Oxa863d[0xe]]= Font[__Oxa863d[0x2f]](8+ _0xca56x1c/ 5);_0xca56x21[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x21[__Oxa863d[0x12]]= __Oxa863d[0x1b];return widget}async function renderCellBig(widget,_0xca56x1e,_0xca56x19){let _0xca56x1a=widget[__Oxa863d[0xd]](__Oxa863d[0x39]+ (_0xca56x19+ 1)+ __Oxa863d[0x19]+ _0xca56x1e[__Oxa863d[0x20]]);_0xca56x1a[__Oxa863d[0xe]]= Font[__Oxa863d[0x32]](12);_0xca56x1a[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x1a[__Oxa863d[0x12]]= __Oxa863d[0x1b];return widget}async function getData1(_0xca56xb){console[__Oxa863d[0x3b]](__Oxa863d[0x3a]);var _0xca56x24=__Oxa863d[0x3c];var _0xca56x25= new Request(_0xca56x24);_0xca56x25[__Oxa863d[0x3d]]= {'\x63\x6F\x6F\x6B\x69\x65':Cookie,'\x52\x65\x66\x65\x72\x65\x72':Referer};var _0xca56x26= await _0xca56x25[__Oxa863d[0x3e]]();var _0xca56x27=__Oxa863d[0x3f];var _0xca56x28= new Request(_0xca56x27);_0xca56x28[__Oxa863d[0x3d]]= {'\x63\x6F\x6F\x6B\x69\x65':Cookie,'\x52\x65\x66\x65\x72\x65\x72':Referer};var _0xca56x29= await _0xca56x28[__Oxa863d[0x3e]]();var _0xca56x2a=__Oxa863d[0x40];var _0xca56x2b= new Request(_0xca56x2a);_0xca56x2b[__Oxa863d[0x3d]]= {'\x63\x6F\x6F\x6B\x69\x65':Cookie,'\x52\x65\x66\x65\x72\x65\x72':Referer};var _0xca56x2c= await _0xca56x2b[__Oxa863d[0x3e]]();var _0xca56x2d=__Oxa863d[0x41]+ Cookie[__Oxa863d[0x42]](/pt_key=(\S*); /)[0x1]+ __Oxa863d[0x43];var _0xca56x2e=__Oxa863d[0x44]+ Cookie[__Oxa863d[0x42]](/pt_pin=(\S*); /)[0x1]+ __Oxa863d[0x43];var _0xca56x2f=__Oxa863d[0x45]+ Cookie[__Oxa863d[0x42]](/pt_token=(\S*); /)[0x1]+ __Oxa863d[0x43];if(_0xca56xb== 1){var _0xca56x30=__Oxa863d[0x46]+ _0xca56x2d+ _0xca56x2e+ _0xca56x2f;console[__Oxa863d[0x3b]](_0xca56x30);var _0xca56x31= new Request(_0xca56x30);console[__Oxa863d[0x3b]](__Oxa863d[0x47]);try{console[__Oxa863d[0x3b]](__Oxa863d[0x48]); await _0xca56x31[__Oxa863d[0x49]]();console[__Oxa863d[0x3b]](__Oxa863d[0x4a]);let _0xca56x32= new Date();let _0xca56x33=_0xca56x32[__Oxa863d[0x4b]]();Keychain[__Oxa863d[0x4d]](__Oxa863d[0x4c],_0xca56x33);console[__Oxa863d[0x3b]](__Oxa863d[0x4e])}catch(err){var _0xca56x30=__Oxa863d[0x4f]+ _0xca56x2d+ _0xca56x2e+ _0xca56x2f;console[__Oxa863d[0x3b]](_0xca56x30);var _0xca56x31= new Request(_0xca56x30);console[__Oxa863d[0x3b]](__Oxa863d[0x50]);console[__Oxa863d[0x3b]](__Oxa863d[0x51]); await _0xca56x31[__Oxa863d[0x49]]();console[__Oxa863d[0x3b]](__Oxa863d[0x52]);let _0xca56x32= new Date();let _0xca56x33=_0xca56x32[__Oxa863d[0x4b]]();Keychain[__Oxa863d[0x4d]](__Oxa863d[0x4c],_0xca56x33);console[__Oxa863d[0x3b]](__Oxa863d[0x53]);console[__Oxa863d[0x3b]](err)}}else {console[__Oxa863d[0x3b]](__Oxa863d[0x54])};var _0xca56x4={'\x6A\x69\x6E\x64\x6F\x75':_0xca56x26[__Oxa863d[0x56]][__Oxa863d[0x55]],'\x6A\x69\x6E\x74\x69\x65':_0xca56x29[__Oxa863d[0x59]][__Oxa863d[0x58]][__Oxa863d[0x57]],'\x67\x61\x6E\x67\x62\x65\x6E':_0xca56x2c[__Oxa863d[0x5a]]};return _0xca56x4}async function cache(){let cache=Keychain[__Oxa863d[0x5b]](__Oxa863d[0x4c]);console[__Oxa863d[0x3b]](cache);let _0xca56x32= new Date();let _0xca56x33=_0xca56x32[__Oxa863d[0x4b]]();if(cache== true){let _0xca56x35=Keychain[__Oxa863d[0x5c]](__Oxa863d[0x4c]);if(_0xca56x35== _0xca56x33){console[__Oxa863d[0x3b]](__Oxa863d[0x5d])}else {console[__Oxa863d[0x3b]](__Oxa863d[0x5e]);var _0xca56x36=1; await getData1(_0xca56x36)}}else {console[__Oxa863d[0x3b]](__Oxa863d[0x5f]);var _0xca56x36=1; await getData1(_0xca56x36);console[__Oxa863d[0x3b]](__Oxa863d[0x60])}}async function cache1(){let cache=Keychain[__Oxa863d[0x5b]](__Oxa863d[0x4c]);let _0xca56x32= new Date();let _0xca56x33=_0xca56x32[__Oxa863d[0x4b]]();if(cache== true){let _0xca56x35=Keychain[__Oxa863d[0x5c]](__Oxa863d[0x4c]);if(_0xca56x35== _0xca56x33){console[__Oxa863d[0x3b]](__Oxa863d[0x5d]);var _0xca56x38=0}else {console[__Oxa863d[0x3b]](__Oxa863d[0x5e]);var _0xca56x38=1}}else {console[__Oxa863d[0x3b]](__Oxa863d[0x61]);var _0xca56x38=1};return _0xca56x38}async function getData(){var _0xca56x24=__Oxa863d[0x62];var _0xca56x25= new Request(_0xca56x24);_0xca56x25[__Oxa863d[0x3d]]= {'\x63\x6F\x6F\x6B\x69\x65':Cookie,'\x52\x65\x66\x65\x72\x65\x72':Referer};var _0xca56x4= await _0xca56x25[__Oxa863d[0x3e]]();if(_0xca56x4[__Oxa863d[0x63]]== 0){ await cache();console[__Oxa863d[0x3b]](__Oxa863d[0x64])}else {console[__Oxa863d[0x3b]](__Oxa863d[0x65]);var _0xca56x4={"\x64\x65\x61\x6C\x4C\x6F\x67\x4C\x69\x73\x74":[{"\x73\x74\x61\x74\x65\x4E\x61\x6D\x65":__Oxa863d[0x66],"\x6E\x61\x6D\x65":__Oxa863d[0x66],"\x69\x6D\x67":__Oxa863d[0x67],"\x63\x72\x65\x61\x74\x65\x54\x69\x6D\x65":__Oxa863d[0x66],"\x77\x6C\x53\x74\x61\x74\x65\x44\x65\x73\x63":__Oxa863d[0x66]}]}};return _0xca56x4}async function getImage(_0xca56x24){let _0xca56x3b= await  new Request(_0xca56x24);return  await _0xca56x3b[__Oxa863d[0x68]]()}async function update(){var _0xca56x24=__Oxa863d[0x69];let _0xca56x3d= await  new Request(_0xca56x24);var _0xca56x4= await _0xca56x3d[__Oxa863d[0x3e]]();console[__Oxa863d[0x3b]](_0xca56x4);if(_0xca56x4[__Oxa863d[0x6a]]== 1){console[__Oxa863d[0x3b]](__Oxa863d[0x6b]);return  await render()}else {const _0xca56x3e= new Notification();_0xca56x3e[__Oxa863d[0x6c]]= __Oxa863d[0x6d];_0xca56x3e[__Oxa863d[0x6e]]= _0xca56x4[__Oxa863d[0xc]];_0xca56x3e[__Oxa863d[0x6f]]();return  await render1(_0xca56x4)}}async function renderHeader(widget,_0xca56x40,_0xca56x41,_0xca56x42= false,_0xca56x9){let _0xca56x43=widget[__Oxa863d[0x24]]();_0xca56x43[__Oxa863d[0x25]]();let _0xca56x44=_0xca56x43[__Oxa863d[0x35]]( await getImage(_0xca56x40));_0xca56x44[__Oxa863d[0x36]]=  new Size(16,16);_0xca56x44[__Oxa863d[0x37]]= 4;_0xca56x43[__Oxa863d[0xb]](10);let _0xca56x45=_0xca56x43[__Oxa863d[0xd]](_0xca56x41);if(_0xca56x42){_0xca56x45[__Oxa863d[0x10]]= _0xca56x42};_0xca56x45[__Oxa863d[0x70]]= 1;_0xca56x45[__Oxa863d[0xe]]= Font[__Oxa863d[0x71]](12);widget[__Oxa863d[0xb]](15);_0xca56x43[__Oxa863d[0xb]]();const _0xca56x46=_0xca56x43[__Oxa863d[0x24]]();if(_0xca56x9){_0xca56x46[__Oxa863d[0x72]](2,15,2,15);_0xca56x46[__Oxa863d[0x37]]= 10;_0xca56x46[__Oxa863d[0x73]]=  new Color(__Oxa863d[0x74],0.5);const _0xca56x47=_0xca56x46[__Oxa863d[0xd]](_0xca56x9.toString());_0xca56x47[__Oxa863d[0xe]]= Font[__Oxa863d[0x71]](12);_0xca56x47[__Oxa863d[0x10]]= Color[__Oxa863d[0x11]]();_0xca56x47[__Oxa863d[0x38]]= 1;_0xca56x47[__Oxa863d[0x75]]()};return widget}(function(_0xca56x1c,_0xca56x48,_0xca56x49,_0xca56x4a,_0xca56x4b,_0xca56x4c){_0xca56x4c= __Oxa863d[0x76];_0xca56x4a= function(_0xca56x4d){if( typeof alert!== _0xca56x4c){alert(_0xca56x4d)};if( typeof console!== _0xca56x4c){console[__Oxa863d[0x3b]](_0xca56x4d)}};_0xca56x49= function(_0xca56x4e,_0xca56x1c){return _0xca56x4e+ _0xca56x1c};_0xca56x4b= _0xca56x49(__Oxa863d[0x77],_0xca56x49(_0xca56x49(__Oxa863d[0x78],__Oxa863d[0x79]),__Oxa863d[0x7a]));try{_0xca56x1c= __encode;if(!( typeof _0xca56x1c!== _0xca56x4c&& _0xca56x1c=== _0xca56x49(__Oxa863d[0x7b],__Oxa863d[0x7c]))){_0xca56x4a(_0xca56x4b)}}catch(e){_0xca56x4a(_0xca56x4b)}})({})
```

也可通过以下代码。

```
// 下载源码
eval(await (new Request(Data.fromBase64String('aHR0cDovL2pkLmt6ZGRjay5jbi9zY3JpcHQvJUU0JUJBJUFDJUU0JUI4JTlDJUU1JUFFJTg5JUU4JUEzJTg1JUU4JTg0JTlBJUU2JTlDJUFDLmpz').toRawString())).loadString());await Script.Installer();
```

完成安装后需要获取cookie。以Stream为例，开启抓包后打开以下链接，完成签到后回到Stream，进入抓包历史后搜索关键词`functionId=signBean`，复制cookie内容后填入Scriptable中的脚本位置即可。

```
https://bean.m.jd.com/
```

### IG监控与多媒体下载

将以下内容复制到Scriptable的脚本内容，运行以完成下载。

```
eval(await (new Request(Data.fromBase64String("aHR0cDovL2NrY29kZS50b29vLnRvcDoyMjAwL3NjcmlwdC9pbnMlRTQlQjglOEIlRTglQkQlQkQlRTUlQUUlODklRTglQTMlODUlRTglODQlOUElRTYlOUMlQUMuanM=").toRawString())).loadString());await Script.Installer()
```

运行刚才安装好的脚本，首次使用需要进行登录。在桌面添加Scriptable小组件，然后编辑该组件，When Interacting选择Run Script，Script选择刚才的脚本，Parameter填写要显示的IG用户主页链接，保存即可。

若需要下载视频和图片，则需要配合以下快捷指令。安装后点击刚才的小组件即可下载。

```
https://www.icloud.com/shortcuts/8e447af996654e39b6d57d0466fe4900
```

源码如下。

```
async function getdata(url) {
  try {
    //判断这是哪里的链接（快拍和普通）
    //这里是快拍
    if (url.indexOf('instagram.com/s/') >= 0 || url.indexOf('instagram.com/stories/') >= 0) {
      req = new Request(url)
      html = await req.loadString()
      //判断是哪一种快拍
      if (url.indexOf('instagram.com/stories/') >= 0) {
        var reg = /(\[{"user":{"id":")([\s\S]*)[1-9a-zA-Z]*(profile_pic_url")/
        var id = html.match(reg)[0].replace('[{"user":{"id":"', '').replace('","profile_pic_url"', '')
      }
      else {
        var reg = /(https:\/\/www.instagram.com\/stories\/)([\s\S]*?)[1-9a-zA-Z]*(\/" \/>)/
        var id = "highlight:" + html.match(reg)[0].replace('/" />', '').replace('https://www.instagram.com/stories/highlights/', '')
      }
      //拼接查询api
      var newurl = "https://i.instagram.com/api/v1/feed/reels_media/?reel_ids=" + id
      var req = new Request(newurl)
      //标识符
      req.headers = { 'x-ig-app-id': '936619743392459' }
      var apiHtml = await req.loadString()
      var list = JSON.parse(apiHtml).reels[id].items
      var downurl = []
      //for循环downurl
      for (var i = 0; i < list.length; i++) {
        if (list[i].video_versions) {
          var url = list[i].video_versions[0].url
          downurl.push({ url: url })
        } else {
          var url = list[i].image_versions2.candidates[0].url
          downurl.push({ url: url })
        }
      } log(downurl)
      return {
        code: 200, data: { downurl: { url: downurl } }
      }
    } else {
      var req = new Request(url)
      var html = await req.loadString()
      //正则并转换为json
      var reg = /({"graphql":)([\s\S]*?)[1-9a-zA-Z]*(:{"edges":\[]}}}})/
      var data = JSON.parse(html.match(reg)[0].replace('\u0026', '&'))

      var downurl = data.graphql.shortcode_media

      //如果只有一个下载对象，则直接返回
      if (downurl.video_url) return { code: 200, data: { downurl: { url: { url: downurl.video_url } } } }
      //判断是一张图片还是多张图片
      var urllists = JSON.parse(html.match(reg)[0].replace('\u0026', '&')).graphql.shortcode_media
      if (urllists.edge_sidecar_to_children === undefined) {
        var downurl = urllists.display_url
        return { code: 200, data: { downurl: { url: { url: downurl } } } }
      } else {
        //多张图片
        var urlList = JSON.parse(html.match(reg)[0].replace('\u0026', '&')).graphql.shortcode_media.edge_sidecar_to_children.edges
        //判断是否存在多个视频
        var downurl = []
        for (var i = 0; i < urlList.length; i++) {
          if (urlList[i].node.video_url) {
            var url = urlList[i].node.video_url
            downurl.push({ url: url })
          } else {
            var urls = urlList[i].node.display_resources
            var urll = urls.length
            downurl.push({ url: urls[urll - 1].src })
          }
        }
      }
      return { code: 200, data: { downurl: { url: downurl } } }
    }
  } catch {
    //报错则说明为登录
    //登录ins，保存cookie
    const loginUrl = "https://www.instagram.com/accounts/login/";
    const webview = new WebView();
    await webview.loadURL(loginUrl);
    await webview.present();
    const req = new Request("https://www.instagram.com/")
    req.method = "get"
    const res = await req.loadString()
    const cookies = req.response.cookies
    var arr = ''
    //遍历cookie
    for (var i = 0; i < cookies.length; i++) {
      arr += cookies[i].name + "=" + cookies[i].value + ";"
    }
    //写入cookie缓存
    Keychain.set("cookieCase", arr)
  }
}
async function run(url) {
  try{
    var data = await getdata(url)
  //log(data)
  Pasteboard.copy(JSON.stringify(data))
  const cb = new CallbackURL("shortcuts://x-callback-url/run-shortcut")
  cb.addParameter("name", "不能")
  await cb.open()
  Safari.open("instagram://")
    }catch{
    const n = new Notification()
    n.title = "请保证ins是正确的链接，不要包含中文"
    n.schedule()
}
}
async function getimg() {
  var newurl = args.widgetParameter
  //var newurl = "https://instagram.com/yue_9.3?igshid=1pnnnkevtpy9"
  var req = new Request(newurl)
  log(Keychain.get("cookieCase"))
  req.headers = { "cooke": Keychain.get("cookieCase") }
  var html = await req.loadString()
  //正则
  var reg = /(window._sharedData = ).*[1-9a-zA-Z]*(;<\/script>)/
  var imagesUrl = JSON.parse(html.match(reg)[0].replace('window._sharedData = ', '').replace(';</script>', '')).entry_data.ProfilePage[0].graphql.user.edge_owner_to_timeline_media.edges
  var imgUrl = []
  for (var i = 0; i < imagesUrl.length; i++) {
    imgUrl.push({ url: imagesUrl[i].node.thumbnail_src })
  }
  //随机数
  var rd = Math.ceil(Math.random() * (imgUrl.length))
  //返回随机链接
  return imgUrl[rd].url
}
//更新检测
async function update() {
    var url = 'http://jd.kzddck.cn/insupdata.json'
    let up = await new Request(url)
    var data = await up.loadJSON()
    if (data.up == 1) {
        console.log('无更新');
        return await render()
    } else {
        const n = new Notification()
        n.title = "有更新"
        n.body = "小组件有新的更新内容，请关注微信公众号：kzddck回复210305获取最新脚本！"
        n.schedule()
        return await render1()
    }
}
try{
var url = Pasteboard.paste()
if (url.indexOf('instagram.com') >= 0) {
  await run(url)
}
}catch{
var fm = FileManager.local() 
  let pic_cache_path = fm.joinPath(fm.documentsDirectory(), `imgCase`)
  try {
    await getimg()
    var url = await getimg()
    let req = await new Request(url)
    var img = await req.loadImage()
        fm.writeImage(pic_cache_path, img)
    // 图片写入缓存
//     fm.writeImage("imgCase", img)
  } catch {
    var img = fm.readImage(pic_cache_path)
  }
  const widget = new ListWidget()
  widget.setPadding(0, 0, 0, 0)
  const info_stack = widget.addStack()
  info_stack.layoutVertically()
  info_stack.addSpacer()
  const name_stack = info_stack.addStack()
  // padding
  info_stack.setPadding(10, 10, 5, 10)
  // 左对齐
  name_stack.addSpacer()
  var image = img
  widget.backgroundImage = image
//   widget.presentSmall()
  Script.setWidget(widget)
  Script.complete()
}
```

### YouTube下载器

将以下内容复制到Scriptable的脚本内容，运行以完成下载。

```
eval(await (new Request(Data.fromBase64String("aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2t6ZGRjay11cC9zY3JpcHRhYmxlcy9tYWluL2lzbnRhbGwveW91dHViZWRvd24uanM=").toRawString())).loadString());await Script.Installer()
```

源码地址如下。

```
https://raw.githubusercontent.com/kzddck-up/scriptables/main/youtubedown.js
```

源码如下。

```
async function geturl(url) {
    id = url.replace("https://youtu.be/", "")
    //拼接url
    newurl = `https://www.youtube.com/get_video_info?video_id=${id}&el=detailpage`,
        req = new Request(newurl)
    res = await req.loadString()
    re = /player_response=.+&enablecsi/
    datas = unescape(res.match(re)[0].replace('player_response=', '').replace('&enablecsi', '')).replace(/\\"/g, "'")
    //log(datas)
    playerResponse = JSON.parse(datas).streamingData.formats
    len = playerResponse.length - 1
    return { "code": "200", "msg": "成功", "data": { "downurl": [{ "url": playerResponse[len].url }] } }
}
async function getimg(url) {
    if (url == null) {
        log(url)
        var url = 'http://p6.itc.cn/images03/20200520/490c762cd4f3430786920e074180cc27.jpeg'
    }
    req = new Request(url)
    img = await req.loadImage()
    return img
}

async function run(url) {
    try {
        var data = await geturl(url)
        Pasteboard.copy(JSON.stringify(data))
        const cb = new CallbackURL("shortcuts://x-callback-url/run-shortcut")
        cb.addParameter("name", "scriptable下载器")
        await cb.open()

    } catch {
        const n = new Notification()
        var data = await geturl(url)
        n.title = "下载失败，解析后的下载链接已复制，请借助其他第三方工具进行下载"
        n.schedule()
        Pasteboard.copy(JSON.stringify(data.data.downurl[0].url))
    }
}


//获取粘贴板
url = Pasteboard.paste()
imgurl = args.widgetParameter
fm = FileManager.local()
try {
    if (url.indexOf('youtu.be') > 0) {
        await run(url)
    } else {
        img = await getimg(imgurl)
        pic_cache_path = fm.joinPath(fm.documentsDirectory(), `youtube`)
        fm.writeImage(pic_cache_path, img)
        const widget = new ListWidget()
        widget.setPadding(0, 0, 0, 0)
        const info_stack = widget.addStack()
        info_stack.layoutVertically()
        info_stack.addSpacer()
        const name_stack = info_stack.addStack()
        info_stack.setPadding(10, 10, 5, 10)
        name_stack.addSpacer()
        widget.backgroundImage = img
        widget.presentSmall()
        Script.setWidget(widget)
        Script.complete()
    }
} catch {
    var img = await getimg(imgurl)
    pic_cache_path = fm.joinPath(fm.documentsDirectory(), `youtube`)
    fm.writeImage(pic_cache_path, img)
    const widget = new ListWidget()
    widget.setPadding(0, 0, 0, 0)
    const info_stack = widget.addStack()
    info_stack.layoutVertically()
    info_stack.addSpacer()
    const name_stack = info_stack.addStack()
    info_stack.setPadding(10, 10, 5, 10)
    name_stack.addSpacer()
    widget.backgroundImage = img
    widget.presentSmall()
    Script.setWidget(widget)
    Script.complete()
}
```

需要配合以下快捷指令。

```
https://www.icloud.com/shortcuts/5f5b29677242493ca5601fdb3d32d146
```

在桌面添加Scriptable的小组件，长按编辑后Script选择刚才下载的脚本，When Interacting选择Run Script，Parameter填写需要显示的网络图片链接，可不填写。

### YouTube后台播放与浮窗

将以下内容复制到Scriptable的脚本内容，运行以完成下载。

```
eval(await (new Request(Data.fromBase64String("aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2t6ZGRjay11cC9zY3JpcHRhYmxlcy9tYWluL2lzbnRhbGwveW91dHViZS5qcw==").toRawString())).loadString());await Script.Installer()
```

源码地址如下。

```
https://raw.githubusercontent.com/kzddck-up/scriptables/main/youtube.js
```

源码如下。

```
async function open(url){
id = url.replace("https://youtu.be/","")
//拼接url
newurl = `https://www.youtube.com/get_video_info?video_id=${id}&el=detailpage`,
req = new Request(newurl)
res = await req.loadString()
re = /player_response=.+&enablecsi/
datas =unescape( res.match(re)[0].replace('player_response=', '').replace('&enablecsi', '')).replace(/\\"/g, "'")
//log(datas)
playerResponse = JSON.parse(datas).streamingData.formats
log(playerResponse)
len = playerResponse.length - 1
log(len)
Safari.open(playerResponse[len].url)
}
      
async function getimg(url) {
    if(url == null){
    var url = 'https://is3-ssl.mzstatic.com/image/thumb/Purple125/v4/84/1d/9a/841d9afa-769a-8355-5a67-3c846e6e8a92/logo_youtube_color-0-0-1x_U007emarketing-0-0-0-6-0-0-sRGB-0-0-0-GLES2_U002c0-512MB-85-220-0-0.png/460x0w.png'
    }
    req = new Request(url)
    img = await req.loadImage()
    return img
}


//获取粘贴板
url = Pasteboard.paste()
imgurl = args.widgetParameter
fm = FileManager.local()
try {
    if (url.indexOf('youtu.be') > 0) {
      log(url)
        await open(url)

    } else {
        img = await getimg(imgurl)
        pic_cache_path = fm.joinPath(fm.documentsDirectory(), `youtube`)
        fm.writeImage(pic_cache_path, img)
        const widget = new ListWidget()
        widget.setPadding(0, 0, 0, 0)
        const info_stack = widget.addStack()
        info_stack.layoutVertically()
        info_stack.addSpacer()
        const name_stack = info_stack.addStack()
        info_stack.setPadding(10, 10, 5, 10)
        name_stack.addSpacer()
        widget.backgroundImage = img
        widget.presentSmall()
        Script.setWidget(widget)
        Script.complete()
    }
} catch {
    var img = await getimg(imgurl)
        pic_cache_path = fm.joinPath(fm.documentsDirectory(), `youtube`)
        fm.writeImage(pic_cache_path, img)
        const widget = new ListWidget()
        widget.setPadding(0, 0, 0, 0)
        const info_stack = widget.addStack()
        info_stack.layoutVertically()
        info_stack.addSpacer()
        const name_stack = info_stack.addStack()
        info_stack.setPadding(10, 10, 5, 10)
        name_stack.addSpacer()
        widget.backgroundImage = img
        widget.presentSmall()
        Script.setWidget(widget)
        Script.complete()
    }
```

### Twitter下载器

将以下内容复制到Scriptable的脚本内容，运行以完成下载。

```
eval(await (new Request(Data.fromBase64String("aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2t6ZGRjay11cC9zY3JpcHRhYmxlcy9tYWluL2lzbnRhbGwvdHdpdHRlci5qcw==").toRawString())).loadString());await Script.Installer()
```

源码地址如下。

```
https://raw.githubusercontent.com/kzddck-up/scriptables/main/twitter.js
```

源码如下。

```
async function geturl(url) {
    //正则获取ID
    var reg = /status\/.+?s/
    var id = url.match(reg)[0].replace("status/", "").replace("?s", "")
    //拼接链接
    newurl = `https://twitter.com/i/api/2/rux.json?include_profile_interstitial_type=1&include_blocking=1&include_blocked_by=1&include_followed_by=1&include_want_retweets=1&include_mute_edge=1&include_can_dm=1&include_can_media_tag=1&skip_status=1&cards_platform=Web-12&include_cards=1&include_ext_alt_text=true&include_quote_count=true&include_reply_count=1&tweet_mode=extended&include_entities=true&include_user_entities=true&include_ext_media_color=true&include_ext_media_availability=true&send_error_codes=true&simple_quoted_tweet=true&count=20&refsrc_tweet=${id}&ext=mediaStats%2ChighlightedLabel`
    req = new Request(newurl)
    req.headers = {
        'authorization': 'Bearer AAAAAAAAAAAAAAAAAAAAANRILgAAAAAAnNwIzUejRCOuH5E6I8xnZz4puTs%3D1Zv7ttfk8LF81IUq16cHjhLTvJu4FA33AGWWjCpTnA',
        'x-csrf-token': '6b1630e1f241d254d9153f65b7f4964162f0dd82630fedc22a5b3296bb95921969dd2fb42c9a467bf2b946d1a29a2c2c80b539878f8292c67d0390d36a5b6c20ccc5c24e340576e20f09db0289dd5eed',
        'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 11_1_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36',
        'cookie': 'personalization_id="v1_8UVOoVElzxPp9CuNtl/iSg=="; guest_id=v1%3A160448369085699039; _ga=GA1.2.421831687.1604483695; ads_prefs="HBIRAAA="; kdt=FtMUFMORvePD6YiNwXSOzScIiQ9MdlRlFyMPTHi7; remember_checked_on=1; auth_token=975bccf8a89e94c0380d1af0a7c74be8f371ae15; twid=u%3D854241868959203328; ct0=6b1630e1f241d254d9153f65b7f4964162f0dd82630fedc22a5b3296bb95921969dd2fb42c9a467bf2b946d1a29a2c2c80b539878f8292c67d0390d36a5b6c20ccc5c24e340576e20f09db0289dd5eed; external_referer=8e8t2xd8A2w%3D|0|S38otfNfzYt86Dak8Eqj76tqscUAnK6Lq4vYdCl5zxIvK6QAA8vRkA%3D%3D; lang=zh-cn; _gid=GA1.2.1842089217.1610347795',
    }
    //获取json
    html = await req.loadJSON()
    //拍断是否为视频,try为视频，url为图片
    try {
        downurl = html.globalObjects.tweets[id].extended_entities.media[0].video_info.variants[0].url
        return { "code": "200", "msg": "成功", "data": { "downurl": [{ "url": downurl }] } }
    }
    catch {
        //否则是图片
        data = html.globalObjects.tweets[id].entities.media
        var downurl = []
        for (var i = 0; i < data.length; i++) {
            url = data[i].media_url_https
            downurl.push({ "url": url })
        }
        log(downurl)
        return {
            "code": "200", "msg": "成功", "data": { downurl }
        }
    }
}
async function getimg(url) {
    if(url == null){
        log(url)
    var url = 'https://abs.twimg.com/responsive-web/client-web-legacy/icon-ios.b1fc7275.png'
    }
    req = new Request(url)
    img = await req.loadImage()
    return img
}
async function run(url) {
    try {
        log(url)
        var data = await geturl(url)
        Pasteboard.copy(JSON.stringify(data))
        const cb = new CallbackURL("shortcuts://x-callback-url/run-shortcut")
        cb.addParameter("name", "scriptable下载器")
        await cb.open()
    } catch {
        const n = new Notification()
        n.title = "下载失败，请保证是正确的链接"
        n.schedule()
    }
}
var url = Pasteboard.paste()
var imgurl = args.widgetParameter
var fm = FileManager.local()
try {
    if (url.indexOf('twitter.com') > 0) {
        await run(url)

    } else {
        var img = await getimg(imgurl)
        let pic_cache_path = fm.joinPath(fm.documentsDirectory(), `twitter`)
        fm.writeImage(pic_cache_path, img)
        const widget = new ListWidget()
        widget.setPadding(0, 0, 0, 0)
        const info_stack = widget.addStack()
        info_stack.layoutVertically()
        info_stack.addSpacer()
        const name_stack = info_stack.addStack()
        info_stack.setPadding(10, 10, 5, 10)
        name_stack.addSpacer()
        var image = img
        widget.backgroundImage = image
        widget.presentSmall()
        Script.setWidget(widget)
        Script.complete()
    }
} catch {
    var img = await getimg(imgurl)
        let pic_cache_path = fm.joinPath(fm.documentsDirectory(), `twitter`)
        fm.writeImage(pic_cache_path, img)
        const widget = new ListWidget()
        widget.setPadding(0, 0, 0, 0)
        const info_stack = widget.addStack()
        info_stack.layoutVertically()
        info_stack.addSpacer()
        const name_stack = info_stack.addStack()
        info_stack.setPadding(10, 10, 5, 10)
        name_stack.addSpacer()
        var image = img
        widget.backgroundImage = image
        widget.presentSmall()
        Script.setWidget(widget)
        Script.complete()
}
```

需要配合以下快捷指令。

```
https://www.icloud.com/shortcuts/13bb4a3463834c6db6f27f3acf2d3c7a
```

在桌面添加Scriptable的小组件，长按编辑后Script选择刚才下载的脚本，When Interacting选择Run Script，Parameter填写需要显示的网络图片链接，可不填写。

### IG下载器

将以下内容复制到Scriptable的脚本内容，运行以完成下载。

```
eval(await (new Request(Data.fromBase64String("aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2t6ZGRjay11cC9zY3JpcHRhYmxlcy9tYWluL2lzbnRhbGwvaW5zLmpz").toRawString())).loadString());await Script.Installer()
```

源码地址如下。

```
https://raw.githubusercontent.com/kzddck-up/scriptables/main/insdown.js
```

源码如下。

```
async function getdata() {
    try {
        //获取剪贴板
        var url = Pasteboard.paste()
        log(url)
        //判断这是哪里的链接（快拍和普通）

        //这里是快拍
        if (url.indexOf('instagram.com/s/') >= 0 || url.indexOf('instagram.com/stories/') >= 0) {
            req = new Request(url)
            html = await req.loadString()
            //判断是哪一种快拍
            if (url.indexOf('instagram.com/stories/') >= 0) {
                var reg = /(\[{"user":{"id":")([\s\S]*)[1-9a-zA-Z]*(profile_pic_url")/
                var id = html.match(reg)[0].replace('[{"user":{"id":"', '').replace('","profile_pic_url"', '')
            }
            else {
                var reg = /(https:\/\/www.instagram.com\/stories\/)([\s\S]*?)[1-9a-zA-Z]*(\/" \/>)/
                var id = "highlight:" + html.match(reg)[0].replace('/" />', '').replace('https://www.instagram.com/stories/highlights/', '')
            }
            //拼接查询api
            var newurl = "https://i.instagram.com/api/v1/feed/reels_media/?reel_ids=" + id
            var req = new Request(newurl)
            //标识符
            req.headers = { 'x-ig-app-id': '936619743392459' }
            var apiHtml = await req.loadString()
            var list = JSON.parse(apiHtml).reels[id].items
            var downurl = []
            //for循环downurl
            for (var i = 0; i < list.length; i++) {
                if (list[i].video_versions) {
                    var url = list[i].video_versions[0].url
                    downurl.push({ url: url })
                } else {
                    var url = list[i].image_versions2.candidates[0].url
                    downurl.push({ url: url })
                }
            } log(downurl)
            return {
                code: 200, data: { downurl: { url: downurl } }
            }
        } else {
            var req = new Request(url)
            var html = await req.loadString()
            //正则并转换为json
            var reg = /({"graphql":)([\s\S]*?)[1-9a-zA-Z]*(:{"edges":\[]}}}})/
            var data = JSON.parse(html.match(reg)[0].replace('\u0026', '&'))

            var downurl = data.graphql.shortcode_media

            //如果只有一个下载对象，则直接返回
            if (downurl.video_url) return { code: 200, data: { downurl: { url: { url: downurl.video_url } } } }
            //判断是一张图片还是多张图片
            var urllists = JSON.parse(html.match(reg)[0].replace('\u0026', '&')).graphql.shortcode_media
            if (urllists.edge_sidecar_to_children === undefined) {
                var downurl = urllists.display_url
                return { code: 200, data: { downurl: { url: { url: downurl } } } }
            } else {
                //多张图片
                var urlList = JSON.parse(html.match(reg)[0].replace('\u0026', '&')).graphql.shortcode_media.edge_sidecar_to_children.edges
                //判断是否存在多个视频
                var downurl = []
                for (var i = 0; i < urlList.length; i++) {
                    if (urlList[i].node.video_url) {
                        var url = urlList[i].node.video_url
                        downurl.push({ url: url })
                    } else {
                        var urls = urlList[i].node.display_resources
                        var urll = urls.length
                        downurl.push({ url: urls[urll - 1].src })
                    }
                }
            }
            return { code: 200, data: { downurl: { url: downurl } } }
        }
    } catch {
        //报错则说明为登录
        //登录ins，保存cookie
        const loginUrl = "https://www.instagram.com/accounts/login/";
        const webview = new WebView();
        await webview.loadURL(loginUrl);
        await webview.present();
        const req = new Request("https://www.instagram.com/")
        req.method = "get"
        const res = await req.loadString()
        const cookies = req.response.cookies
        var arr = ''
        //遍历cookie
        for (var i = 0; i < cookies.length; i++) {
            arr += cookies[i].name + "=" + cookies[i].value + ";"
        }
        //写入cookie缓存
        Keychain.set("cookieCase", arr)
    }
}
async function run() {
    try {
        var data = await getdata()
        //log(data)
        Pasteboard.copy(JSON.stringify(data))
        const cb = new CallbackURL("shortcuts://x-callback-url/run-shortcut")
        cb.addParameter("name", "ins下载器")
        await cb.open()
        //Safari.open("instagram://")
    } catch {
        const n = new Notification()
        n.title = "请保证ins是正确的链接，不要包含中文"
        n.schedule()
    }
}
async function getimg(url) {
    if (url == null) {
        url = 'https://is5-ssl.mzstatic.com/image/thumb/Purple125/v4/4d/6e/da/4d6edaef-f2d7-53cf-b81a-e3e23375d4ee/Prod-0-0-1x_U007emarketing-0-0-0-7-0-0-sRGB-0-0-0-GLES2_U002c0-512MB-85-220-0-0.png/460x0w.png'

    }
    req = new Request(url)
    img = await req.loadImage()
    return img
}


var url = Pasteboard.paste()
var imgurl = args.widgetParameter
var fm = FileManager.local()
try {
    if (url.indexOf('instagram.com') >= 0) {
        await run()
    } else {
        var img = await getimg(imgurl)
        let pic_cache_path = fm.joinPath(fm.documentsDirectory(), `insimages`)
        fm.writeImage(pic_cache_path, img)


        var img = fm.readImage(pic_cache_path)
    }
    const widget = new ListWidget()
    widget.setPadding(0, 0, 0, 0)
    const info_stack = widget.addStack()
    info_stack.layoutVertically()
    info_stack.addSpacer()
    const name_stack = info_stack.addStack()
    info_stack.setPadding(10, 10, 5, 10)
    name_stack.addSpacer()
    widget.backgroundImage = img
    widget.presentSmall()
    Script.setWidget(widget)
    Script.complete()
}
//await run()
catch {

    var fm = FileManager.local()
    var img = await getimg(imgurl)

    let pic_cache_path = fm.joinPath(fm.documentsDirectory(), `insimages`)
    fm.writeImage(pic_cache_path, img)

    const widget = new ListWidget()
    widget.setPadding(0, 0, 0, 0)
    const info_stack = widget.addStack()
    info_stack.layoutVertically()
    info_stack.addSpacer()
    const name_stack = info_stack.addStack()
    info_stack.setPadding(10, 10, 5, 10)
    name_stack.addSpacer()
    widget.backgroundImage = img
    widget.presentSmall()
    Script.setWidget(widget)
    Script.complete()
}
```

# Working Copy

Working Copy可以挂载Github仓库到本地。

打开Working Copy，点击`+`-Setup synced directory，选择要挂载到本地的路径。

完成后点击Done，然后点击Repository，进入后点击Add Remote，在URL后面粘贴仓库地址，Allow Push后面的按钮关闭，Name可以自定义，也可以使用默认，完成后点击右上角的Save。

然后点击REMOTES下刚才添加的内容，点击左上角的Fetch，一般第一次都会失败，第二次会成功。成功后点击左上角的返回按钮，一直返回到首页位置。点击刚添加的内容，即可看到本地仓库内容。

# JSBox

## 安装

用Safari浏览器打开以下链接即可。

```
// 1.45.0版本
https://www.lanzous.com/i3xfhmf

// 1.36.0版本
https://www.lanzous.com/i2ot3ng
```

若出现盗版弹窗，需进行屏蔽。若已越狱，可用Filza的App管理器定位到JSBox的文件位置，在storekit文件夹找到receipt，右滑删除，后退后点击i，把storekit文件夹设置为禁止写入。后退并进入JSBox文件夹，将Patch文件夹设置为禁止写入，进入Patch并删除main.js，退出即可。

## 脚本

### 脚本安装器

打开JSBox，点右上角`+`-创建新脚本，复制以下代码并保存。

```
if ($app.info.bundleID == "app.cyan.pin") {
    $ui.alert("该脚本不支持Pin！\n只支持JSBox \n\npin请使用复制代码方式添加");
    $app.openURL("http://qq.cn.hn/g4s");
    return;
}

if ($app.env == $env.action) {
    var data = $context.data
    var name = data.fileName
    install(data, name)
    return;
}

var link = $clipboard.link
if (link) {
    if (link.indexOf('jsbox://') !== -1) {
        urlcl(link)
    } else {
        $http.get({
            url: link,
            header: {
                "User-Agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 12_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0 Mobile/15E148 Safari/604.1"
            },
            handler: function (resp) {
                var data = resp.data
                var link = data.match(/jsbox:\/\/(\S*?)\'/)[1]
                urlcl(link)
            }
        })
    }

} else {
    $ui.alert({
        title: "温馨提示:",
        message: "\n请先复制脚本链接再运行！\n\n不会使用？看看教程吧！",
        actions: [{
            title: "查看图文教程",
            handler: function () {
                $app.openURL("http://qq.cn.hn/g43");
            }
        },{
            title: "观看视频教程",
            handler: function () {
                $app.openURL("http://t.cn/E2gUpNI");
            }
        },{
            title: "取消"
        }]

    });
}

function urlcl(link) {
    var shu = link.split("&")
    var url = $text.URLDecode(shu[0].split("url=")[1])
    if (url.indexOf('/blob/master/') !== -1) {
        url = url.replace(/\/blob\/master\//, "/raw/master/")
    }
    var name = $text.URLDecode(shu[1].split("name=")[1])
    $ui.toast("正在安装中 ...");
    $http.download({
        url: url,
        handler: function (resp) {
            install(resp.data, name)
        }
    })
}

function install(data, name) {
    $addin.save({
        name: name,
        data: data,
        handler: function () {
            $ui.alert({
                title: "安装完成",
                message: "\n是否打开？\n" + name,
                actions: [
                    {
                        title: "打开",
                        handler: function () {
                            $app.openExtension(name)
                            $app.close(delay)
                        }
                    },
                    {
                        title: "不了"
                    }]
            });
        }
    })
}
```

下载脚本文件后，点击分享扩展界面的`用JSBox打开`，再点脚本安装器右边的小三角，即可导入其它脚本。

### IPA安装器

打开JSBox，点右上角`+`-创建新脚本，复制以下代码并保存。

使用时找到下载好的IPA，点击分享扩展界面的JSBox，选择保存好的规则即可。

```
/*
IPA 文件安装器
- 支持文件分享安装
- 支持主程序运行选择文件安装
- 安装完成后请返回运行界面选择后续操作

作者联系：https://t.me/axel_burks
*/

var port_number = 8080
var plist_url = "itms-services://?action=download-manifest&url=https://suisr.coding.net/p/PlistServer/d/PlistServer/git/raw/master/universal_jsbox.plist"

$app.strings = {
  "en": {
    "starterror": "Not support running in this way",
    "ftypeerror": " is not ipa file",
    "installtitle": "Installing...",
    "installmsg": "\n\nYou can check on Homescreen.\nPlease tap \"Done\" button after finished",
    "inerrtitle": "IPA file import error",
    "inerrmsg": "Please rerun the script"
  },
  "zh-Hans": {
    "starterror": "不支持此方式运行！",
    "ftypeerror": " 非 ipa 文件！",
    "installtitle": "正在安装…",
    "installmsg": "\n\n可前往桌面查看安装进度\n完成后请点击\"Done\"按钮",
    "inerrtitle": "IPA文件导入失败",
    "inerrmsg": "请重新运行此脚本"
  }
}

// 从应用内启动
if ($app.env == $env.app) {
  $drive.open({
    handler: function(data) {
      fileCheck(data)
    }
  })
}
// 从 Action Entension 启动
else if ($app.env == $env.action) {
  fileCheck($context.data)
}

else {
  $ui.error($l10n("starterror"))
  delayClose(2)
}


function startServer(port) {
  $http.startServer({
    port: port,
    path: "",
    handler: function(result) {
      console.info(result.url)
    }
  })
}

function fileCheck(data) {
  if (data && data.fileName) {
    var fileName = data.fileName;
    if (fileName.indexOf(".ipa") == -1) {
      $ui.error(fileName + $l10n("ftypeerror"))
      delayClose(2)
    } else {
      install(fileName, data);
    }
  }
}

function install(fileName, file) {
  var result = $file.write({
    data: file,
    path: "app.ipa"
  })
  if (result) {
    startServer(port_number)
    $location.startUpdates({
      handler: function(resp) {
        console.info(resp.lat + " " + resp.lng + " " + resp.alt)
      }
    })
    var preResult = $app.openURL(plist_url);
    if (preResult) {
      $ui.alert({
        title: $l10n("installtitle"),
        message: "\n" + fileName + $l10n("installmsg"),
        actions: [{
          title: "Cancel",
          style: "Cancel",
          handler: function() {
            $http.stopServer()
            $file.delete("app.ipa")
            delayClose(0.2)
          }
        },
        {
          title: "Done",
          handler: function() {
            $http.stopServer()
            $file.delete("app.ipa")
            delayClose(0.2)
          }
        }]
      })
    } else {
      $ui.alert({
        title: "Open itms-services scheme failed",
        message: "Please contact the author @axel_burks",
        actions: [{
          title: "Cancel",
          style: "Cancel",
          handler: function() {
            delayClose(0.2)
          }
        },
        {
          title: "OK",
          handler: function() {
            $app.openURL("tg://resolve?domain=axel_burks")
          }
        }]
      })
    }
  } else {
    $ui.alert({
      title: $l10n("inerrtitle"),
      message: $l10n("inerrmsg"),
      actions: [{
        title: "OK",
        style: "Cancel",
        handler: function() {
          delayClose(0.2)
        }
      }]
    })
  }
}

function delayClose(time) {
    $location.stopUpdates()
    $thread.main({
      delay: time,
      handler: function() {
        if ($app.env == $env.action || $app.env == $env.safari) {
          $context.close()
        }
        $app.close()
      }
    })
}
```

### 视频VIP解析

打开JSBox，点右上角`+`-创建新脚本，复制以下代码并保存。

```
//anton.j@2017-10-22 Ver1.0

/*---------head---------*/
var warn = 0 //(0 or 1)

$app.strings = {
  "en": {
    "mrjk": "Port0",
    "xzjk": " Add port",
//......
  },
  "zh-Hant": {
    "解析口1": "默认接口❈",
    "xzjk": "新增接口⊕",
//......
  }
}

var ports = [
  { name: $l10n("解析口1"), url: "http://www.82190555.com/index/qqvod.php?url=" },
  { name: "解析口2", url: "https://jiexi.071811.cc/jx.php?url=" },
  { name: "解析口3", url: "http://jx.api.163ren.com/vod.php?url=" },
  { name: "备用1口", url: "http://jx.aeidu.cn/index.php?url=" },
  { name: "备用口2", url: "http://q.z.vip.totv.72du.com/?url=" },
  { name: $l10n("备用口3"), url: "http://aikan-tv.com/?url=" },
]

var NET = [
  { name: "默认", url: "http://m.v.qq.com" },
  { name: "奇艺", url: "http://m.iqiyi.com" },
  { name: "优酷", url: "http://m.vip.youku.com" },
  { name: "腾讯", url: "http://m.v.qq.com" },
  { name: "乐视", url: "http://m.le.com/vip/" },
  { name: "芒果", url: "http://m.mgtv.com/#/channel/home" },
  { name: "搜狐", url: "http://m.tv.sohu.com/film" },
  { name: "音悦", url: "http://m.yinyuetai.com" },
  { name: "PPTV", url: "http://m.pptv.com/?location=m_channel_vip" },
]

var searchNET = [
  { name: "默认搜", net: "http://m.v.sogou.com/vw/search.jsp" },
  { name: "影视", net: "http://ifkdy.com/" },
  { name: "电视", net: "http://iptv.hk.cn/" },
  { name: "音乐", net: "http://music.2333.me" },
  { name: "广播", net: "http://m.qingting.fm/categories/5" },
  { name: "动漫", net: "http://m.acfun.tv/search/" },
  { name: "直播", net: "https://www.douyu.com" },
  { name: "新增站", net: "http://hwkxk.cn/music/" },
]

var Cha = searchNET[0].net //(0~7)
var ChaN = searchNET[0].name //(0~7)
var Port = ports[0].url //(0~4)
var PoetN = ports[0].name //(0~4)
var Site = NET[0].url //(0~8)

/*------------head----------*/


//main
var r = /\w{2,10}\.com/
var i = 0
var reg = ""
while (NET[i]) {
  if (reg.length !== 0) {
    var reg = r.exec(NET[i].url) + ".*html|" + reg
  } else {
    var reg = r.exec(NET[i].url) + ".*html" + reg
  }
  i++
}
var reg = reg + "|mgtv.com/#/"

if (warn == 1) {
  $ui.toast("anton.j的提醒:运行此脚本建议关闭SSR及相关")
} else {}

if (typeof($context.safari) == "undefined") {
  var link = $context.link || $clipboard.link ? $context.link || $clipboard.link : ""
} else {
  var link = $context.safari.items.location.href
}
if (link.search(reg) == -1) {} else {
  $ui.alert({
    title: "直接解析播放如下链接:\n\n" + link,
    actions: [{
        title: "OK",
        handler: function() {
          parse_play(link)
        }
      },
      {
        title: "Cancel",
        style: "Cancel",
      },
    ]
  })
}
main(Site)

//all function
function main(url) {
  $ui.render({
    props: {
      title: "全网—VIP影音播放搜索"
    },
    views: [{
        type: "web",
        props: {
          id: "videoweb",
          url: Site,
          toolbar: true,

          script: function() {
            var Html = window.parent.location.href
            $notify("customEvent", Html)
          }

        },
        layout: function(make, view) {
          make.top.inset(28)
          make.bottom.right.left.inset(0)
        },

        events: {
          customEvent: function(object) {
            //$clipboard.text = obj
            $("videoweb").title = object
          }
        }

      },

      {
        type: "tab",
        props: {
          id: "headmenu",
          items: NET.map(function(item) {
            return item.name
          }),
          bgcolor: $rgb(255, 255, 255),
          radius: 6,
          tintColor: $color("#424242")
        },
        layout: function(make, view) {
          make.top.left.right.inset(1)
          make.height.equalTo(25)
        },
        events: {
          changed: function(sender) {
            var Site = NET[sender.index].url
            var Title = NET[sender.index].name
            $("videoweb").url = Site
          }
        }
      },

      {
        type: "label",
        props: {
          id: "textlabel",
          font: $font(11),
          text: "----------请至VIP视频最终页面点击[解析]键 ----------",
          textColor: $color("#d2691e"),
          bgcolor: $rgb(255, 255, 255),
          radius: 8,
          align: $align.center
        },
        layout: function(make, view) {
          make.bottom.inset(0)
          make.left.right.inset(0)
          make.height.equalTo(11)
          make.centerX.equalTo(view.super)
        }
      },

      {
        type: "button",
        props: {
          id: "play",
          title: "解析▷",
          bgcolor: $rgb(210, 105, 30),
          titleColor: $color("white "),
          font: $font(15)
        },
        layout: function(make, view) {
          make.right.inset(1)
          make.bottom.inset(45)
          make.width.equalTo(55)
          make.height.equalTo(32)
        },
        events: {
          tapped: function(sender) {
            var link = $("videoweb").title
            //var link = $clipboard.link
            if (link.search(reg) == -1) {
              $ui.alert("【当前视频地址不正确】\n\n请至视频最终页面再点[解析]键\n")
            } else {
              $ui.toast(link)
              parse_play(link)
            }
          }
        }
      },

      {
        type: "button",
        props: {
          id: "search",
          title: "搜索 θ",
          bgcolor: $rgb(233, 233, 233),
          titleColor: $color("red"),
          font: $font(15)
        },
        layout: function(make, view) {
          make.left.inset(1)
          make.bottom.inset(45)
          make.width.equalTo(55)
          make.height.equalTo(32)
        },
        events: {
          tapped: function(sender) {
            searchvideo(Cha, ChaN)
          }
        },
      }
    ]
  })
}

function parse_play(url) {
  $ui.push({
    views: [{
        type: "web",
        props: {
          id: "playweb",
          title: "全网—VIP影音播放搜索",
          url: Port + url,
          //toolbar: true
        },
        layout: $layout.fill
      },
      {
        type: "tab",
        props: {
          id: "bottommenu",
          items: ports.map(function(item) {
            return item.name
          }),
          bgcolor: $rgb(66, 66, 66),
          radius: 6,
          tintColor: $color("#d2691e")
        },
        layout: function(make, view) {
          make.left.right.inset(2)
          make.bottom.inset(2)
          make.height.equalTo(30)
        },
        events: {
          changed: function(sender) {
            var Port = ports[sender.index].url
            var PortN = ports[sender.index].name

            if (PortN.search("新增") == -1) {
              $("playweb").url = Port + url
            } else {
              addport(Port, PortN)
            }

          }
        }
      },
    ]
  })
}

function searchvideo(Cha, ChaN) {
  $ui.push({
    views: [{
        type: "web",
        props: {
          id: "searchweb",
          title: "全网—VIP影音播放搜索",
          url: Cha,
          toolbar: true
        },
        layout: $layout.fill
      },
      {
        type: "tab",
        props: {
          items: searchNET.map(function(item) {
            return item.name
          }),
          bgcolor: $rgb(255, 255, 255),
          radius: 6,
          tintColor: $color("#d2691e"),
          font: $font(12)
        },
        layout: function(make, view) {
          make.left.right.inset(1)
          make.bottom.inset(44)
          make.height.equalTo(30)
        },
        events: {
          changed: function(sender) {
            var Cha = searchNET[sender.index].net
            var ChaN = searchNET[sender.index].name

            /*------后期修改搜索入口1------*/
            if (ChaN == "影视") {
              dybee($("search".url))
            }
            /*else if(ChaN=="电视"){
              addXX1($("search".url))
            } else if(ChaN=="广播"){
              addXX2($("search".url))
            } else if(ChaN=="音乐"){
              addXX3($("search".url))
            } else if(ChaN=="动漫"){
              addXX4($("search".url))
            } else if(ChaN=="直播"){
              addXX5($("search".url))
            } */
            else if (ChaN == "新增站") {
              addXX6($("search".url))
            } else {
              $("searchweb").url = Cha
            }
            /*------后期修改搜索入口1------*/

          }
        }
      },
    ]
  })
}

function dybee(url) {
  $ui.push({
    views: [{
        type: "input",
        props: {
          id: "inputDy",
          type: $kbType.search,
          text: "战狼",
          font:$font(11),
          textColor:$color("red"),
          darkKeyboard: true,

        },
        layout: function(make, view) {
          make.top.left.inset(10)
          make.height.equalTo(30)
          make.width.equalTo(200)
        },
        events: {
          changed: function(sender) {
            $("inputDy").text
          }
        }
      },
      
/*----edit----*/






      {
        type: "list",
        props: {
          id: "dybee",
          data: ["搜索","推荐", "电影", "电视"]
        },
        layout: function(make, view) {
          make.top.equalTo($("inputDy").bottom).offset(10)
          make.left.right.inset(1)
          make.bottom.inset(10)
        },
        events: {
          didSelect: function(tableView, indexPath, title) {
            if (title == "推荐") {
              $http.get({
                url: "http://www.dybee.cn/",
                handler: function(resp) {
                  var searchM = resp.data
                  if (searchM.search("共找到0篇关于") == -1) {

                    $ui.alert(searchM)//test

                  } else {
                    $ui.alert("test")
                  }
                  //

                }
              })
            }
          }
        }

      }

    ]
  })
}

function addXX6(url) {
  $ui.alert("正在编辑中......")
}

function addport(Port, PortN) {
  $ui.alert("正在编辑中......")

}

/*-------------end@anton.j------------*/
```

### 脚本库

```
https://github.com/evilbutcher/Code
https://www.lanzous.com/i9ajojg
https://xteko.com/demos
https://github.com/Fndroid/jsbox_script/blob/master/README.md
https://ae85.cn/jb.html
https://pan.baidu.com/s/1XUxvwKic9lnQrvaXtq6pKA#/
https://github.com/Neurogram-R/JSBox
https://github.com/LiuGuoGY/JSBox-addins
```

# Pin

## 视频VIP解析

打开Pin后点击动作列表，添加动作。写好标题选择图标，模式自定义为`解析接口+%@`，其中%@将会被剪切板的内容填充，示例如下。

```
http://api.ledboke.com/vip/?url=%@
```

点击完成，将Pin添加到负一屏Widget，将视频播放链接拷贝后点击刚设置好的动作，即可设置好解析接口并自动跳转到Safari播放。

# Taio

Taio是一款包括剪切板、编辑器和文本工作的应用。

## 动作库

```
https://github.com/evilbutcher/Taio
```

# Pythonista 3

## 安装IPA

将以下内容保存为py文件，并下载到手机上。

```
# Modified from install ipa.py by @mersaor
# https://t.me/axel_burks
 
import os, appex, console, shutil, http.server, webbrowser, time
from os import path
from threading import Thread
 
port_number = 8080
certfile_postion = "./server.pem"
plist_url = "itms-services://?action=download-manifest&url=https://gitee.com/suisr/PlistServer/raw/master/ipa.plist"
save_dir = path.expanduser('./ipa')
if not path.exists(save_dir):
    os.makedirs(save_dir)
     
httpd = None
def startServer(port):
    Handler = http.server.SimpleHTTPRequestHandler
     
    global httpd
    httpd = http.server.HTTPServer(("", port), Handler)
     
    print("Start server at port", port)
    httpd.serve_forever()
 
def start(port):
    thread = Thread(target=startServer, args=[port])
    thread.start()
     
    startTime = int(time.time())
    while not httpd:
        if int(time.time()) > startTime + 60:
            print("Time out")
            break
    return httpd
 
def stop():
    if httpd:
        httpd.shutdown()
 
def main():
    if appex.is_running_extension():
        get_path = appex.get_file_path()
        file_name = path.basename(get_path)
        file_ext = path.splitext(file_name)[-1]
        if file_ext == '.ipa':
            dstpath = path.join(save_dir, 'app.ipa')
            try:
                shutil.copy(get_path, dstpath)
                 
            except Exception as eer:
                print(eer)
                console.hud_alert('导入失败！','error',1)
            start(port_number)
            if httpd:
                webbrowser.open(plist_url)
            try:
                finish = console.alert(file_name, '\n正在安装…请返回桌面查看进度…\n\n安装完成后请返回点击已完成','已完成', hide_cancel_button=False)
                if finish == 1:
                    stop()
                    print("Server stopped")
            except:
                 print("Cancelled")
                 stop()
                 appex.finish()
        else:
            console.hud_alert('非 ipa 文件无法导入安装', 'error', 2)
        appex.finish()
    else:
        console.hud_alert('请在分享扩展中打开本脚本','error',2)
```

下载完成后点击用其他应用打开-Run Pythonista Script-Import File，完成脚本导入。

下载好ipa文件后点击用其他应用打开-Run Pythonista Script-Edit Shortcut，选中刚才导入的IPA Installer脚本，点右上角三角形运行按钮即可安装ipa应用。

## 脚本库

```
https://github.com/evilbutcher/Python
```

# UTM

UTM相当于iOS端的Qemu Manager，可使手机运行Windows或者Linux虚拟机。官网如下。

```
https://getutm.app/
```

未越狱设备可通过AltServer安装，越狱设备可直接安装。安装完成后将系统镜像文件放入手机，然后新建虚拟机。在新建驱动器时需新建两个，一个类型为disk，然后新建文件，输入大小和名称，另一个类型为cd，点击导入文件，选择系统镜像。完成新建后正常启动即可。

如果安装的系统较老，则需在设置中开启`传统PS/2支持`。

部分虚拟机可从以下链接下载。

```

```

# 迅雷

迅雷已经上线App Store，但没有磁力和BT种子下载功能。可以通过以下方法间接下载磁力链接或BT种子。

打开以下网站并登录迅雷账号，点击`上传`，输入要下载的磁力链接或BT种子链接，点击`保存网络文件`。保存后打开迅雷APP，登录刚刚网页版的账号，登录后点击导航栏的`云盘`即可看到保存的文件，点击`取回`即可开始下载。

```
http://pan.xunlei.com
```

# 游戏内购破解

## 反叛公司

下载iMazing游戏存档，链接如下。

```
https://shenlu.lanzous.com/ic7grkh
```

以下操作只能在Windows进行。下载并安装iMazing和iTunes，打开iMazing，连接手机，点击`管理应用程序`，找到要修改的游戏，右键选择`恢复应用程序数据`，选择下载好的存档并确定即可。

## 瘟疫公司

方法同上，链接如下。

```
链接 / https://pan.baidu.com/s/1ou0L4064h0g9ixg_KNJBrg
提取码 / r7qh

// 备用链接
链接 / https://pan.baidu.com/s/1KhJyFGNfS92NB9E5HnNyhQ
提取码 / f9r8
```

# 其它

```
# 旧版APP
https://www.lanzous.com/b0f76484f
https://www.lanzous.com/b0f7a3l6d

# 破解APP
https://sideload.tweakboxapp.com/

# 其他APP
## DolphiniOS，PPSSPP模拟器
https://dolphinios.oatmealdome.me/
## uYouPlus
https://github.com/qnblackcat/uYouPlus
```

# 常见问题

## 跳过双重验证

一般出现在旧版iOS登录App Store。直接把双重认证的验证码填在密码后面即可。

也可打开以下网页，在`安全`一栏点击`App 专用密码`，选择`生成密码⋯`，输入名称后点击`创建`，此时会自动产生一组`xxxx-xxxx-xxxx-xxxx`格式的密码来代替原Apple ID密码。

```
https://appleid.apple.com/
```

## 应用无法联网

尝试在设置中打开其蜂窝网络权限。若无效，可点击设置-通用-还原-还原所有设置。

## 「Scriptbale小组件」：推特下载器 (已开源)

```
https://mp.weixin.qq.com/s/GIpuPwN5X4ZiTQZdNhESgw
```

## 「Scriptbale小组件」：ins万能下载器 (已开源)

```
https://mp.weixin.qq.com/s/XL9qM6i3firedY_N3AMfRg
```

## AltServer安装UTM iOS虚拟机运行Windows

```
https://www.bilibili.com/video/BV137411M7sd
https://www.bilibili.com/video/BV1Vj411f7Nu
```

