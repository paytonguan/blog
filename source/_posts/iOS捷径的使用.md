---
title: iOS捷径的使用
categories: iOS
abbrlink: iOS-Shortcuts-Usage
date: 2024-08-09 11:35:29
tags:

---

![](topic.jpg)

iOS捷径的使用。

<!-- more -->

# TODOS

```
# 搭配其他APP

快捷指令＋Anki 实现快速输入、摘录、添加本地图片、划词添加
https://www.bilibili.com/video/BV1ct4y1m7bL

快捷指令＋iCost 实现相对自动化记账
https://sspai.com/post/66133

快捷指令＋vika 维格表（记账APP）
https://mp.weixin.qq.com/s/BeAQyGYLq52Jm6B-MfEFUg

快捷指令＋Seeds（笔记APP）
https://seedsnote.com/help/advanced/ios_shortcut/

快捷指令＋Notion（笔记APP）
https://www.bilibili.com/video/BV1C54y137nx

快捷指令＋印象笔记
https://zhuanlan.zhihu.com/p/373771525


# 校园网相关

实现drcom校园网自动登录
https://mp.weixin.qq.com/s/wkw4KkeuYC8BWXF3h5w4RQ

快速连接南京大学校园网
https://github.com/LadderOperator/MyShortcuts

实现南航校园网的一键登录
https://github.com/richardoLee/Shortcut-for-nuaa.portal

快速连接北京理工大学BIT-Web校园网
https://sspai.com/post/58189


# 日历相关

实现华东师范大学课表一键导入iOS日历
https://github.com/JJAYCHEN1e/ECNU_Bring-Your-Timetable-to-Calendar-App

实现南京大学课表一键导入iOS日历
https://github.com/LadderOperator/MyShortcuts

自动同步提醒事项和日历
https://zhuanlan.zhihu.com/p/169566930

将提醒事项同步到微软日历
https://sspai.com/post/63919

快捷指令＋日历（增强日历功能）
https://post.smzdm.com/p/alp6d9ke/


# 其他

快捷指令搭配NFC
https://zhuanlan.zhihu.com/p/89824463

实现自动换壁纸
https://sspai.com/post/64387

修改 iPhone 充电提示音
https://sspai.com/post/62846

实现自动判断上下班＋导航＋放歌
https://post.smzdm.com/p/ammqzvlz/

实现自动钉钉打卡
https://post.smzdm.com/p/ar0vp55w/

实现远程开机
https://post.smzdm.com/p/aoo8lzdm/

让你的 Siri 自动播报每日天气
https://mp.weixin.qq.com/s/8DlVNe_lLNvBowGSFSEgNQ

快速打开某一张图片
https://mp.weixin.qq.com/s/3pE8R3Mka95df9SiajHlmg

快捷设置提醒＋上班手机自动静音＋收盘前查看基金估值＋筛选快递短信并设置提醒等
https://www.bilibili.com/video/BV1gX4y1A7je

将网页文章推送至Kindle
https://sspai.com/post/60019

快速连接Sony耳机
https://sspai.com/post/56974
```

# 制作

打开快捷指令，点击+号新建。以下列例子进行说明。

## 下载微信推文图片

流程大致如下。

```
├── 获取剪贴板
├── 获取URL内容
├── 用多信息文本制作HTML
├── 匹配文本
│   └── data-src="(.*?)"
├── 获取匹配文本的组
├── 获取URL内容
└── 存储到相簿
```

点击添加操作，选择共享-获取剪贴板，长按并移动到蓝框内。点击+号添加操作，返回添加操作主页面，选择网页-获取URL内容，长按并移动到第一个动作的下方。

然后继续增加操作，搜索html并将`用多信息文本制作HTML`拖到下一个动作。复制微信推文后运行该捷径，寻找图片链接的规律，可发现均为`data-src="[图片链接]"`的格式。

故添加操作`匹配文本`，并将`[0-9a-zA-Z]`修改为`data-src="(.*?)"`。其中`()`代表组，便于后面提取图片链接，而`.*?`是非贪婪的匹配任何字符，即尽可能少的匹配，在本例中代表精确匹配所有图片链接。

添加操作`获取匹配文本的组`以取得图片链接，然后添加操作`获取URL内容`以下载图片，添加操作`存储到相簿`将图片存储到相簿，保存并运行即可。

## 下载小红书视频/图文

流程大致如下。

```
├── 获取剪贴板
└── 如果
    ├── 不含xhs
    │   ├── 显示提醒
    │   └── 退出快捷指令
    └── 含xhs
        ├── 获取URL内容
        │   ├── User-Agent
        │   └── Cookie
        ├── 从多信息文本制作HTML
        ├── 从菜单中选取
        │   ├── 下载视频
        │   │   ├── 匹配文本
        │   │   │   └── video src=\"(.*?)\"
        │   │   ├── 获取匹配文本的组
        │   │   ├── 替换文本
        │   │   │   └── 将amp;替换为空值
        │   │   ├── 获取URL内容
        │   │   ├── 存储到相簿
        │   │   └── 从菜单中选取
        │   │       ├── 保存文字内容
        │   │       │   ├── 匹配文本
        │   │       │   │   └── "name": "(.*?)\"
        │   │       │   ├── 获取匹配文本的组
        │   │       │   ├── 添加到变量
        │   │       │   │   └── 存入变量a
        │   │       │   ├── 匹配文本
        │   │       │   │   └── "comments":\d+,.*?"desc\":\"(.*?)\"
        │   │       │   ├── 获取匹配文本的组
        │   │       │   ├── 替换文本
        │   │       │   │   ├── 将\\n替换为空值
        │   │       │   │   └── 点开操作，打开正则表达式选项
        │   │       │   ├── 添加到变量
        │   │       │   │   └── 存入变量a
        │   │       │   └── 创建备忘录
        │   │       │       └── 使用变量a 
        │   │       └── 不保存文字内容      
        │   │           └── 结束菜单
        │   └── 下载图片
        │       ├── 匹配文本
        │       │   └── imageList\":\[(.*?)\]
        │       ├── 获取匹配文本的组
        │       ├── 匹配文本
        │       │   └── \"(\\u002F.*?format.*?)\"
        │       ├── 替换文本
        │       │   └── 将\u002F替换为/
        │       ├── 替换文本
        │       │   └── 将"替换为空值
        │       ├── 为每个项目重复
        │       │   ├── 替换文本
        │       │       └── 将//替换为https://
        │       ├── 获取URL内容
        │       ├── 从列表中选取
        │       │   ├── 选择想保存的图片
        │       │   └── 存储到相簿
        │       └── 从菜单中选取
        │           ├── 保存文字内容
        │           │   ├── 匹配文本
        │           │   │   └── "name": "(.*?)\"
        │           │   ├── 获取匹配文本的组
        │           │   ├── 添加到变量
        │           │   │   └── 存入变量b
        │           │   ├── 匹配文本
        │           │   │   └── "<div class="note"[\s\S]*class="content"\s*data-v-\w+?>([\s\S]*)</p></div></div>
        │           │   ├── 替换文本
        │           │   │   ├── 将<(\S*?)[^>]*>.*?|<.*? />替换为空值
        │           │   │   └── 点开操作，打开正则表达式选项
        │           │   ├── 添加到变量
        │           │   │   └── 存入变量b
        │           │   └── 创建备忘录
        │           │       └── 使用变量b
        │           └── 不保存文字内容      
        │               └── 结束菜单
        └── 退出快捷指令
```

添加操作`获取剪贴板`取得链接，添加操作`如果`判断链接是否为小红书分享链接。若不含xhs，则不是小红书链接，添加操作`显示提醒`提示用户错误信息，然后添加操作`退出快捷指令`结束。

若为小红书链接，则添加操作`获取URL内容`拿到网页内容，其中需要展开并添加以下两个头部值。

|     键     |                                                                                                                                                                                                                                 文本                                                                                                                                                                                                                                 |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| User-Agent | Mozilla/5.0 (iPhone; CPU iPhone OS 13_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1                                                                                                                                                                                                                                                                                                                            |
| Cookie     | timestamp2=c25daa113bced3fc44240e15d6c01368; hasaki=JTVCJTIyTW96aWxsYS81LjAlMjAoaVBob25lOyUyMENQVSUyMGlQaG9uZSUyME9TJTIwMTNfMyUyMGxpa2UlMjBNYWMlMjBPUyUyMFgpJTIwQXBwbGVXZWJLaXQvNjA1LjEuMTUlMjAoS0hUTUwsJTIwbGlrZSUyMEdlY2tvKSUyMFZlcnNpb24vMTMuMC40JTIwTW9iaWxlLzE1RTE0OCUyMFNhZmFyaS82MDQuMSUyMiwlMjJ6aC1DTiUyMiwzMywtNDgwLHRydWUsdHJ1ZSx0cnVlLCUyMnVuZGVmaW5lZCUyMiwlMjJ1bmRlZmluZWQlMjIsbnVsbCwlMjJpUGhvbmUlMjIsbnVsbCxudWxsLG51bGwsJTIyJTIyLDE5NDcxNjYxMDYlNUQ= |

此处使用了自定义的User-Agent。捷径默认的User-Agent如下。

```
快捷指令/1050.4.6CFNetwork/1121.2.2Darwin/19.2.0
```

添加操作`从多信息文本制作HTML`显示网页源代码，然后添加操作`从菜单中选取`，让用户选择是下载视频还是图文。

如果是下载视频，则添加操作`匹配文本`，使用正则表达式`video src=\"(.*?)\"`匹配，并添加操作`获取匹配文本的组`以取得视频链接。添加操作`替换文本`，将`amp;`替换为空值，然后添加操作`获取URL内容`以下载视频，添加操作`存储到相簿`将视频存储到相簿。

添加操作`从菜单中选取`，让用户选择是否保存文字内容。

若保存，则添加操作`匹配文本`，匹配`"name": "(.*?)\"`，以表示标题和作者。添加操作`获取匹配文本的组`，然后添加操作`添加到变量`，将内容存入变量`a`。然后添加操作`匹配文本`，匹配`"comments":\d+,.*?"desc\":\"(.*?)\"`，以表示正文。添加操作`获取匹配文本的组`，添加操作`替换文本`，将`\\n`替换为空，其中需要点开操作，打开`正则表达式`选项。添加操作`添加到变量`，将内容也存入变量`a`。至此变量`a`已存有所有需要的信息，故添加操作`创建备忘录`，使用a创建备忘录。

若不保存，则不进行操作。最后结束菜单。

如果是下载图片，则添加操作`匹配文本`，使用正则表达式`imageList\":\[(.*?)\]`匹配，并添加操作`获取匹配文本的组`以取得多个图片链接。再添加操作`匹配文本`，使用正则表达式`\"(\\u002F.*?format.*?)\"`匹配，从而得到精确的图片链接。添加操作`替换文本`，将`\u002F`替换为`/`，以将unicode编码转为中文，添加操作`替换文本`，将`"`替换为空以去掉两端的双引号，添加操作`为每个项目重复`，其操作为`替换文本`，将`//`替换为`https://`，至此得到完整的URL链接。添加操作`获取URL内容`从链接下载图片，添加操作`从列表中选取`选择想保存的图片，添加操作`存储到相簿`将图片存储到相簿。

添加操作`从菜单中选取`，让用户选择是否保存文字内容。

若保存，则添加操作`匹配文本`，匹配`"name": "(.*?)\"`（标题和作者），添加操作`获取匹配文本的组`，然后添加操作`添加到变量`，将内容存入变量`b`；添加操作`匹配文本`，匹配`"<div class="note"[\s\S]*class="content"\s*data-v-\w+?>([\s\S]*)</p></div></div>`（正文），添加操作`替换文本`，将`<(\S*?)[^>]*>.*?|<.*? />`替换为空，然后添加操作`添加到变量`，将内容也存入变量`b`。至此变量`b`已存有所有需要的信息，故添加操作`创建备忘录`，使用b创建备忘录。

若不保存，则不进行操作。最后结束菜单。

至此快捷指令结束，添加操作`退出快捷指令`即可。

# 捷径库

## 总仓库

```
https://shortcuts.sspai.com/
https://jiejingku.net/
https://jiejinghe.com/
https://sharecuts.cn/
https://jiejingku.net/category/shortcuts
```

## 声音

```
# 虾米音乐
https://www.icloud.com/shortcuts/a3f2abecad1c4ed7b35d81e67c11069a

# 网易云音乐
https://www.icloud.com/shortcuts/ce4f89e80cb7428ab82db298adec17dc

# QQ音乐声控
https://www.icloud.com/shortcuts/5741fbc3748d43d9a07fdd60ee0d8c5e

# QQ音乐
https://www.icloud.com/shortcuts/d16cb159b3d8447ebdd17390fe9813d2

# 音乐全网下载
https://www.icloud.com/shortcuts/be5bfd954e184cbbaccd0dae8189e818

# 中国广播电台FM
https://www.icloud.com/shortcuts/4c40fb48bac94c24b6a0d9bfba181424

# 录音
https://www.icloud.com/shortcuts/d114c56df5c44ffaa6492113037144c6

# 百度语音合成
https://www.icloud.com/shortcuts/c051b5e6cab94d70926e3b9bbb29af36

# 现在播放
https://workflow.is/workflows/3d954aa947474ed8b93d13f5b9d18625

# 随机播放
https://workflow.is/workflows/7e2dab4135c24808b36a5384cde60b7d

# 获取歌曲信息
https://workflow.is/workflows/70900391fdd6423ab2e6d925abf148cc

# 添加一些歌曲
https://workflow.is/workflows/c5f6d0b19e7e4463af581c0011860443

# 加入我的音乐
https://workflow.is/workflows/5bf4367cdef74e3cbe5d4078bdac9882

# 播放我的音乐
https://workflow.is/workflows/39d58771c84c4a50af50b3a0e1f7bda7

# Instagram分享音乐
https://workflow.is/workflows/402507226b8844258dee694d58b126e9

# QQ音乐无损下载
https://www.icloud.com/shortcuts/b2c51ca5e7324904aa5be02fae7c4045

# 网易云音乐无损下载
https://www.icloud.com/shortcuts/a4e38fb4529d4cf4b43f844264fdecb2

# 网易云助手
https://www.icloud.com/shortcuts/d6ed49f0b11e418ba5c7797af719068f

# 网易歌单工具箱
https://www.icloud.com/shortcuts/8d71ed4b433d490aa6f5e71d2f2b4ea9

# 音效盒子
https://www.icloud.com/shortcuts/8f3593e814244e32a16bc821896f1b5e

# 上朝网易云
https://www.icloud.com/shortcuts/401e94954a5a4a50951be0f2cf11372c
```

## 图片

```
# 应用图像至相机胶卷
https://workflow.is/workflows/613a8f5e6e714cc9aecaa4e76d981248

# 保存照片
https://workflow.is/workflows/a6487b004917494b88fe0fc01f0ffbc6

# 分享最新照片
https://workflow.is/workflows/5f9c56bfe6de447598a6d0440697c470

# 拼接图像
https://www.icloud.com/shortcuts/7a4129acb8df4f48b79ee930f1ce3d1f

# 拼接长图
https://www.icloud.com/shortcuts/07e7003f74b74216920b647eeb50ca17

# GIF制作转换
https://www.icloud.com/shortcuts/f810ee7228c244ed9707c3dbbcad5ef1

# 水印相机
https://www.icloud.com/shortcuts/65774ba01e514320b2640c40463a4cea

# 位置查找
# 以图查路线、拍照共享、清除照片元数据
https://www.icloud.com/shortcuts/ae804942349242ac98646c1c77c9aa7a

# 视频调速
https://www.icloud.com/shortcuts/799955de8e204b95b63c305268a168dc

# 以图搜图
https://www.icloud.com/shortcuts/59d6bfaae10a465da7366127166f5639

# 九宫格
https://www.icloud.com/shortcuts/8f8671ef0bff4545b3233d4415092e10

# 传图
https://www.icloud.com/shortcuts/2c04bce6e62b4a398df88c8acaff3428

# 删改拼图片
https://workflow.is/workflows/41cfefdd0eca448391ef78186555f221

# 截图加壳
https://www.icloud.com/shortcuts/e406b46a379548a69a1af503aeca37f4
https://www.icloud.com/shortcuts/2e6b1c0a0a114014812bf26897b5a2aa
https://www.icloud.com/shortcuts/7edc8f86d6ed4a96a43acacee36392de

# 删除多张图片
https://workflow.is/workflows/c545d9c33b0f40d8a80712b282a19eb3

# 调整照片
# 调整原始照片，然后删除原稿
https://workflow.is/workflows/014c0e9d263247979bc6a1c9894e07e4

# 爬图片
https://www.icloud.com/shortcuts/65ea0d09069545cfa9abd78839763146

# Bing壁纸
https://www.icloud.com/shortcuts/6d63fb492b094483aa18ce65b921ca0d

# 图标与图链
https://www.icloud.com/shortcuts/608014dbe70e45588653193fd8b1a4b9
```

## 视频

```
# 短视频去水印
https://www.icloud.com/shortcuts/cf5f76704f9444c9aa17e2a2f85b4f5a

# 拍摄GIF
https://www.icloud.com/shortcuts/0d77fb02b4d64b17858db35b5008d8c0

# 抖音去水印
https://www.icloud.com/shortcuts/c22783212e7a4918acf377546431fbeb

# 下载Tumblr视频
https://workflow.is/workflows/051ddcc30c984d47afe7b1fde9a403e7
https://workflow.is/workflows/f8656cbb90524dffa1fe2eba8975946e
https://workflow.is/workflows/087a0abf12c049dd9ab256afaec790c0

# 新浪微博视频
https://workflow.is/workflows/fd7fe0b7548046e689e9776b2182a8b1

# 下载Twitter的GIF
https://workflow.is/workflows/aa9821c4bce44b6f8febcbda6245c1f0

# 下载Youtube视频
https://workflow.is/workflows/ec284f18720c45e39ff31128e6d42602
https://workflow.is/workflows/73971870942c49da96cae6a490aea8e0
https://workflow.is/workflows/617578d73f8244a5afed2a1665c8e320

# 电视直播
https://www.icloud.com/shortcuts/0f9322e7b1244db68eb9a61d06dd757f
https://www.icloud.com/shortcuts/9a549e208d544532892f0fcc90dfaf8e

# SKY电影
https://www.icloud.com/shortcuts/f29d4a33f2734b3b8f790ea5d591fd1e

# 语音点播MV
https://www.icloud.com/shortcuts/d45086dbc5e042129ae2e7601cb41b5c

# 手机电视
https://www.icloud.com/shortcuts/0695a29bd0224b7f84e9b696adeb4330

# 在线电影
https://www.icloud.com/shortcuts/7f8220d6d2264fbb811c9f6add1fbe47

# 香港电视直播
https://www.icloud.com/shortcuts/01e2ed40f598489393769d9707c8c248

# 港澳台直播
https://www.icloud.com/shortcuts/653c5caaf997435788da1090e6b06406

# 唐人影视
https://www.icloud.com/shortcuts/022baf8d47bb449ab858bbcabcf408af

# 电视节目选播
https://www.icloud.com/shortcuts/dff26dbe75df442f8e82b61ba06963ac

# 翡翠台直播
https://www.icloud.com/shortcuts/be2e98653ffb467dbc54acb45105c1b4

# icdrama
https://www.icloud.com/shortcuts/362700ca27a04a9f9797d4b28c11b63a

# 小新在线影视
https://www.icloud.com/shortcuts/b77d3661dacd4651bcddb8cee863b1d1

# 全能音视频下载
https://www.icloud.com/shortcuts/b6f75b050fa84b49ae0ac8173fe976a7

# 抖音无水印视频
https://www.icloud.com/shortcuts/b7698896dc8a4057b8174e989ec21198
https://www.icloud.com/shortcuts/fa885e093ec443b2807040f2b3ad2bfd

# 抖音音频下载
https://www.icloud.com/shortcuts/e00730220f614ad193d69b5a75313dc3

# 快手无水印下载
https://www.icloud.com/shortcuts/b5221374c6024181ae19f7f759575035
https://www.icloud.com/shortcuts/118a2c455bb54406979071a8942c556d

# 万能视频下载
https://www.icloud.com/shortcuts/939dfd29e1d14800a00bc86a252ca84f

# 电影动漫资源搜索
https://www.icloud.com/shortcuts/e1e9401dcb4340bd96d463cf194ebbbf

# 全能视频VIP
https://www.icloud.com/shortcuts/4a39135fc0284377a1236cbbb0bac53c

# Viu TV99频道
https://www.icloud.com/shortcuts/2e78d444d706470e95b3112557bbd13b

# Youtube保存到任意App
https://workflow.is/workflows/01fdf6b4a30d43fcb1ae5f01763b4abc

# 贴吧视频下载
https://www.icloud.com/shortcuts/00975db818f846efb50df1489213b96e

# 火山短视频去水印下载
https://www.icloud.com/shortcuts/6cf84eae2eba4717ae75ae10cdd3f722

# 微博秒拍视频下载
https://www.icloud.com/shortcuts/e09a96b7e8bc4913bf8708f57cffce9f

# Twitter视频下载
https://www.icloud.com/shortcuts/0a9451a9f19a4a5c89afed57ef6fd9d9
https://workflow.is/workflows/75562e41b6c74540903786ae9b6f1c50

# 全网VIP视频解析
https://www.icloud.com/shortcuts/8a10557abf924d5dbdaaef5e65248593
https://www.icloud.com/shortcuts/58add5dba526413c862ac90d7cada939

# 照片视频-GIF互转
https://www.icloud.com/shortcuts/6041338fea174bacb386b7b14446fa6f
https://www.icloud.com/shortcuts/114d8f7e8fde44408c502723187f6a6e

# 从视频提取音频
https://www.icloud.com/shortcuts/5dd917b032e14d838554ea838dd6f978

# QQ空间视频下载
https://www.icloud.com/shortcuts/da27ac884dc04dd0accb75eb19e27ccc

# 获取B站封面视频
https://www.icloud.com/shortcuts/7d20ebb35d6441fcb9e0f0f6f225cbb41

# bilibili视频下载
https://www.icloud.com/shortcuts/7cfaa2d46b624e5093f9e13bded09ffc
```

## 词典

```
# 词典聚合
https://www.icloud.com/shortcuts/b40fa4ed278846e7b860674066b22c72

# 成语词典
https://www.icloud.com/shortcuts/5e8af02dd0c04703a65f5d0285d36888

# 新华字典
https://www.icloud.com/shortcuts/2a998ba59748437f8e8dd680c6fd67d3
```

## 剪贴板

```
# 复制到剪贴板
https://workflow.is/workflows/3242da310c4a45ec8270a09e49e6aa37

# 剪贴板翻译
https://workflow.is/workflows/5de0062c54c041a386be048dd84fa361

# 剪切板列表
https://workflow.is/workflows/db3321f2eec84c4eb2caf29a6df81286
```

## 搜索

```
# 自定义搜索
https://workflow.is/workflows/ac4b4abac0a2490e872b7134f5f9ca8a

# 谷歌高级搜索
https://workflow.is/workflows/6e7e0853ae664c5ea7a0bae7dad6941e

# 搜索电话号码
https://workflow.is/workflows/aa509941b57d48d4b51a0c2b1c06cf8c

# 你不会自己百度么
https://www.icloud.com/shortcuts/0b23911b67b341188f663c26ce737440

# 快捷百度搜索
https://www.icloud.com/shortcuts/030b88dff7bf40eba72fb9e2afb63dfb

# 小草搜索
https://www.icloud.com/shortcuts/dbec7cf292eb4601a7c9b35c51884146
```


## 天气

```
# 墨迹天气
https://www.icloud.com/shortcuts/f038cb7c58934a94aca2b4610ee5f447

# 墨迹天气实景图
https://www.icloud.com/shortcuts/1ba7cca9510f4aa7910f6c2bf29e06f9

# 彩云天气
https://www.icloud.com/shortcuts/09661308cc0b455e96c5ee660d9946e3

# 天气预报
https://www.icloud.com/shortcuts/274df58cd5354f6eb9630da30fdfba3e

# 报时语音天气
https://www.icloud.com/shortcuts/298247c77d364f7b828c18cb2e05b9fe

# 带背景音乐天气预报
https://www.icloud.com/shortcuts/31149463f4ca4df68148813331ea9603

# 今天的天气
https://workflow.is/workflows/cf6db9a3c90c4bc7adec27f739480937
```


## 社交

```
# Instagram全类型下载
https://www.icloud.com/shortcuts/7f4f267f7b6f4f3299206d43cde5c4eb
https://workflow.is/workflows/3a4f22ce8e794c7489a059224f3bc0b0

# Instagram保存图片
https://workflow.is/workflows/73aa53dfae9c45729befb49cdc6863f0

# Tumblr解析下载
https://www.icloud.com/shortcuts/400c06379b014e08be927162689f6047
https://www.icloud.com/shortcuts/6ac1b99b9e8340799d8b0c737cd0f314

# Tumblr保存到相册
https://workflow.is/workflows/897924d6858d47a4a6737733517a36eb

# 微博热搜榜
https://www.icloud.com/shortcuts/13c9b1f7cdbc434bbec2b28e577746ce

# 新浪微博
https://www.icloud.com/shortcuts/9994a3fdc685479d9b94fb6445a30536

# tumblr展开网址
https://workflow.is/workflows/62a9534d3ee1467080cd4136578d3d35

# 即刻视频下载
https://www.icloud.com/shortcuts/d6637e8536254a46ba81a09fabbad8f9

# 即刻图片下载
https://www.icloud.com/shortcuts/0f74a2c6ffbd4a59856612725ca22dc6
```

## 资源搜索与下载

```
# JS直链下载
# 自定义JS直链下载列表，可选择下载全部或挑选部分进行下载，理论上可以下载任何文本文件，如conf、list、md等
# 下载文件保存在iCoud Drive/shortcuts/elecV2JS文件夹，每次运行都会清空该目录的旧文件
https://www.icloud.com/shortcuts/50d7d8576dd74f7685811502ff72c078

# 高清影视种子下载
https://www.icloud.com/shortcuts/c5672516fa5645918abc48577a30efd6

# 迅雷安装器
https://www.icloud.com/shortcuts/e7c0ad75a9bf46bf889ba4be5463a7de

# 新磁力搜索
https://www.icloud.com/shortcuts/9a68ed9c6ea54654a4e7b8918b426855

# 小新下载器
https://www.icloud.com/shortcuts/90d5942429f14f5095eb7a0664d7918b

# 新磁力搜索
https://www.icloud.com/shortcuts/9a68ed9c6ea54654a4e7b8918b426855
```

## 电商

```
# 京东签到
# 下载stream并打开抓包，然后在Safari输入https://bean.m.jd.com并登录京东账号
# 刷新网页后返回stream并停止抓包。点击抓包历史，找到带有plogin开头的链接并点击，再选择请求，找到自己的cookie并复制。安装以下捷径后运行，输入刚刚复制的cookie，即可完成一键签到
https://www.icloud.com/shortcuts/35be01753adc49409a334464cb4c7470

# 双十一淘宝自动养猫
https://www.icloud.com/shortcuts/db359887fea14a61b0f0cd7c43642e4f

# 超级抢购助手
https://www.icloud.com/shortcuts/dca53a0ff3704f22badb7009b828b495

# 淘宝语音搜索
https://www.icloud.com/shortcuts/ba79d6ae5a1f455eb296cc314cf82447

# 淘宝语音搜索
https://www.icloud.com/shortcuts/ba79d6ae5a1f455eb296cc314cf82447

# 网购历史价格查询
https://www.icloud.com/shortcuts/e2c2777962384ffbbff3f04f2874ef67
```

## 电子支付

```
# 支付宝添加信用卡
https://www.icloud.com/shortcuts/4aef0930744d461c92333e1afaa9accf

# 三合一收款二维码生成
https://www.icloud.com/shortcuts/319f80622c234de8b811b7ba2d5a7228

# 支付宝打赏
https://www.icloud.com/shortcuts/bfe71390fe6a4d178d1f3f1a375765b7

# 支付助手
# 扫一扫、微信扫码、微信收款、支付宝扫码、Apple Pay、AA付款、查快递、蚂蚁森林、蚂蚁庄园、彩票、股票、运动、淘票票、乘车码、生活缴费、火车票等
https://www.icloud.com/shortcuts/45c64b77d773411485eb1c697f36cd8c

# 支付宝红包
https://www.icloud.com/shortcuts/abeb70e152f94a27871bb93abcc5475f
```

## 翻墙

```
# 批量节点获取
https://www.icloud.com/shortcuts/69caa3c45c7e48b5b6f3da98cdaeeac7

# 更新surge/shadowrocket主配置
https://workflow.is/workflows/55ecb14722f94d7c8c44ce3ce24d81c8

# 小火箭
https://www.icloud.com/shortcuts/8e3d0a5482004b8ca0f7c65301fdc936
```

## APP

```
# App Store商店切换
https://www.icloud.com/shortcuts/2910a1fd97404f149509cfd5612a046d
https://www.icloud.com/shortcuts/015c63d4358142389f1c723b7475406d

# App Store Region
https://www.icloud.com/shortcuts/5bb33439f8544ea4b9e708dd1b7a81ec

# 限免软件
https://www.icloud.com/shortcuts/0e5e70111c2f49a9bc1f10629c6ca74e

# 苹果应用安装器
https://www.icloud.com/shortcuts/0e2e25d73a7b4af0b6579158cf336dde

# 查看app信息
https://workflow.is/workflows/14f9deb92d4e42c4b286571b40533061
```

## 短链接与二维码

```
# 长短链接转换
https://www.icloud.com/shortcuts/22a07d8aeff24a4d9f977e1c9d5d9d0f

# iCloud外链获取器
https://www.icloud.com/shortcuts/c45f40eca0ba4a06a48fb99da71df047

# 短链接生成器
https://www.icloud.com/shortcuts/0c2ff4b4193b42779c192818c1a1e492

# 二维码生成器
https://www.icloud.com/shortcuts/1a8fb9d0760e40ff94ae374d2cdff497

# 制作/扫描二维码
https://www.icloud.com/shortcuts/52490640004c4fc4b2ebfd038eaf651c
https://workflow.is/workflows/3ed9b9ec4cf3463a9df0af7fca642e0b

# 二维码识别器
https://www.icloud.com/shortcuts/ff1992922a504d8797821da55f384b67

# 短链接
https://www.icloud.com/shortcuts/23ea80da4460403082b33c82bb41b5ad

# Shorten Link
# 一款短链接聚合工具，支持国内外优质短链接平台获取
https://www.icloud.com/shortcuts/e34d06d582884b19a32e4ecf5b9660e1

# WIFI二维码
https://www.icloud.com/shortcuts/59b1018835d94018ba6097ff0462cda9

# 万能扫码
https://www.icloud.com/shortcuts/e247aa760193472db8588804a8ea76d0

# 长短链接转换
https://www.icloud.com/shortcuts/22a07d8aeff24a4d9f977e1c9d5d9d0f
```

## 备份

```
# 一键备份所有捷径
https://www.icloud.com/shortcuts/6fbf11bce41b418396bf60ffaa5c14f3

# AB Backup备份
https://workflow.is/workflows/3db6bdcda3104b3b9bbaea8ddf655670

# 备份workflow工作流
https://workflow.is/workflows/c5421ebe19bd4bf885da8b74d2cbaded
```

## 工具箱

```
# 苹果工具箱
https://www.icloud.com/shortcuts/a7228c50afd94f2dbab9d160a8b5d50d

# 小新工具箱
https://www.icloud.com/shortcuts/dad33a5b449f45ee98c60d528d1fd878
```

## 翻译

```
# 英译中Pro
https://www.icloud.com/shortcuts/22805b54bdec4951bb36162e1f93c9bc

# 网页翻译
https://www.icloud.com/shortcuts/4c8839065dc44dda9ca22ecfc329cb18

# 翻译
https://workflow.is/workflows/a51df78f80414edf8469727f892d662d
```

## OCR与文字语音转换

```
# 百度语音合成
https://www.icloud.com/shortcuts/c051b5e6cab94d70926e3b9bbb29af36

# 文本转语音播放
https://www.icloud.com/shortcuts/5f95555da1454ce3a8a99f3f689410ee

# 图片文字识别
https://www.icloud.com/shortcuts/8aec6f5f73a54ecbab895bc468c60535
```

## 系统

```
# 群发短信
https://www.icloud.com/shortcuts/587fb88a053f4a76a2a0594c0ca299f1

# 查询未知号码来电信息
https://workflow.is/workflows/95b02bb18b2e4cfba06919034929a4d9

# 信息照片导航
https://workflow.is/workflows/84daad7aa32f488f842356a246087a5b

# 手机快捷设置
https://workflow.is/workflows/f1e96f0ed0494d3288dc69c9083deab7

# 信息翻译发送
https://workflow.is/workflows/f1d0b3a07ecc47ecbb963651740f7b74

# 提醒事项助手
https://www.icloud.com/shortcuts/38a51eafd83440dc9dcbe401d86c8706

# 系统开关设置
https://www.icloud.com/shortcuts/253fac20d197429292313f3bf3f104f3

# 通知中心翻页
https://workflow.is/workflows/7afbf6fa09204e5e837d52cab41f0497

# 换铃声
https://www.icloud.com/shortcuts/9928bcde86fe45ada142fac6c82e6441

# WIFI开关
https://www.icloud.com/shortcuts/b63662901aa54a37bf791a9b9a638c3d

# 一键重启
https://www.icloud.com/shortcuts/02fb4b468e534b569b14b22d9d0fd8bd

# 低功耗模式
https://workflow.is/workflows/640ae7eb22f143a093c77215a6e373e2

# 电池
https://workflow.is/workflows/febeed3ed07a4c78b9e77c2f48136a2a
```

## 签到

```
# 全网
https://www.icloud.com/shortcuts/91af4b2564134654ad2dbc7bce54bbdf

# 贴吧
https://www.icloud.com/shortcuts/5fd43b4ed6664fa4bcbf85bd2c18a408

# 淘宝
https://www.icloud.com/shortcuts/d7286cd634544d0ab8ecaf4bd7ad376d
```

## 其它

```
# 网速测试
https://www.icloud.com/shortcuts/0e5a81cb1076414680fd4f1a2d0c9094

# 谷歌网盘转存助手
https://www.icloud.com/shortcuts/51cdfaff6fd44ac295a8a34bd67e3da8

# 健康码
https://www.icloud.com/shortcuts/7b3a4c33ef374b7f8ef7eddcf4867234

# 分享当前Wi-Fi
https://www.icloud.com/shortcuts/926073374aa9498999756b17fadd9d28

# 网盘搜
https://www.icloud.com/shortcuts/feb916abb943481caa6a61335c2b224b

# 抽奖工具
https://www.icloud.com/shortcuts/9dc04fa400f248ac9608e6d138a07de3

# 人脸识别
https://www.icloud.com/shortcuts/67175ef09c1d4e7da2d77d476506740f

# 网络工具
https://www.icloud.com/shortcuts/74b26667ec774e6fbbfbe97c8aeff4e9

# 保存到iCloud
https://workflow.is/workflows/08f3cc0e184844339eb2f5caa58a316b

# 低消耗
https://www.icloud.com/shortcuts/2d1a0e792358405fbb2462572962e6cd

# 共享单车
https://www.icloud.com/shortcuts/259e77b79b884b0eb3b554d15672965f

# 数羊
https://www.icloud.com/shortcuts/9a9967b93caf4942b109d2efd59abbb3

# 手机亮度调节
https://www.icloud.com/shortcuts/1a1c0b2192d64550a4352eec7b8a1bfa

# 压缩/解压
https://www.icloud.com/shortcuts/4a352c743eae47f7bda758429109773c

# 程序改名加锁
https://www.icloud.com/shortcuts/0c73f53b09a24ad88f9eeeb315794b81

# 闪瞎你的眼
https://www.icloud.com/shortcuts/225f35dab5c84a96b101bd6e3b4d3d1a

# 修改步数
# QQ、微信、支付宝步数不能改变
https://www.icloud.com/shortcuts/cc759611240742cfb48e9cb83fafb806
# 以下捷径可用于微信、支付宝。需要下载小米运动APP，然后下载捷径后将捷径中的配置修改为自己的账号和密码。
https://www.icloud.com/shortcuts/ddc53a9e14d8415387a36a4cc46e3ad2

# 附近免费WIFI
https://www.icloud.com/shortcuts/81c787aacc9d41c4a33410426f43ed4b

# 在线福利6.1
https://www.icloud.com/shortcuts/7bf3abd752fe4f62a74a6c3a0d08e4a4

# 早上好
https://www.icloud.com/shortcuts/2e673487577c4a45af9047e83ab276eb

# 晚安
https://www.icloud.com/shortcuts/de7f95054f414776b0aa93c91bd06604

# 历史上的今天
https://www.icloud.com/shortcuts/eedc97baa8ec4c929312154af9a42ba3

# 保存网页为pdf到ibook
https://workflow.is/workflows/2027271452ce4712853838f43550d326

# 任天堂红白机小游戏
https://www.icloud.com/shortcuts/61facc44bd0f4f16b7c13468cf08a4e3

# 实时公交
https://www.icloud.com/shortcuts/aff4b5e0770b4a2cae6dae18249434d1

# 油价语音播报
https://www.icloud.com/shortcuts/a187659e1a0a4ab585eb72a764e3fed7

# 仙女棒
https://www.icloud.com/shortcuts/e3a0675319994f6d87b16c7ea1b73c5c

# 彩票开奖结果
https://www.icloud.com/shortcuts/ebc6541a714b42fb8cb4247d2b71ab77

# 钉钉打卡
https://www.icloud.com/shortcuts/7749df7b9c3342539b96d9aead7267ea

# 图灵机器人
https://www.icloud.com/shortcuts/1ae4ce4344b54f15b8a9831e0ab81bb4

# 运动播报
https://www.icloud.com/shortcuts/bc9e69c607ef40d08933aac0cd20588a

# 知乎日报
https://www.icloud.com/shortcuts/6f60912434e24f64b645d894ad78c47b

# 百度知道日报
https://www.icloud.com/shortcuts/df85acb1585e481fa8d9de965743b046

# 近期上映电影
https://www.icloud.com/shortcuts/32999ca5bdce4a879ba48e5890d1b257

# 程序改名加锁
https://www.icloud.com/shortcuts/0c73f53b09a24ad88f9eeeb315794b81

# 老司机
https://www.icloud.com/shortcuts/baaeae0d7d164472baed3e531673733b

# 空气质量查询
# 语音、自动定位、手动输入
https://www.icloud.com/shortcuts/de1d387855b24990a0b6648c2eba3a5f

# 追书神器
https://www.icloud.com/shortcuts/ec6c8f83043a4fb3885c4a302d7c2129

# 骂人宝典
https://www.icloud.com/shortcuts/73ded407f0d7417899346a3b5aa6d8ff

# 人脸识别
https://www.icloud.com/shortcuts/67175ef09c1d4e7da2d77d476506740f

# 睡眠声音
https://www.icloud.com/shortcuts/c9c50392fb7f4603af03cb9caf0fa9cb

# 一言
https://www.icloud.com/shortcuts/cd37638ddd02421c966be16961db558f
https://www.icloud.com/shortcuts/7f21e942496c46e9a7a98338d311a07a

# 自定义勿扰时长
https://www.icloud.com/shortcuts/92706b85621b42d2aaaf7cf296c749e5

# 低消耗
https://www.icloud.com/shortcuts/2d1a0e792358405fbb2462572962e6cd

# 高德地图语音导航
https://www.icloud.com/shortcuts/28b98b7fd9b84301955b72345e8515b1

# 查快递
https://www.icloud.com/shortcuts/04a23ce1b0b64c998519341752811e7e

# Picsew Editor
https://www.icloud.com/shortcuts/fc50347ce3f047309001520ceecb12ee

# 白描助手
https://www.icloud.com/shortcuts/20988a652e264b13889aa5db1ac3dbff

# 多种外币转换
https://workflow.is/workflows/9570db942bfb45f18581bd36121573b0

# 快递物件查询
https://workflow.is/workflows/177a3fd258ab4cfd806ff99df15d867c

# 打开链接并复制密码
https://workflow.is/workflows/e5846d5f70db404a914db7c6ae07e6c8

# 货币换算
https://workflow.is/workflows/50f0132cbe654370bc35189e1cf31b86

# 抛硬币
https://workflow.is/workflows/4e37faab34864e1883c533754c788066

# 合并PDF文件
https://workflow.is/workflows/adbb5f464fba4bc68a42a31bf91d1c4c

# 扩展和共享网址
https://workflow.is/workflows/acad8115a5564d6ea336b70b87dcab72

# 计时器
https://workflow.is/workflows/93fd0318d0e14cdaa32c5d021effaa8b

# IP和位置
https://workflow.is/workflows/ea529c591a2740169388c0d2b8ca6505

# 插接板书签离线
https://workflow.is/workflows/78e3d1afef39487db3d4a96d55e9a131

# 书签
https://workflow.is/workflows/86a36fe10ac54a0cbe21c0016e762dee

# Biking Distance
https://workflow.is/workflows/90b54056340742f2bd24ed45f894d22e

# 旅行距离
https://workflow.is/workflows/5e86637a392147449e7f0aebb3b18cf4

# 莫尔斯电码编码器手电筒
https://workflow.is/workflows/3f723d2d0e0a4c45b920281cf7d30284

# 手电筒
https://workflow.is/workflows/31699d9bf6054e028c69286cdf932ce6

# Translate
https://www.icloud.com/shortcuts/5ce941f97e7644e1951c3aa3cacf4804

# 极搜
https://www.icloud.com/shortcuts/9016687bcc894015ba2c7bbf3766359c

# 文本分析
https://www.icloud.com/shortcuts/458e944a6e8b409695e069f92b5e7bc0

# iiilab解析器
https://www.icloud.com/shortcuts/dbfdd71fdfd540ada81c08f8d24280b4

# Parse Video X
https://www.icloud.com/shortcuts/26224a223a344c0490ef1ecf91ee4ebc

# 网页内容编辑
https://www.icloud.com/shortcuts/c355a99f35174f488ed53e9499034e8f
```

# 自动化

可在指定事件发生时执行动作。自动化可以搭配快捷指令使用，如指定每天九点运行淘宝自动签到快捷指令。

# 快速调用捷径

可通过辅助触控实现，在辅助触控中将捷径设置为其中一个图标即可。

也可通过轻点背面的功能，选择轻点背面时调用捷径。

# 参考教程

## LEECO - 捷径作品

```
https://shimo.im/docs/DHcYv8xjcYxvHvQg/read
```

## 快捷指令｜运动刷榜，支持微信，支付宝等！

```
https://mp.weixin.qq.com/s/6E5monX6gp0bMc0J4U_mCQ
```

## Workflow规则｜小新制作

```
https://www.limufang.com/post/526.html
```

## 捷径制作教学

```
https://mp.weixin.qq.com/s/iGzhb1G3YY3p5UEgxeoVJg
https://mp.weixin.qq.com/s/OMTyQ_LAR6zaRQbz_sog5g
```


