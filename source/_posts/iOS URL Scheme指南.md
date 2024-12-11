---
title: iOS URL Scheme指南
categories: iOS
abbrlink: iOS-URL-Scheme-Guide
date: 2023-11-15 13:18:29
tags:

---

![](topic.jpg)

URL Scheme通过类似网站形式的字符串，在浏览器打开时自动跳转到对应的APP。如在浏览器中输入`weixin://`则跳转到微信。

<!-- more -->

# 查询

## 通过电脑

用解压工具打开要查询的IPA，获得Payload文件夹，打开并在里面的app文件上右键，选择显示包内容。用Xcode等工具打开Info.plist，进入URL types下的URL Schemes即可看到。或者用文本编辑器打开Info.plist，搜索CFBundleURLTypes也可得到。

## 通过手机

越狱后通过Filza进入IPA查看即可，路径同上。

## 通过捷径

可通过以下捷径查询。

```
https://www.icloud.com/shortcuts/7803b1b01e6f443187651b6a57fee0f0
```

# 常用URL Schemes

## 腾讯系

```
# QQ
## 主页面
mqq://
## 群
mqqapi://card/show_pslcard?src_type=internal&version=1&card_type=group&uin=[QQ群号]
## 联系人
mqqapi://card/show_pslcard?src_type=internal&version=1&uin=[QQ号码]

# QQ HD
mqqflyticket://

# 微信
## 主页面
weixin://
wechat://
## 扫一扫
weixin://scanqrcode
# 微信读书
weread://

# TIM
## 主页面
tim://
## 商务聊天
timtribe://

# 滴滴
diditaxi://

# 腾讯新闻
qqnews://

# 腾讯微云
weiyun://

# 腾讯地图
sosomap://

# QQ音乐
## 主页面
QQmusic://
## 最近播放
qqmusic://today?mid=31&k1=2&k4=0

# QQ浏览器
mttbrowser://

# QQ斗地主
tencent382://

# QQ安全中心
qmtoken://

# QQ国际版
mqqiapi://

# QQ邮箱
qqmail://
qqquicklogin://

# 腾讯企业邮箱
qqbizmailDistribute2://

# 腾讯手机管家
mqqsecure://

# 腾讯视频
tenvideo://
wb2162695501://

# 腾讯微博
TencentWeibo://

# 全民K歌
qmkege://

# 王者荣耀
tencentlaunch1104466820://

# 王者荣耀助手
qw1105200115://

# 企业微信
wxwork://

# 腾讯动漫
comicreader://
```

## 阿里系

```
# 淘宝
taobao://

# 天猫
tmall://

# 支付宝
## 主页面
alipay://
## 扫一扫
alipayqr://platformapi/startapp?saId=10000007
## 付款
alipay://platformapi/startapp?appId=20000056
## 记账
alipay://platformapi/startapp?appId=20000168
## 滴滴
alipay://platformapi/startapp?appId=20000778
## 蚂蚁森林
alipay://platformapi/startapp?appId=60000002
## 转账
alipayqr://platformapi/startapp?saId=20000116
alipays://platformapi/startapp?appId=20000221
## 手机充值
alipayqr://platformapi/startapp?saId=10000003
## 口令红包
alipayqr://platformapi/startapp?saId=88886666
## 口令红包（带口令）
alipayqr://platformapi/startapp?saId=88886666&passcode=[code]
## 蚂蚁庄园
alipays://platformapi/startapp?appId=66666674
## 蚂蚁宝卡
alipays://platformapi/startapp?appId=60000057
## 股票
alipays://platformapi/startapp?appId=20000134
## 生活缴费
alipays://platformapi/startapp?appId=20000193
## 彩票
alipays://platformapi/startapp?appId=10000011
## 淘票票
alipays://platformapi/startapp?appId=20000131
## 查快递
alipays://platformapi/startapp?appId=20000754
## AA收款
alipays://platformapi/startapp?appId=20000263
## 收款
alipays://platformapi/startapp?appId=20000123
## 还信用卡
alipays://platformapi/startapp?appId=09999999

# 旺旺卖家版
wangwangseller://?

# 旺信
wangxin://?

# 菜鸟裹裹
cainiao://

# 钉钉
dingtalk://
```

## 百度系

```
# 百度云
baiduyun://

# 百度贴吧
com.baidu.tieba://

# 百度音乐
baidumusic://

# 百度地图
baidumap://

# 百度输入法
BaiduIMShop://

# 百度阅读
bdbook://

# 手机百度
bdboxiosqrcode://

# 百度视频
baiduvideoiphone://
bdviphapp://

# 百度糯米
bainuo://

# 百度魔图
photowonder://

# 百度魔拍
wondercamera://

# 百度导航
bdNavi://

# 百度浏览器
bdbrowser://

# 百度网盘
baiduyun://
```

## 网易系

```
# 网易云音乐
## 主页面
orpheuswidget://
## 播放已下载的音乐
orpheuswidget://download
## 听歌识曲
orpheuswidget://recognize
## 私人FM
orpheuswidget://radio

# 网易新闻
newsapp://

# 网易邮箱
neteasemail://

# 网易公开课
neteaseVopen://
ntesopen://

# 网易将军令
netease-mkey://?

# 有道词典
yddict://
yddictproapp://

# 有道翻译官
ydtranslator://

# 网易严选
yanxuan://

# 网易有钱
moneykeeperwiz://

# 网易漫画
necomics://

# 有道翻译官
ydtranslator://
```

## 银行

```
# 建设银行
wx2654d9155d70a468://

# 浦发银行
wx1cb534bb13ba3dbd://?

# 招商银行
cmbmobilebank://?

# 中国银行
BOCMBCIZF://

# 农业银行
bankabc://

# 中国工商银行
wb19490884://

# 邮政银行
wb1424286189://

# 网商银行
ap2016041301292400://
```

## 浏览器

```
# iCabMobile
iCabMobile://

# UC
ucbrowser://

# 猎豹
sinaweibosso.422729959://

# Chrome
## 主页面
googlechrome://
## 进剪切板链接
googlechrome://%@
## 用google搜索剪切板内容
googlechrome://www.google.com/#newwindow=1&q=%@

# 指尖
zhijian://

# Opera
oupeng-callback://

# 夸克
quark://

# 海豚
dolphin://

# Aloha
optly7634822906://

# 萝卜
wb3791188321://

# 鲨鱼
searchss://

# Take
TakeBrowser://

# Vip
wxbdba67b8ae3d296e://

# Ohajiki
oohttp://
```

## 视频

```
# Youtube
youtube://

# 哔哩哔哩
bilibili://

# 布卡漫画
buka://

# 爱奇艺
iqiyi://
qiyi-iphone://

# 优酷
youku://

# 土豆
tudou://

# PPTV
pptv://

# PPS
ppstream://

# 暴风影音
com.baofeng.play://

# 微视
weishiiosscheme://?

# ACfun
acfun://

# 抖音
awemesso://
wb1462309810://

# 西瓜
snssdk32://

# 搜狐
sohuvideo-iphone://
sohuvideo://

# 快手
gifshow://

# nPlayer
nplayer://

# AVPlayer
AVPlayer://

# yy直播
yymobile://

# 一直播
xktv://

# 花椒直播
huajiao://

# 咪咕
miguvideo://
```

## 社交

```
# 微博
weibo://

# 微博国际版
weibointernational://

# Twitter
Twitter://?
tweetie://

# Facebook
fb://

# Tumblr
tumblr://

# Telegram
tg://

# Instagram
instagram://
```

## 音乐

```
# 酷狗音乐
kugouURL://

# 天天动听
ttpod://

# 酷我音乐
com.kuwo.kwmusic.kwmusicForKwsing://?

# 虾米音乐
## 主页面
xiami://
## 每日推荐歌单
xiami://dailysong?action=play
## 私人电台
xiami://radio/private
## 虾米猜
xiami://radio/guess
## 每日30首
xiami://playdailysong
## 听歌识曲
xiami://soundhound

# 喜马拉雅
## 打开专辑
iting://open?msg_type=13&album_id=[专辑ID]
```

## 图片

```
# 抓图猫
imagecat://

# Piiic 2
wb1934889315://

# Picsew
## 主页面
picsew://
## 打开最近长截图
picsew://recent

# NOMO
nomocamera://

# Cuto Wallpaper
cuto://

# Picsew
picsew://
```

## 系统

```
# 照片
photos-redirect://

# 短信
sms://

# 备忘录
save://notes

# App Store
## 自动搜索剪贴板内容
itms-apps://search.itunes.apple.com/WebObjects/MZSearch.woa/wa/search?media=software&term=%@

# 偏好设置
## 无线局域网
App-Prefs:root=WIFI
## 蜂窝移动网络
App-Prefs:root=MOBILE_DATA_SETTINGS_ID

# Workflow
## 打开动作
workflow://run-workflow?name=[动作名]

# 电话
tel://[电话号码]
```

## 电商

```
# 虎扑
tencent222049://

# 闲鱼
laiwange48dbf143://
fleamarket://

# 拼多多
pinduoduo://

# 美团
imeituan://

# 京东
## 主页面
openapp.jdmobile://
## 早起打卡
openapp.jdmobile://virtual?params={"des":"jdreactcommon","modulename":"JDReactClockIn","category":"jump";}

# 多点
## 会员码
memberNumberDesktop://
## 扫码
scanDesktop://
```

## 记录

```
# 有道云笔记
youdaonote://

# 印象笔记
evernote://

# 我的标记
## 主页面
clover-imark://
## 新建1000*1000空白画布
clover-imark://new/canvas?width=1000&height=1000
## 快速新建画布（网页|底部|手机相册|相机|最新照片）
clover-imark://new/[|web|map|library|camera|latest]
```

## 搜索

```
# 360
msearchapp://

# 搜狗
sogousearch://
```

## 共享单车

```
# 摩拜
mobike://

# ofo
ofoapp://
```

## 学习

```
# 扇贝英语
shanbay://

# 小猿搜题
solar://

# 沪江小D词典
hjdict://

# TED
ted://

# 一席
wx5bc45646a79e74ab://
```

## 游戏

```
# 天天爱消除
tencentlaunch100689805://

# 阴阳师
com.netease.onmyoji://

# 叨鱼
sdguc://

# 崩坏三
bh3rd://

# 永远的七日之都
neteaseqrzd://

# 芝士超人
trivia://
```

## 阅读

```
# 樊登读书会
tbopen23329854://

# 多看阅读
duokan-reader://

# 当当阅读
alipayLogin20150804://

# 开卷有益
Kingreader://

# 拿铁阅读
LatteRead://
```

## 外卖

```
# 美团外卖
meituanwaimai://

# 饿了么
eleme://
```

## 其它

```
# 迅雷
thunder://?
QQ05FD1622://

# 115云盘
wb1307639798://

# 我的标记
clover-imark://

# 小米运动
fb370547106731052://

# 中国移动
wxbcb43ea5d2d6384c://

# 广东移动
wb3299513026://

# 探探
tantanapp://

# 相册宝
albumplus://

# 拍照取字
## 主页面
paizhaoquzi://
## 取最新一张图
paizhaoquzi://first

# 圈子账本
qzzb://

# JSBox
## 主页面
JSBox://
## 运行脚本
jsbox://run?name={编码后的脚本名}

# 京东金融
## 双签
jdmobile://share?jumpType=7&jumpUrl=1373

# 冲顶大会
tencent1105277177://

# 伙伴办公
Open://

# 下厨房
xcfapp://

# 有妖气
u17app://

# 花生地铁WiFi
wxa813eada7109a3cc://

# 少数派
sspai://
wx1cabf05524b18692://

# 车到哪儿
wx5207ffc5bbac1da7://

# 乐网
seven.adclear://

# 收趣
shouqu.me://

# 一个番茄
com.pomodorowang://

# 京东微联
wb1662044625://

# 户外助手
wb3060101786://

# 得到
dedaoapp://

# 掌上公交
wb1955957116://

# Seed
seedapp://

# 云墙
netfits://

# 潮汐
tide://

# Keep
keep://

# 今日头条
snssdk141://

# Gmail
googlegmail://

# 12306
cn.12306://

# Browser and File Manager for Documents
eidwl://
```

# 参考教程

## URL Scheme分享

```
https://st3376519.huoban.com/share/1985010/VGi2N5Vf0C1MVnHCVWiBc8L9g15c9VGJbMGcFrb6/172707/list
```
