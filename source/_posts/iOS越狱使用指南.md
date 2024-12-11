---
title: iOS越狱使用指南
categories: iOS
abbrlink: iOS-Jailbreak-Usage
date: 2023-11-10 11:33:29
tags:

---

![](topic.jpg)

iOS越狱指南。

<!-- more -->

# TODOS

```
https://ios.cfw.guide/using-palen1x/


https://www.reddit.com/r/jailbreak/comments/1tmvh7/question_what_exactly_is_bootstrap/
https://onejailbreak.com/blog/iapstore-tweak/
https://leavez.xyz/2017/02/12/mario-tweak/
https://www.bilibili.com/read/cv12153485/
```

# 应用商店

应用商店可在Cydia/Sileo通过插件的方式安装。

## Zebra

作者源如下。

```
https://getzbra.com/repo/
https://getzbra.com/beta/
```

## Installer

作者源如下。

```
https://apptapp.me/repo/
```

## AltStore

添加以下源并按顺序安装AltDaemon、AppSync Unified、appinst (App Installer)、AltStore (ALPHA)即可。

```
https://pwnders.github.io/repo/
https://cydia.akemi.ai/
```

# 补丁软件

补丁软件可以修改APP，达到破解效果。

## Flex 3

### 安装和破解

源地址如下。

```
https://getdelta.co/
```

登录以下账号即可永久使用会员。

```
账号 / sundasheng521@qq.com
密码 / 7758521a
```

### 手动添加补丁

打开Filza，进入路径/var/mobile/Library/Application Support/Flex3，使用文本编辑器打开patches.plist文件，拉到最下方，在末尾的`<dict>`后复制要添加的补丁代码，保存即可。

### 补丁制作

下载FlexTool插件，在设置中选择要修改的APP。打开后可通过FlexTool定位到需要修改的函数位置。打开Flex，点击+号，选择要修改的APP，点击Add Units，点击APP名称以进行初次编译。编译完成后搜索刚才找到的函数，勾选并返回到补丁主界面。点击刚才选择的函数，修改其返回值即可。

### 补丁列表

#### 支付宝十万步数

```
<dict>
    <key>UUID</key>
    <string>0A3D50C5-F2C6-4933-B997-C1647F3E109A</string>
    <key>apiVersion</key>
    <integer>3</integer>
    <key>appIdentifier</key>
    <string>com.alipay.iphoneclient</string>
    <key>author</key>
    <string>hyczby5218</string>
    <key>cloudDescription</key>
    <string>支付宝修改步数</string>
    <key>cloudID</key>
    <integer>46823</integer>
    <key>downloadDate</key>
    <integer>1551604711</integer>
    <key>name</key>
    <string>支付宝修改步数</string>
    <key>switchedOn</key>
    <true/>
    <key>units</key>
    <array>
        <dict>
            <key>methodObjc</key>
            <dict>
                <key>className</key>
                <string>APStepInfo</string>
                <key>displayName</key>
                <string>-(long long) numberOfSteps</string>
                <key>prefix</key>
                <string>-</string>
                <key>selector</key>
                <string>numberOfSteps</string>
                <key>typeEncoding</key>
                <string>q16@0:8</string>
            </dict>
            <key>name</key>
            <string>修改步数</string>
            <key>overrides</key>
            <array>
                <dict>
                    <key>argument</key>
                    <integer>0</integer>
                    <key>type</key>
                    <dict>
                        <key>subtype</key>
                        <integer>0</integer>
                        <key>type</key>
                        <integer>9</integer>
                    </dict>
                    <key>value</key>
                    <dict>
                        <key>type</key>
                        <integer>9</integer>
                        <key>value</key>
                        <integer>100000</integer>
                    </dict>
                </dict>
            </array>
        </dict>
    </array>
</dict>
```

#### Thor移除验证设备验证

```
<dict>
    <key>UUID</key>
    <string>AE872A89-5EB7-483D-B50A-88C29F6E20C9</string>
    <key>apiVersion</key>
    <integer>3</integer>
    <key>appIdentifier</key>
    <string>com.pixelcyber.dake.thor</string>
    <key>author</key>
    <string>zjh521901</string>
    <key>cloudDescription</key>
    <string>thor移除验证设备验证补丁</string>
    <key>cloudID</key>
    <integer>46126</integer>
    <key>downloadDate</key>
    <integer>1552100989</integer>
    <key>name</key>
    <string>Thor ·Fz夜</string>
    <key>switchedOn</key>
    <false/>
    <key>units</key>
    <array>
        <dict>
            <key>methodObjc</key>
            <dict>
                <key>className</key>
                <string>AppReceiptValidator</string>
                <key>displayName</key>
                <string>-(void) prepareVerify</string>
                <key>prefix</key>
                <string>-</string>
                <key>selector</key>
                <string>prepareVerify</string>
                <key>typeEncoding</key>
                <string>v16@0:8</string>
            </dict>
            <key>name</key>
            <string>🈲贩卖🈲谋利🈲</string>
            <key>overrides</key>
            <array/>
        </dict>
    </array>
</dict>
```

## SuperCharge

SuperCharge脚本的后缀名为st。将st脚本发送到手机，并用SuperCharge打开即可。源地址如下。

```
https://repo.supercharge.app/
```

# 游戏修改

## 必要插件

一般需要IGG和Filza。

IGG全称为iGameGuardian，用于修改游戏。安装用嘻哈源即可，官方源如下。

```
https://aquawu.github.io/igg/
```

进入插件配置，打开使用储存空间，下限设为0x00000000，上限设为0x150000000，并打开移除未匹配项，打开有号数，完成配置。

## 王者荣耀

### 修改快捷信息/语音库

在语音库中清除任意数量的信息（比如4个），然后用`小心草丛`填入，不要点确定。切到IGG，选择smoba，I32搜索1。

切回王者荣耀，删除4个`小心草丛`，选择4个`敌人消失`填入，不要点确定，再次切到IGG，I32搜索2。

切回王者荣耀，删除4个`敌人消失`，选择4个`回防高地`填入，不要点确定，再次切到IGG，I32搜索3。

切回王者荣耀，删除4个`回防高地`，选择4个`请求支援`填入，不要点确定，再次切到IGG，I32搜索4。

切回王者荣耀，删除4个`请求支援`，分别选择`小心草丛`、`敌人消失`、`回防高地`、`请求支援`，不要点确定。再次切到IGG，上次得到的4-5个结果分别变成了1、2、3、4、5，把这几个数字换成所需要的消息，点确定即可。

详细的快捷消息代码如下。

| 代码 | 含义           |
| ---- | -------------- |
| 1    | 小心草丛       |
| 2    | 敌人消失       |
| 3    | 回防高地       |
| 4    | 请求支援       |
| 5    | 猥琐发育，别浪 |
| 6    | 注意野区       |
| 7    | 保护我方输出   |
| 8    | 小心对方偷塔   |
| 9    | 稳住，我们能赢 |
| 10   | 大家打开语音   |
| 11   | 跟着我         |
| 12   | 注意分散站位   |
| 13   | 注意保护后排   |
| 14   | 等等我，马上到 |
| 15   | 技能不全，撤退 |
| 16   | 状态不好，撤退 |
| 17   | 撤退，别打主宰 |
| 18   | 撤退，别打暴君 |
| 19   | 我回防高地     |
| 20   | 集合埋伏       |
| 21   | 清理兵线       |
| 22   | 我拿buff，谢谢 |
| 23   | 集合准备团战   |
| 24   | 拖住，我偷塔   |
| 25   | 集合进攻暴君   |
| 26   | 集合进攻主宰   |
| 27   | 全体进攻对抗路 |
| 28   | 全体进攻中路   |
| 29   | 全体进攻发育路 |
| 30   | 优先攻击输出   |
| 31   | 优先攻击突进   |
| 32   | 优先推塔       |
| 33   | 准备越塔强攻   |
| 34   | 正在前往支援   |
| 35   | 上去开团       |
| 36   | 等我集合打团   |
| 37   | 我来抓人了     |
| 38   | 大招已经好了   |
| 39   | 我去抓对抗路   |
| 40   | 我去抓中路     |
| 41   | 我去抓法语撸   |
| 42   | 干得漂亮       |
| 43   | 让我独享经验   |
| 44   | [全部]抱歉     |
| 45   | [全部]谢谢你   |
| 46   | [全部]挑衅     |
| 47   | 我拿BUFF再团   |
| 48   | 别团，等人齐   |
| 49   | 收到           |
| 50   | 多观察小地图   |
| 51   | 团不过先发育   |
| 52   | 清完兵线再团   |
| 53   | 继续推我回防   |
| 54   | 当心敌方陷阱   |
| 55   | I'll carry you |
| 56   | 集合反蓝       |
| 57   | 集合反红       |
| 58   | 集合入侵野区   |
| 59   | 帮我看红，谢谢 |
| 60   | 帮我看蓝，谢谢 |
| 61   | 我单带，别接团 |
| 62   | 经济落后，别团 |
| 63   | 兵线没到，撤退 |
| 64   | 敌人偷塔，撤退 |
| 65   | 打团了，别单带 |
| 66   | 等我复活在团   |
| 67   | 注意敌方绕后   |
| 68   | 法师来拿蓝     |
| 69   | 射手来拿红     |
| 70   | 求让BUFF，谢谢 |
| 71   | 交个朋友       |
| 72   | 新年快乐       |
| 73   | 集合进攻龙王   |
| 74   | 撤退，别打龙王 |
| 79   | 阵营-长城      |
| 80   | 阵营-玄雍      |
| 81   | 阵营-稷下      |
| 86   | 裂开吧！敌人   |
| 91   | 打团请保护我   |
| 92   | 兄弟，我来了   |
| 93   | 别急，有我     |
| 94   | 大佬，救命     |

### 修改超长名字

老账号使用改名卡，在输入框不要输入东西。新账号则在创建昵称界面不要输入文字。

打开IGG并选择smoba，设置临近范围为0x9，然后i32搜索12。打开联合，i32搜索86400，这时只有一个结果。

把12改成64，然后清除。再次打开设置，设置临近范围为0x21，然后i32搜索42。打开联合，i32搜索12，把12改成88。

此时回到游戏界面修改即可，最长21个汉字或61个字母。

### 设置三个相同的想玩英雄

在排位赛页面清除所有自定义英雄，选择第一个英雄（如赵云）。打开IGG并选择smoba，然后I32搜索107（赵云）。

去掉赵云，再次选择一个英雄（如小乔）。打开IGG，I32搜索106（小乔），此时结果只剩下一个。

点击内存，再点击右上角移动，此时会跳转到刚才的结果，且底部的两个数值为0。返回王者荣耀，随便选择三个英雄，再打开IGG，此时三个数字均发生变化。

修改该三个数字为所需要的英雄代码。返回王者荣耀，点击确定，出现三个一样的头像即可。

头像代码如下。

| 代码 | 含义                 |
| ---- | -------------------- |
| 105  | 廉颇                 |
| 106  | 小乔                 |
| 107  | 赵云                 |
| 108  | 墨子                 |
| 109  | 妲己                 |
| 110  | 嬴政                 |
| 111  | 孙尚香               |
| 112  | 鲁班七号             |
| 113  | 庄周                 |
| 114  | 刘婵                 |
| 115  | 高渐离               |
| 116  | 荆轲                 |
| 117  | 钟无艳               |
| 118  | 孙斌                 |
| 119  | 扁鹊                 |
| 120  | 白起                 |
| 121  | 芈月                 |
| 122  | 诸葛亮（旧）         |
| 123  | 吕布                 |
| 124  | 周瑜                 |
| 125  | 庞统                 |
| 126  | 夏侯淳               |
| 127  | 甄姬                 |
| 128  | 曹操                 |
| 129  | 典韦                 |
| 130  | 宫本武藏             |
| 131  | 李白                 |
| 132  | 马可波罗（旧）       |
| 133  | 狄仁杰               |
| 134  | 达摩                 |
| 135  | 项羽                 |
| 136  | 武则天               |
| 137  | 司马懿               |
| 138  | 孔夫子               |
| 140  | 关羽                 |
| 141  | 貂蝉                 |
| 142  | 安琪拉               |
| 143  | 安绿山               |
| 144  | 程咬金               |
| 146  | 露娜                 |
| 148  | 姜子牙               |
| 149  | 刘邦                 |
| 150  | 韩信                 |
| 152  | 王昭君               |
| 153  | 兰陵王               |
| 154  | 花木兰               |
| 155  | 艾琳                 |
| 156  | 张亮（旧）           |
| 157  | 不知火舞             |
| 158  | 八神庵               |
| 162  | 娜可露露             |
| 163  | 橘右京               |
| 166  | 亚瑟                 |
| 167  | 孙悟空               |
| 168  | 牛头                 |
| 169  | 后裔                 |
| 170  | 刘备                 |
| 171  | 张飞                 |
| 173  | 李元芳               |
| 174  | 虞姬                 |
| 175  | 钟馗                 |
| 176  | 杨玉环               |
| 177  | 成吉思汗             |
| 178  | 杨戬                 |
| 179  | 女娲                 |
| 180  | 哪吒                 |
| 182  | 干将（旧）           |
| 183  | 雅典娜               |
| 184  | 蔡文姬               |
| 186  | 太乙真人             |
| 187  | 东皇太一             |
| 189  | 鬼谷子               |
| 190  | 诸葛亮               |
| 191  | 大乔                 |
| 192  | 黄忠                 |
| 193  | 凯                   |
| 194  | 苏烈                 |
| 195  | 百里玄策             |
| 196  | 百里守约             |
| 197  | 异星                 |
| 198  | 梦琪                 |
| 199  | 公孙离               |
| 207  | 封网                 |
| 222  | 徐福                 |
| 225  | 庞统（傀儡）         |
| 237  | 司马懿               |
| 240  | 觉醒关羽             |
| 241  | 金龙                 |
| 242  | 黑龙                 |
| 243  | 红龙                 |
| 244  | 绿龙                 |
| 254  | 花木兰（重剑）       |
| 305  | 廉颇（改版）         |
| 310  | 嬴政                 |
| 312  | 沈梦溪               |
| 320  | 宫本武藏（强化）     |
| 332  | 马可波罗             |
| 333  | 马可波罗（加强）     |
| 346  | 露娜（改版）         |
| 355  | 艾琳（改版）         |
| 356  | 张亮                 |
| 366  | 亚瑟（改版）         |
| 378  | 杨戬（改版）         |
| 382  | 干将                 |
| 501  | 明世隐               |
| 502  | 裴擒虎               |
| 503  | 狂铁                 |
| 504  | 米莱狄               |
| 505  | 瑶                   |
| 506  | 云中君               |
| 507  | 李信                 |
| 508  | 伽罗                 |
| 509  | 盾山                 |
| 510  | 孙策                 |
| 511  | 猪八戒               |
| 512  | 囚徒                 |
| 513  | 上官婉儿             |
| 515  | 嫦娥                 |
| 516  | 舜                   |
| 518  | 马超                 |
| 520  | 少司命               |
| 522  | 东方曜               |
| 523  | 西施                 |
| 524  | 蒙犽                 |
| 525  | 鲁班大师             |
| 526  | 王翦                 |
| 527  | 蒙恬                 |
| 528  | 吕蒙                 |
| 529  | 盘古                 |
| 530  | 宫本武藏（改版）     |
| 531  | 镜                   |
| 532  | 镜（分身）           |
| 533  | Zombie               |
| 620  | 韩信                 |
| 621  | 庄周                 |
| 630  | 宫本武藏（改版强化） |
| 654  | 母僵尸               |
| 668  | Zombie               |
| 675  | Zombie               |
| 687  | Zombie               |
| 700  | 坦克轮子             |
| 701  | 坦克炮管             |
| 732  | 马可波罗（旧）       |

NPC皮肤代码如下。

| 代码 | 含义             |
| ---- | ---------------- |
| 770  | 暴君             |
| 771  | 主宰（英雄）     |
| 772  | 暴君3vs3（英雄） |
| 773  | 年兽（英雄）     |
| 2025 | 暴君3vs3         |
| 2145 | 主宰             |

技能代码如下。

| 代码  | 含义     |
| ----- | -------- |
| 80101 | 监视     |
| 80102 | 治疗     |
| 80103 | 晕眩     |
| 80104 | 惩戒     |
| 80105 | 干扰     |
| 80106 | 隐遁     |
| 80107 | 净化     |
| 80108 | 斩杀     |
| 80109 | 疾跑     |
| 80110 | 狂暴     |
| 80111 | 振奋     |
| 80112 | 月之守护 |
| 80115 | 闪现     |
| 80116 | 寒冰惩戒 |
| 80117 | 传送     |
| 80121 | 弱化     |
| 80122 | PVE闪现  |
| 81102 | 坚定意志 |
| 81107 | 不灭信仰 |
| 81109 | 奔狼号令 |
| 81110 | 幽梦之息 |
| 81112 | 时光凝滞 |

地图代码如下。

| 代码  | 含义                   |
| ----- | ---------------------- |
| 4010  | 变身大乱斗（地图）     |
| 41150 | 抢鲲大乱斗（娱乐地图） |
| 20001 | 经典1v1（竞技地图）    |
| 20002 | 经典3v3（竞技地图）    |
| 20009 | 火焰山（娱乐地图）     |
| 20011 | 经典5v5（竞技地图）    |
| 20012 | 无限乱斗（娱乐地图）   |
| 20013 | 无限火力（娱乐地图）   |
| 20015 | 迷雾模式（娱乐地图）   |
| 20016 | 荣耀峡谷（竞技地图）   |
| 20017 | 无限乱斗（娱乐地图）   |
| 20018 | 实战训练（地图）       |
| 20028 | 五军对决（娱乐地图）   |
| 20040 | 实战模拟（快速地图）   |
| 20041 | 实战模拟（标准地图）   |
| 20071 | 抢鲲模式（娱乐地图）   |
| 20072 | 强化模式（娱乐地图）   |
| 20080 | 快跑模式（娱乐地图）   |
| 20099 | 无塔1v1（竞技地图）    |
| 20699 | 无塔3v1（竞技地图）    |
| 20999 | 无塔5v5（竞技地图）    |
| 20111 | 征兆模式（竞技地图）   |
| 20151 | CS模式（竞技地图）     |
| 21000 | 峡谷大乱斗（地图）     |
| 21001 | 峡谷大乱斗（地图）     |

攻速上限代码如下。

```
163840000；32768000
```

TS视角如下。

```
1500000-165000  修改167888
Online-基址FD38  Beta-基址FD68
154444-174444  修改160001
```

3D视角如下。

```
1097285734
```

上帝视角如下。

```
1,081,006,571;
-1，25，
-1,125,398;
-1,082,130,432;
-1,088,838,298::37
改
-1,082,130,32
```

### 体验隐藏角色

打开王者荣耀，进入单机模式，选择任意难度进入房间。任意选择英雄，此处以廉颇（105）为例。切到IGG，选择smoba，I32搜索105。

回到王者荣耀，选择小乔（106），切到IGG，I32搜索106。点右上角开关，再点左上角全改，此处测试将数值改成武则天（136）。回到王者荣耀，点击皮肤，变成武则天，开局。

退出本局游戏，再开一次房间，任意选个英雄，切到IGG，点全改，改为艾琳（155）或年兽（773）等其他特殊隐藏角色/NPC。回到王者荣耀，开局即可。

若读条中英雄图像是白色，并且游戏崩溃，则划掉后台，重新操作。如再次操作依然崩溃，说明该英雄资源不存在，不支持修改。

### 享受定位特权

下载王者人生APP并打开，可查找到能享受特权的店铺。以KFC为例，记住其位置。下载Wifi万能钥匙，查找该特权店对应的Wi-Fi名称，此处为KFC FREE WIFI。

安装Fake GPS Pro插件，进入Wifi信息修改，设置成KFC FREE WIFI，MAC地址可以任意。安装Anywhere插件，寻找到特权店铺的位置，扎标，然后点击上面跳出的地址推荐，再选择应用到王者荣耀和王者人生。进入王者荣耀，若出现王者特权提示，则成功。否则需要在Anywhere中微调位置直至出现弹窗为止。

或者可以通过修改经纬度的方式实现修改定位。打开flex3，添加王者荣耀，搜索privilegeManager，进入后再搜setuserlatitude longitude。勾选后返回，输入特权店的经纬度即可。修改Wi-Fi和MAC地址则同上。

### 修改荣耀战区

修改定位方法同上，修改定位完成后进入游戏即可修改战区。注意每周一才可修改。

### 修改技能效果

下列步骤必须在游戏中进行修改，且必须落地后在购买装备点击技能的时候进行修改。注意一局只能修改一次。

打开王者荣耀后，IGG进入smoba，I32搜索`英雄代码+00`，如搜索墨子则为10800。打开联合开关，搜索100，关闭联合开关，搜索`英雄代码+00`，全部改0即可。

常见代码及对应技能如下。

| 代码 | 英雄名称 |        技能效果        |
|------|----------|------------------------|
|  108 | 墨子     | 无限击退（被动）       |
|  112 | 鲁班     | 无限biubiubiu（被动）  |
|  114 | 刘禅     | 无限锤（一技能）       |
|  129 | 典韦     | 无限乱舞（一技能）     |
|  133 | 狄仁杰   | 无限卡牌（强化被动）   |
|  134 | 达摩     | 无限回血（强化被动）   |
|  146 | 露娜     | 无限标记（强化被动）   |
|  149 | 刘邦     | 无限剑气               |
|  150 | 韩信     | 无限挑飞               |
|  152 | 王昭君   | 无限下雨               |
|  162 | 娜可露露 | 强化普攻               |
|  163 | 橘右京   | 无限拔刀斩（被动）     |
|  166 | 亚瑟     | 无限沉默（被动）       |
|  167 | 孙悟空   | 无限棍子（被动）       |
|  170 | 刘备     | 无限两次弹射           |
|  194 | 苏烈     | 无限击飞               |
|  198 | 梦琪     | 无限三爪击             |
|  305 | 廉颇     | 强化普攻               |
|  503 | 狂铁     | 无限击飞               |
|  506 | 云中君   | 飞行状态下无限撕裂状态 |
|  510 | 孙策     | 无限回血（被动）       |

## 激门峡谷

以下方法不适用于iOS13以下系统。

打开Filza，找到APP管理器，点击LOL右侧的`i`。点击主程序，进入wildrift.app目录，找到主程序wildrift，点击右侧的`i`。点击打开方式-十六进制编辑器，返回后点击主程序wildrift。

按照以下内容完成修改后保存，然后重新下载游戏以避免闪退。

### 透视

选择左侧地址码并输入`01FD1FD0`，将`08 41 40 39`修改为`08 00 80 D2`。

### 除雾

选择左侧地址码并输入`01FCF238`，将`49 71 5F B8`修改为`C0 03 5F D6`。

## 王牌战士

必须在游戏中进行修改，必须加载完之后修改，一局只能修改一次。

### 游击型英雄无后

F32搜25，全部改0即可。

### 赵海龙独立无后

F32搜25，联合F32搜35，全部改0即可。

### 赵海龙隐身

F32搜0.0025，全部改99即可。

### 自瞄

游戏中需要关闭辅助瞄准。

i64搜4816377120，联合i64搜3212836864，修改为4811533255104790528即可。

# 建立SSH连接

将手机连接到电脑后，在电脑端打开爱思助手，在工具箱中选择打开SSH连接，记下IP和端口，其中127.0.0.1可以用localhost代替。

打开终端并输入以下命令，即可建立SSH连接。

```
// 2222为端口，localhost为IP
ssh -p 2222 root@localhost
```

# 常见问题

## Cydia无法联网

可用爱思助手安装1.2.3版本的乐网，暂时让Cydia联网。然后安装`连个锤子-越狱联网一键修复`插件，官方源如下。打开插件，点击修复网络，等待修复完成后注销即可。

```
http://apt.tinyapps.cn/
```

## Cydia提示证书无效

修改系统时间到当前时间即可。

## Cydia出现红色Depend依赖

缺失依赖的越狱源失效，翻墙后重新刷新源即可。

## Sileo提示「unable to fetch some archives」

若在手机端已安装NewTerm插件，则打开NewTerm并执行以下命令即可。若未安装，则先通过SSH使电脑连接到手机，然后在电脑端执行以下命令。

```
apt-get update --fix-missing
killall SpringBoard
```

## Slieo提示Target Packages is configured multiple times

在Slieo和Cydia中添加的源地址与名字冲突，在两边均添加同一个源时可能会出现该情况。总体不影响使用，建议卸载Cydia，并在NewTerm输入以下命令以移除cydia.list文件。

```
/etc/apt/sources.list.d
```

## Slieo提示trying to overwrite 'xxx'，with is also in package xxx 1.0

安装冲突。可以先卸载提示中的安装包，再尝试安装。

## Slieo提示missing 'Description' field

插件包缺少描述等相关信息。不影响环境和插件的使用。

## Slieo提示'Version' field value '': version string is empty

部分插件或脚本破坏了越狱环境。打开Fliza，进入路径/var/lib/dpkg/status，备份status文件后点击，选择文本编辑器模式打开。搜索lib.corefoundation，在Version一栏填写1676.10，存储即可。

## Slieo提示Depends PreferenceLoader

切换到软件包选项卡，点击日期旁边的三条横线，然后点击开发者以切换到开发者模式，然后搜索PreferenceLoader，安装即可。

## 无法正常内购

此问题是itunesstored进程无法正常启动所导致。若安装了内购破解插件，如LocalIAPStore，先尝试关闭或卸载。

若无效，则可使用DaemonDisabler开启itunesstored进程，插件源如下。安装后在插件设置中打开com.apple.itunesstored.plist，然后软重启。

```
https://level3tjg.xyz/repo/
```

若仍无效，可尝试使用Choicy禁止注入itunesstored。安装Choicy插件并进入Choicy设置，选择进程配置-守护进程，拉到最底部，点击显示所有守护进程，找到itunesstored后点击进入，打开禁用插件注入开关。注销并测试是否可以正常内购。

若依旧无法正常内购，可尝试重启或在安全模式下进行内购。

## 电脑连接SSH时提示「WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!」

打开用户目录下的.ssh/known_hosts文件，将有手机IP地址的记录删掉即可。

# 参考教程

## iOS越狱教程-内存基址修改器的外番(二)

```
https://mp.weixin.qq.com/s/LJAhI2pGOlnmxne6TAnBCw
```

## iOS越狱教程-内存基址修改器的神奇之处(十二)

```
https://mp.weixin.qq.com/s/0Je9UhWjVTuK-WIDcP886A
```

## iOS越狱教程-内存基址修改器的外番(三)

```
https://mp.weixin.qq.com/s/Il5_Gsbf89McIBM70zdNJg
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
