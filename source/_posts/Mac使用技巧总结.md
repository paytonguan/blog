---
title: Mac使用技巧总结
categories: Mac
abbrlink: Mac-Skills
date: 2019-11-25 14:12:29
tags:
---

![](topic.jpg)

总结了一些MacOS的使用技巧，不断更新。

<!-- more -->

# 终端相关

## 快捷键

|  快捷键   | 作用 |
|-----------|------|
| Command+K | 清屏     |

## 终端优化

### Homebrew

Homebrew是类似于Linux的apt-get软件包管理器。

#### 安装与卸载

##### 官方途径

在终端输入以下命令即可。

```
# 安装
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 卸载
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall.sh)"
```

##### 国内途径

###### 最新方法

```
# 安装
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"

# 卸载
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh)"
```

或以下命令。

```
# 安装
/bin/bash -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/ineo6/homebrew-install/install.sh)"

# 卸载
/bin/bash -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/ineo6/homebrew-install/uninstall.sh)"
```

若命令执行中卡在`Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core'`，可执行以下命令后再重新执行脚本。

```
cd "$(brew --repo)/Library/Taps/"
mkdir homebrew && cd homebrew
git clone git://mirrors.ustc.edu.cn/homebrew-core.git
```

若为安装cask时卡住，则为以下命令。

```
cd "$(brew --repo)/Library/Taps/"
cd homebrew
git clone https://mirrors.ustc.edu.cn/homebrew-cask.git
```

<details>
<summary>【进阶】对于M1芯片</summary>

M1芯片需要使用ARM版的Homebrew，安装路径为/opt/homebrew。安装完成后须在终端输入以下命令以设置环境变量。

```
# bash用户
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.bash_profile
eval "$(/opt/homebrew/bin/brew shellenv)"

# zsh用户
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zshrc
eval "$(/opt/homebrew/bin/brew shellenv)"
```

也可在M1芯片安装普通的x86版，命令如下。

```
arch -x86_64 /bin/bash -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/ineo6/homebrew-install/install.sh)"
```

若同时安装了两个版本，需要在~/.zshrc或~/.bash_profile添加以下命令。

```
alias abrew='arch -arm64 /opt/homebrew/bin/brew'
alias ibrew='arch -x86_64 /usr/local/bin/brew'
```

保存后，在终端输入`abrew`表示使用ARM版，输入`ibrew`表示使用x86版。
</details>

###### 失效教程

<details>
<summary></summary>

在终端输入以下命令以获取install文件。

```
curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install >> brew_install
```

打开所获取的install文件（一般在用户目录），更改脚本中`BREW_REPO`和`CORE_TAP_REPO`两处，两个镜像源任选一个。

```
// 中国科学技术大学源
BREW_REPO = "https://mirrors.ustc.edu.cn/brew.git".freeze
CORE_TAP_REPO = "https://mirrors.ustc.edu.cn/homebrew-core.git".freeze

// 清华大学源
BREW_REPO = "https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git".freeze
CORE_TAP_REPO = "https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git".freeze
```

在终端输入以下命令以执行脚本即可。

```
/usr/bin/ruby brew_install
```

卸载则输入以下命令。

```
cd `brew --prefix`
rm -rf Cellar
brew prune
rm `git ls-files`
rm -r Library/Homebrew Library/Aliases Library/Formula Library/Contributions
rm -rf .git
rm -rf ~/Library/Caches/Homebrew
```
</details>

#### 更换默认源

```
# 中国科学技术大学源
## 替换核心软件仓库
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

## 替换cask软件仓库
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git

## 替换Bottles源
## 以下二选一
（bash用户）
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile

（zsh用户）
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc


# 清华大学源
## 替换核心软件仓库
cd "$(brew --repo)"
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git

## 替换cask软件仓库
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git

## 替换Bottles源
## 以下二选一
（bash用户）
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile

（zsh用户）
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```

也可通过以下链接获取换源命令。

```
https://brew.idayer.com/guide/change-source
```

完成后在终端输入以下命令更新Homebrew。

```
brew update
brew upgrade
```

#### 常用命令

|             命令             |         操作         |
|------------------------------|----------------------|
| brew help                    | 帮助                 |
| brew list                    | 列出安装包           |
| brew update                  | 更新Homebrew         |
| brew upgrade                 | 更新包               |
| brew install [包名]          | 安装包               |
| brew install [包名]@版本     | 安装固定的包         |
| brew uninstall [包名]        | 卸载包               |
| brew info [包名]             | 查看包信息           |
| brew -v                      | Homebrew版本         |
| brew search [包名]           | 搜索可安装的包       |
| brew autoremove              | 自动卸载未用包       |
| brew cleanup                 | 清理所有包的旧版本   |
| brew outdated                | 查看需要更新的包     |
| brew deps --installed --tree | 列出软件包的依赖关系 |

#### 常用包

##### tree

用树状结构表示当前目录下的文件层级。

##### tcl

Python的图形化界面依赖。

#### 常见问题

##### brew install xxx 404

bottles镜像地址更新，须在~/.zshrc或~/.bash_profile中修改HOMEBREW_BOTTLE_DOMAIN的值。可通过更换默认源中的链接获取最新的bottles镜像地址。

##### command not found: brew

对于ARM版，需安装步骤添加环境变量。

对于x86版，可尝试在~/.zshrc或~/.bash_profile手动添加以下环境变量。

```
# bash用户
echo 'eval "$(/usr/local/Homebrew/bin/brew shellenv)"' >> ~/.bash_profile
eval "$(/usr/local/Homebrew/bin/brew shellenv)"

# zsh用户
echo 'eval "$(/usr/local/Homebrew/bin/brew shellenv)"' >> ~/.zshrc
eval "$(/usr/local/Homebrew/bin/brew shellenv)"
```

### 设置zsh为默认Shell

Catalina及以上系统默认shell已为zsh，无需设置。

可在终端输入以下命令查看当前使用的shell和已安装的shell。

```
echo $SHELL
cat /etc/shells
```

若未安装zsh，可通过以下命令安装。

```
brew install zsh
```

输入以下命令，更换默认shell为zsh，重启终端生效。

```
chsh -s /bin/zsh
```

### oh-my-zsh

#### 安装

oh-my-zsh用于美化zsh终端，采用下列命令安装oh-my-zsh。

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

在终端输入以下命令以打开配置文件。

```
sudo vim ~/.zshrc
```

按i键以修改文件。修改`ZSH_THEME`，即可修改oh-my-zsh的主题。oh-my-zsh的GitHub Wiki页面提供了主题列表。

```
https://github.com/robbyrussell/oh-my-zsh/wiki/themes
```

推荐使用以下主题。

```
ZSH_THEME="agnoster"
```

在文件末尾加入以下代码，以在终端不显示电脑名称。输入后按`Esc`，然后输入`:wq`并回车，保存并退出。

```
DEFAULT_USER=$USER
```

修改完成后，需运行以下命令以更新zsh配置。

```
source ~/.zshrc
```

使用agnoster主题需要安装Powerline字体。在终端运行以下命令。

```
git clone https://github.com/powerline/fonts.git --depth=1
cd fonts
./install.sh
```

在终端菜单点击偏好设置，在描述文件选项卡新建一个描述文件，将文本的字体修改为Powerline系的字体，注意需要在字体集中选择所有字体才能找到。保存后将该配置文件设置为默认，然后重启终端即可。

若希望改变主题默认配色，可在配置文件中配置相应的ANSI颜色。


#### 外置主题

除使用oh-my-zsh内置主题，也可安装其它外置主题。

##### Powerlevel10k

终端输入以下命令。

```
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k
```

在~/.zshrc修改以下内容，保存后重启终端。

```
POWERLEVEL9K_MODE="nerdfont-complete"
ZSH_THEME="powerlevel10k/powerlevel10k
```

该主题需要使用Nerd Fonts。打开以下链接，下载Hack.zip后解压，安装其中的所有字体。

```
https://github.com/ryanoasis/nerd-fonts/releases
```

完成后在终端的偏好设置中，将配置文件的字体设置为Hack Nerd Font。在终端自动配置流程将被触发，若未被触发，可运行以下命令。

```
p10k configure
```

#### 更新

在终端输入以下命令即可。

```
upgrade_oh_my_zsh
```

#### 插件

##### zsh-syntax-highlighting

语法高亮插件，在终端输入以下命令安装。

```
cd ~/.oh-my-zsh/custom/plugins &&\
git clone git://github.com/zsh-users/zsh-syntax-highlighting.git
```

修改~/.zshrc中的plugins，添加`zsh-syntax-highlighting`即可。

```
plugins=(
    zsh-syntax-highlighting
)
```

##### zsh-autosuggestions

自动建议补全插件，在终端输入以下命令安装。

```
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

修改~/.zshrc中的plugins，添加`zsh-autosuggestions`即可。

```
plugins=(
    zsh-autosuggestions
)
```

##### auto-jump

自动跳转插件，在终端输入以下命令安装。

```
git clone git://github.com/wting/autojump.git
cd autojump
./install.py or ./uninstall.py
```

也可通过brew安装。

```
brew install autojump
```

##### incr

自动补全插件。打开下面网站下载incr插件，并放置于`/.oh-my-zsh/customs/plugins/incr`中（若无则新建）。

```
https://mimosa-pudica.net/zsh-incremental.html
```

修改~/.zshrc，在文件末尾添加以下代码即可。

```
source $ZSH/custom/plugins/incr/incr*.zsh
```

### proxychains-ng

proxychains-ng用于终端翻墙，可用Homebrew安装。

```
brew install proxychains-ng
```

在终端输入以下命令以打开配置文件。

```
vim /usr/local/etc/proxychains.conf
```

在`[ProxyList]`下注释掉原来的代理，并添加代理类型。示例如下。

```
socks5 127.0.0.1 9050
```

如果所在的网络很复杂，可能需要在配置文件中启用`dynamic_chain`，然后在`[ProxyList]`下添加多个代理。两种类型的区别如下。

|         类型         |                                                     含义                                                     |
|----------------------|--------------------------------------------------------------------------------------------------------------|
| dynamic_chain        | 按照列表中出现的代理服务器的先后顺序组成一条链，如果有代理服务器失效，则自动将其排除，但至少要有一个是有效的 |
| strict_chain（默认） | 按照后面列表中出现的代理服务器的先后顺序组成一条链，要求所有的代理服务器都是有效的                           |

在`~/.zshrc`末尾添加如下内容并保存。

```
#   ---------------------------------------
#   proxychain-ng config
#   ---------------------------------------
alias pc='proxychains4'
```

配置完成后即可使用，示例如下。

```
pc curl http://www.google.com
```

## 终端命令

macOS中用`\40`代表空格。

### 磁盘工具添加调试菜单

```
defaults write com.apple.DiskUtility DUDebugMenuEnabled 1
```

### 显示Spotlight的搜索标题

```
mdimport /usr/include
mdimport /usr/local/include
mdimport /System/Library/Frameworks
```

### 安装Xcode Command Line

```
xcode-select --install
```

若提示`不能下载该软件，因为网络出现问题`，可打开以下链接，并搜索`Command Line`。可以通过在Mac App Store搜索Xcode，通过查看适合本机系统的Xcode版本以确定应当使用的Command Line版本。下载后安装即可。

```
https://developer.apple.com/download/more/
```

### 文件比较

```
diff [文件1] [文件2]
```

### 十进制转十六进制

```
printf '%x\n' [十进制数]
```

### 获取开机日志

```
log show --predicate "processID == 0" --start $(date "+%Y-%m-%d") --debug > ~/Desktop/bootlog.txt
```

### 应用多开

```
open -n /Applications/xxxx.app
```

### 测量系统放电瓦数

若想保留此函数的功能，则需将此函数写入`~/.profile`中。

```
// 将以下函数直接复制到终端运行
watts() {
  FORMULA=$(system_profiler SPPowerDataType | awk '/Amperage\ \(mA\):/ {printf $3" * "}; /Voltage\ \(mV\):/ {print $3}')
  WATTS=$(echo "scale=3; ${FORMULA} / 1000000" | bc 2>/dev/null)
  if ! [[ ${WATTS} =~ [0-9] ]];
  then
    WATTS=0
  fi
  echo "${WATTS} mW"
}

// 使用
watts
```

### 终端的电源活动监视器

若想保留此函数的功能，则需将此函数写入`~/.profile`中。

```
// 创建函数
powertop() {
  top -stats pid,command,power -o power
}

// 调用
powertop
```

### 查询CPU运行情况

```
sudo powermetrics --show-process-energy
```

### 把安装应用调成任意来源

```
sudo spctl --master-disable
```

### 获取某磁盘的UUID

```
diskutil info diskXsY | grep -i "Partition UUID" | rev | cut -d' ' -f 1 | rev
```

### 取消4位数密码限制

```
pwpolicy -clearaccountpolicies
passwd
```

### Catalina挂载系统分区

```
sudo -S mount -uw / && killall Finder
```

### 查看睡眠时间

```
sysctl -a | grep sleeptime
```

### 查看睡眠唤醒时间

```
sysctl -a | grep waketime
```

### 查看最近一天睡眠唤醒原因

```
log show --last 1d | grep "Wake reason"
```

### 检查AppleALC和Lilu是否工作正常

```
log show --predicate 'process == "kernel" AND (eventMessage CONTAINS "AppleALC" OR eventMessage CONTAINS "Lilu")' --style syslog –source
```

### 关闭系统更新提醒红点

```
defaults write com.apple.systempreferences AttentionPrefBundleIDs 0
```

### 删除权限不足的文件

```
sudo rm -r -f [你要删除的文件（可以从Finder拖进来，支持多个）]
```

### 关机

```
sudo shutdown -h now
```

### 开启原生SSD Trim功能

```
sudo trimforce enable
```

### 一键查询硬件信息

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/daliansky/dell7000/master/Tools/archey)"
```

### 笔记本插电源出提示音

```
// 开启
defaults write com.apple.PowerChime ChimeOnAllHardware -bool true; open /System/Library/CoreServices/PowerChime.app &

// 关闭
defaults write com.apple.PowerChime ChimeOnAllHardware -bool false; killall PowerChime
```

### 手动安装kexts

#### 移动kexts

```
// 删除重名/错误的kexts
sudo rm -Rf /Library/Extensions/KextToInstall.kext
sudo rm -Rf /System/Library/Extensions/KextToInstall.kext

// 把新kext移动到LE或SLE
sudo cp -R KextToInstall.kext /Library/Extensions
sudo cp -R KextToInstall.kext /System/Library/Extensions
```

#### 修复权限和重建缓存

Mac不使用内核文件进行引导，而使用预链接内核。重建缓存即重建内核缓存/预链接内核。可用以下终端命令，也可在磁盘工具中选择macOS磁盘并点击修复。以下命令仅适用于Catalina系统及以下。

```
# 修复权限
sudo chmod -Rf 755 /S*/L*/E*
sudo chown -Rf 0:0 /S*/L*/E*
sudo chmod -Rf 755 /L*/E*
sudo chown -Rf 0:0 /L*/E*
sudo rm -Rf /S*/L*/PrelinkedKernels/*
sudo rm -Rf /S*/L*/Caches/com.apple.kext.caches/*

# 重建缓存（法一）
# 将-U后面的改为其它路径，即可在其它盘上进行重建缓存
sudo touch -f /S*/L*/E*
sudo touch -f /L*/E*
sudo kextcache -Boot -U /

# 重建缓存（法二）
sudo kextcache -i /
```

### 让AirDrop支持有线传输

```
defaults write com.apple.NetworkBrowser BrowseAllInterfaces 1 && killall Finder
```

### 清空iCloud、iMassage和FaceTime缓存

```
cd ~/Library/Caches/
rm -R com.apple.Messages*
rm -R com.apple.imfoundation*
cd ~/Library/Preferences/

rm com.apple.iChat*
rm com.apple.icloud*
rm com.apple.ids.service*
rm com.apple.imagent*
rm com.apple.imessage*
 
rm com.apple.imservice*
rm -R ~/Library/Messages/
```

### 终端ftp工具

```
// 安装
brew install inetutils

// 使用
ftp
```

### 设置macOS电源相关

```
// 查看系统范围内的电源状态
pmset -g
pmset -g live

// 查看电源状态
pmset -g assertions

// 从日志查看电源状态
pmset -g log

// -a针对所有电源，-b针对电池电源，-c针对壁式充电器电源，-u以UPS电源为目标
// 休眠到内存（台式机默认设置）
sudo pmset -a hibernatemodes 0

// 休眠到硬盘
sudo pmset -a hibernatemodes 1

// 混合休眠（笔记本电脑默认配置）
sudo pmset -a hibernatemodes 3

// 省电休眠
sudo pmset -a hibernatemode 25

// 关闭powernap
sudo pmset -a powernap 0

// 关闭休眠
sudo pmset -a autopoweroff 0
sudo pmset -a standby 0

// 重置设置
pmset -a restoredefaults

// 查看睡眠唤醒原因
pmset -g log | grep -Ei 'wake.*due'

// 关闭硬盘摔落保护功能
sudo pmset -a sms 0
```

下表为参数含义对照。

| 参数                               | 含义                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| proximitywake                      | 相同iCloud ID的设备靠近唤醒（1开启，0关闭）                  |
| disksleep                          | 设置不活动后磁盘停止旋转之前的时间，值以分钟为单位（默认10） |
| standby                            | 在系统已休眠一段时间后，此功能将从休眠转变为休眠状态（1开启，0关闭） |
| standbydelayhigh/standbydelaylow   | 将睡眠会话写入磁盘并关闭内存电源之前的延迟（以秒为单位）。当电池电量超过50%时，使用standbydelaylow，当电池电量低于50%时，使用standbydelaylow。对于台式机，这些值应相同 |
| highstandbythreshold               | 待机延迟的电池阈值（默认50%）                                |
| autopoweroff                       | 从休眠状态转换为关闭状态                                     |
| autopoweroffdelay                  | 计算机关闭之前的延迟（以秒为单位）                           |

### 清空bootercfg存储在NVRAM的变量

```
sudo nvram bootercfg="log=0 debug=0"
```

### 将逻辑卷转换为物理卷

```
sudo diskutil corestorage revert /
```

### 查看加载驱动

```
// 查看所有加载的驱动
kextstat

// 查看除苹果以外加载的驱动
kextstat | grep -v "com.apple"

// 查看加载的非官方驱动
kextstat | grep -v "com.apple" | grep -v "Energy"

// 查看特定名称的驱动（以ACPIplat为例）
kextstat | grep -y acpiplat
kextstat | grep -i acpiplat
```

### Finder显示完整文件路径

```
// 显示
defaults write com.apple.finder _FXShowPosixPathInTitle -bool TRUE && killall Finder

// 取消显示
defaults write com.apple.finder _FXShowPosixPathInTitle -bool FALSE && killall Finder
```

### 用Finder打开终端当前路径

```
open .
```

### 终端调用Chrome

```
// 打开Chrome浏览器
open -a Google\ Chrome --args -disable-web-security

// 直接打开网站
open www.baidu.com
```

### 屏蔽系统更新

```
sudo softwareupdate --ignore "macOS Catalina"
```

### 去掉应用的小红点提示

```
defaults write com.apple.systempreferences AttentionPrefBundleIDs 0 && killall Dock
```

### 配置Safari的开发功能

或打开Safari后选择`偏好设置`，勾选`高级`选项卡下的`在菜单栏显示开发菜单`，也可达到相同效果。

```
// 开启
defaults write com.apple.Safari IncludeInternalDebugMenu 1

// 关闭
defaults write com.apple.Safari IncludeInternalDebugMenu 0
```

### 制作Mac安装盘

以Sierra安装包为例。

```
sudo /Applications/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/Sierra --applicationpath /Applications/Install\ macOS\ Sierra.app --nointeraction
```

### ._文件删除

```
dot_clean [路径]
// 或
find . -type f -name '._*' -delete
```

### 删除.DS_Store

```
find . -name '*.DS_Store' -type f -delete
```

### 禁止/启用.DS_Store自动生成

```
// 禁止
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool TRUE

// 启用
defaults delete com.apple.desktopservices DSDontWriteNetworkStores
```

### zsh导入bash环境变量

向`~/.zshrc`添加以下代码。

```
source ~/.bash_profile
```

在终端执行以下命令即可。

```
source .zshrc
```

### 暂时在zsh中使用bash

```
exec /bin/bash
```

### 查看EFI版本

EFI32或EFI64。

```
ioreg -l -p IODeviceTree | grep firmware-abi
```

# 系统技巧

## 查看系统版本

点击菜单栏中的关于本机即可。或在磁盘工具中也能看到磁盘对应的系统版本。

## 系统网络在线重装

在开机时按住以下快捷键之一即可。

|         快捷键         |                        效果                       |
|------------------------|---------------------------------------------------|
| Command+R              | 重新安装当前系统版本                              |
| Option+Command+R       | 升级到最新版macOS                                 |
| Shift+Option+Command+R | 安装出厂时的macOS或与其最接近的官方仍在提供的版本 |

## 访问Mac共享

在Mac打开设置，选择共享-文件共享，点击`选项`，勾选两个复选框，并勾选用于共享的用户。回到设置页面，打开`网络偏好设置`，选择活跃的连接，点击高级-WINS，输入Windows的工作组名称，一般为WORKGROUP。

在Windows端打开运行，输入`\\[Mac的IP地址]`即可访问。

## 旧款Mac安装Catalina

使用macOS Catalina Patcher即可。

```
http://dosdude1.com/catalina/
```

## 直接下载Boot Camp驱动程序

使用Brigadier即可。

```
https://github.com/timsutton/brigadier/releases
```

## 安全模式下读取EFI分区

注意，此法不能读取内置Windows分区和外接驱动器（非APFS/HFS）分区。

在正常模式的系统下执行以下命令以获取msdosfs.kext，默认在根目录的kexts文件夹下。

```
sudo mkdir /kexts
sudo cp -RX /System/Library/Extensions/msdosfs.kext /kexts
sudo /usr/libexec/PlistBuddy -c "Add :OSBundleRequired string" /kexts/msdosfs.kext/Contents/Info.plist
sudo /usr/libexec/PlistBuddy -c "Set :OSBundleRequired \"Safe Boot\"" /kexts/msdosfs.kext/Contents/Info.plist
sudo chmod -Rf 755 /kexts/msdosfs.kext
sudo chown -Rf 0:0 /kexts/msdosfs.kext
```

在安全模式下通过以下命令加载此kext，再挂载EFI分区即可。

```
sudo kextload /kexts/msdosfs.kext
```

## 连接ZJU有线网络

打开系统偏好设置-网络，点击+，接口选择vpn，类型选L2TP/IPSec，完成后填写下列信息。

```
服务器地址 / 10.5.1.7
账户名称 / 学号@a10（20/50元包月则为@a20/@a50）
```

点击鉴定设置-密码并输入，保存后点击`高级`。会话选项下三个选项均勾选，保存。打开终端输入以下代码。 

```
sudo vim /etc/ppp/options
```

按`i`键后输入下列语句。

```
plugin L2TP.ppp
l2tpnoipsec // 第一个字母为小写L
```

按Esc后输入`:wq`并回车，回到网络设置界面。连接上述设置好的VPN，正常情况下连接成功。

## 重置蓝牙配置

进入系统盘的/Library/Preference，删除`com.apple.Bluetooth.plist`文件后重启即可。

## 备份EFI分区脚本

脚本地址如下，运行EFI Backup-Restore.command即可。

```
https://github.com/corpnewt/EFI-Backup-Restore
```

## 系统精简

打开终端并输入以下命令。

```
// 获取Su权限
sudo su
passwd root
su root

// 解除系统应用
cd /
mkdir RemovedFiles
cd RemovedFiles
mkdir System-Library-LaunchAgents
FLA=/RemovedFiles/System-Library-LaunchAgents/
cd /System/Library/LaunchAgents/
mv com.apple.AddressBook*                      $FLA # 通讯录
mv com.apple.CalendarAgent.plist               $FLA # 日历
mv com.apple.iCloudUserNotifications.plist     $FLA # iCloud
mv com.apple.icbaccountsd.plist                $FLA # iCloud
mv com.apple.icloud.fmfd.plist                 $FLA # iCloud
mv com.apple.cloud*                            $FLA # iCloud
mv com.apple.imagent.plist                     $FLA # FaceTime
mv com.apple.IMLoggingAgent.plist              $FLA # FaceTime
mv com.apple.AirPlayUIAgent.plist              $FLA # Airplay
mv com.apple.AirPortBaseStationAgent.plist     $FLA
mv com.apple.bird.plist                        $FLA # iCloud 缓存
mv com.apple.findmymacmessenger.plist          $FLA
mv com.apple.gamed.plist                       $FLA # macOS 游戏中心
mv com.apple.parentalcontrols.check.plist      $FLA # 家长控制
mv com.apple.Maps.pushdaemon.plist             $FLA
mv com.apple.ScreenReaderUIServer.plist        $FLA
mv com.apple.speech.*                          $FLA

cd /System/Library/LaunchDaemons/
launchctl unload -wF com.apple.AirPlayXPCHelper.plist # AirPlay
launchctl unload -wF com.apple.apsd.plist # Apple push notification

# 应先在系统偏好设置中禁用位置服务，再执行该语句
launchctl unload -wF com.apple.locationd.plist

launchctl unload -wF com.apple.findmymac.plist
launchctl unload -wF com.apple.findmymacmessenger.plist

launchctl unload -wF com.apple.icloud.findmydeviced.plist
launchctl unload -wF com.apple.cloudfamilyrestrictionsd-mac.plist 
launchctl unload -wF com.apple.mbicloudsetupd.plist

launchctl unload -wF com.apple.dvdplayback.setregion.plist 

launchctl unload -wF com.apple.SubmitDiagInfo.plist 
launchctl unload -wF com.apple.CrashReporterSupportHelper.plist 
launchctl unload -wF com.apple.ReportCrash.Root.plist 
launchctl unload -wF com.apple.GameController.gamecontrollerd.plist

launchctl unload -wF com.apple.spindump.plist
launchctl unload -wF com.apple.metadata.mds.spindump.plist

# Disable cups (common unix printing service)
#launchctl unload -wF org.cups.cupsd.plist
#launchctl unload -wF org.cups.cups-lpd.plist
```

## 更换字体

打开/System/Library/Frameworks/ApplicationServices.framework/Frameworks/CoreText.framework/Resources/DefaultFontFallbacks.plist，将STHeitiSC-Light进行修改即可，如冬青黑体简体中文W3为HiraginoSansGB-W3。

## 远程控制

可通过iMessage，打开后选择或新建对方的信息会话，点击右上角后选择共享即可。

## 删除Launchpad的废图标

打开访达，选择前往-前往文件夹，输入`/private/var/folders`并回车。在本路径下搜索`com.apple.dock.launchpad`，双击进去可看到db。

打开终端并输入以下命令即可。

```
# 替换为刚才找到的db路径
cd /private/var/folders/[...]/db

sqlite3 db "delete from apps where title='[需要删除的应用名称]';"&&killall Dock
```

# 操作技巧

## Menu Bar操作

按住Command键并拖动菜单栏图标可调整其顺序，拖出菜单栏则清除此图标。

若菜单栏上没有图标，则转至`/System/Library/CoreServices/Menu Extras`，直接用鼠标将Eject.menu拖放至菜单栏即可。

## 常用快捷键

Mac键盘与普通键盘键位对应如下表。 

| Mac     | Windows             |
| ------- | ------------------- |
| Command | Windows徽标键       |
| Option  | Alt键               |
| Delete  | 空格（Backspace）键 |
| Return  | 回车（Enter）键     |

替换Command和Control键后，常用快捷键对应如下（Space键即空格键，Win键即Windows徽标键）。

| Mac                           | Windows                | 作用                       |
| ----------------------------- | ---------------------- | -------------------------- |
| **常用**                      |                        |                            |
| Option                        |                        | 显示菜单隐藏项             |
| Command+C                     | Ctrl+C                 | 复制                       |
| Command+V‍                     | Ctrl+V                 | 粘贴                       |
| Command+Option+V              | Ctrl+Alt+V             | （Ctrl+C后）剪切           |
| Command+Delete                | Ctrl+Backspace         | 删除                       |
| Command+Z                     | Ctrl+Z                 | 撤销                       |
| Command+A                     | Ctrl+A                 | 全选                       |
| Command+F                     | Ctrl+F                 | 全选                       |
| Command+S                     | Ctrl+S                 | 保存                       |
| **截屏**                      |                        |                            |
| Option                        |                        | 维持比例                   |
| Control                       |                        | 保存截图到剪切板           |
| Command+Shift+3               | Ctrl+Shift+3           | 截取全部屏幕到文件         |
| Command+Shift+Control+3       | Ctrl+Shift+Win+3       | 截取全部屏幕到剪切板       |
| Command+Shift+4               | Ctrl+Shift+4           | 截取所选屏幕区域到一个文件 |
| Command+Shift+Control+4       | Ctrl+Shift+Win+4       | 截取所选屏幕区域到剪贴板   |
| Command+Shift+Control+Space+4 | Ctrl+Shift+Win+Space+4 | 仅捕捉一个窗到剪贴板       |
| **文件**                      |                        |                            |
| Control+Command+I             |                        | 查看多个文件简介           |
| Command+I                     |                        | 查看单个文件简介           |
| Command+Shift+N               | Ctrl+Shift+N           | 新建文件夹                 |
| Command+Shift+G               | Ctrl+Shift+G           | 输入文件路径               |
| Command+Up                    | Ctrl+Up                | 回到上级目录               |
| Command+Down                  | Ctrl+Down              | 进入下级目录（打开文件）   |
| Space                         | Space                  | 预览                       |
| Command+O                     | Ctrl+O                 | 打开所选文件               |
| Command+Shift+Delete          | Ctrl+Shift+Backspace   | 清倒废纸篓                 |

## 临时禁止通知中心弹出通知

按住`Option`后点击右上角通知中心图标即可。

## 把PNG转成JPG

直接修改后缀名即可。

## 触控板设置三指拖移

进入系统偏好设置-辅助功能-鼠标与触控板-触控板选项…，勾选启用拖移并选择三指拖移即可。

## 归类文件

选中多个文件，右键选择`使用多个文件新建文件夹`。

## 显示隐藏文件

利用`command（⌘）+shift+.`即可。

也可打开终端，输入以下命令。

```
// 显示隐藏文件
defaults write com.apple.finder AppleShowAllFiles -bool true
// 或defaults write com.apple.finder AppleShowAllFiles YES

// 隐藏隐藏文件
defaults write com.apple.finder AppleShowAllFiles -bool false
// 或defaults write com.apple.finder AppleShowAllFiles NO
```

## 删除所有短信

设置短信为保留30天，然后设置日期，拨动往后的几个月份直至短信被删除即可。

# 软件技巧

## 破解百度网盘客户端限速

打开终端，输入下列命令以执行破解脚本。注意，只适用于2.2.2及以下的版本，2.2.3以后均加入了监测机制。

```
cd ~/Downloads && git clone https://github.com/CodeTips/BaiduNetdiskPlugin-macOS.git && ./BaiduNetdiskPlugin-macOS/Other/Install.sh
```

完成后打开百度云盘，此时要输入大概十次系统密码，进入云盘界面发现限速已经破解。下载时点击`免费试用`，倒计时将会保持在8秒。

## QQ功能扩展

下载以下仓库，解压后在终端运行Other文件夹中的`install.sh`即可。

```
https://github.com/TKkk-iOSer/QQPlugin-macOS/tree/master
```

## 微信功能扩展

该插件已造成封号，不推荐使用。

在终端输入以下命令即可。

```
curl -o- -L https://raw.githubusercontent.com/lmk123/oh-my-wechat/master/install.sh | bash -s
```

项目地址如下。

```
https://github.com/MustangYM/WeChatExtension-ForMac
```

安装扩展后可以多开微信，在命令行输入以下命令即可。

```
open -n /Applications/WeChat.app
```

## Quick Look插件

通过安装Quick Look插件，可以在点击空格预览的时候有更多功能，仓库如下。

```
https://github.com/sindresorhus/quick-look-plugins
```

在终端通过以下命令进行安装，常用插件有qlcolorcode、qlstephen、qlmarkdown、quicklook-json、qlimagesize、suspicious-package等。

```
brew cask install [插件名]
```


## Xcode SDK文件

用于为Xcode提供旧版SDK文件。

```
https://github.com/phracker/MacOSX-SDKs
```

# 常用APP

## APP库

### 普通

```
https://github.com/catofmrlu/MacApps
https://github.com/hemanth/awesome-pwa
https://www.macupdate.com/
```

### 破解

```
http://mac-torrent-download.net
https://cmacapps.com
https://xclient.info/
https://www.waitsun.com/
https://www.macbl.com/
```

## 磁盘分区

### Paragon Hard Disk Manager

磁盘分区。

```
https://www.zhinin.com/paragon_hard_disk_manager-mac.html
```

## 划词翻译

### Bob

```
https://github.com/ripperhe/Bob
```

## OCR识别

### CaptuocrToy

```
https://github.com/gragrance/CaptuocrToy/releases
```

## 系统性能与监控

### DPCIManager

查看硬件信息。

```
https://sourceforge.net/projects/dpcimanager/files/latest/download
```

### DarwinDumper

输出硬件信息。

```
https://bitbucket.org/blackosx/darwindumper/downloads/
```

### Geekbench

跑分软件。

```
https://www.geekbench.com/
```

### Intel Power Gadget

查看CPU状态。

```
https://software.intel.com/en-us/articles/intel-power-gadget
```

### iStat Menus 6

用于性能监控。

```
// 下载地址
https://bjango.com/mac/istatmenus/

// 注册信息
Email: 982092332@qq.com
SN: GAWAE-FCWQ3-P8NYB-C7GF7-NEDRT-Q5DTB-MFZG6-6NEQC-CRMUD-8MZ2K-66SRB-SU8EW-EDLZ9-TGH3S-8SGA
```

### IOJones

IORegistryExplorer的替代品。

```
https://sourceforge.net/projects/iojones/
```

### Heaven

显卡性能测试软件。

```
https://benchmark.unigine.com/heaven
```

### Novabench

跑分软件。

```
https://novabench.com/download#personal
```

### CUDA-Z

查看Nvidia驱动。

```
https://sourceforge.net/projects/cuda-z/
```

### AJA System Test

测试磁盘速度。

```
https://macdownload.informer.com/aja-system-test/download/
```

### MachineProfile

查询电脑信息，Mac App Store可下载。

## 系统设置

### Simple Display Rotation Wrapper

屏幕旋转。

```
https://github.com/fewtarius/displayrotation
```

### Scroll Reverser

只改变鼠标滚轮方向而不改变触控板滑动方向，在终端输入以下命令安装。此APP会让三指点击失效。

```
brew cask install scroll-reverser
```

### ShowHiddenFiles

显示/隐藏文件。

```
https://gotoes.org/sales/ShowHiddenFilesMacOSX/How_To_Show_Hidden_Files.php
```

### Trim Enabler

为SSD打开Trim功能。

```
https://cindori.org/trimenabler/
```

### Pacifist

安装包文件提取工具。

```
https://www.zhinin.com/pacifist-mac.html
```

注册码如下。

```
Name / Tom Smith
Serial number / 4Ip8izH03A-0K48zlp3zw-1Sp
```

### EFI MountianShow

挂载EFI分区。

```
https://www.tonymacx86.com/threads/updated-efi-mounter-v3-renamed-to-efi-mountianshow.210413/
```

### RAMDisk Utility

生成内存缓存盘，程序在其中运行速度可明显提升。

```
https://www.firewolf.science/2015/06/ramdisk-utility-for-os-x/
```

### TotalFinder

Finder的增强。

```
http://www.pc6.com/mac/113433.html
```

## 系统优化

### OnyX

功能全面的系统清理工具。

```
https://www.titanium-software.fr/en/onyx.html
```

## 编辑工具

### Hex Fiend

Hex编辑器。

```
http://ridiculousfish.com/hexfiend/
```

### FileMerge

安装Xcode后即可搜索到，用于比较代码差异，常用于编写SSDT时比较打补丁前后的DSDT差异。

## 音乐播放

### VOX

音乐播放软件。

```
https://vox.rocks/mac-music-player/download
```

## 其它

### Touchbar Pet

Touchbar养宠物。

```
https://www.cr173.com/mac/1100456.html
```

### Classic Finder

模仿旧版Finder。

```
https://bitbucket.org/bszyman/classic-finder-app/downloads/
```

### Itsycal

任务栏添加日历。

```
https://www.mowglii.com/itsycal/
```

### Keka

解压缩软件。

```
https://www.keka.io/zh-cn/
```

### Magnet

分屏软件。

```
https://macwk.com/soft/magnet
https://www.zhinin.com/magnet-mac.html
```

# Python 3

Big Sur已自带python3，Catalina及以下自带python 2。

## 安装

### 通过pyenv

```
brew install pyenv
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc
echo 'export LDFLAGS="${LDFLAGS} -L/usr/local/opt/zlib/lib"' >> ~/.zshrc
echo 'export CPPFLAGS="${CPPFLAGS} -I/usr/local/opt/zlib/include"' >> ~/.zshrc
echo 'export PKG_CONFIG_PATH="${PKG_CONFIG_PATH} /usr/local/opt/zlib/lib/pkgconfig"' >> ~/.zshrc
source ~/.zshrc
exec "$SHELL"
brew install openssl readline sqlite3 xz zlib
pyenv install -l
pyenv install [版本号]
```

### ~~旧方法~~

<details>
<summary></summary>

查看系统自带的python版本。

```
python -V
```

用Homebrew安装python 3。

```
brew install python3
```

查看python 3位置，并记录。

```
which python3
```

打开配置文件。

```
// 以下二选一
（终端shell为zsh）
vi ~/.zshrc

（终端shell为bash）
vi ~/.bash_profile
```

在文件中添加下面语句，保存并退出。

```
// 路径改为上面记录的python 3路径
alias python="/usr/local/bin/python3"
```

编译系统配置文件。

```
// 以下二选一
（终端shell为zsh）
source ~/.zshrc

（终端shell为bash）
source ~/.bash_profile
```

查看系统python版本，应已改为python 3。

```
python -V
```
</details>

## pip

pip是python的安装包工具。

### 安装

在终端输入以下命令即可。

```
sudo easy_install pip
```

### 更新

注意不要用sudo。

```
pip install --user --upgrade pip
```

### 卸载

在终端输入以下命令即可。

```
sudo pip uninstall pip
```

### 包管理

#### 卸载所有由pip安装的包

```
pip uninstall -y -r <(pip freeze)
```

### 常见问题

#### 安装后出现Warning: pip is being invoked by an old script wrapper

不要将pip和sudo一起使用，否则将容易出现该错误。

若出现该错误，可打开~/.zshrc，将以下行取消注释，保存后重启终端。

```
# If you come from bash you might have to change your $PATH.
export PATH=$HOME/bin:/usr/local/bin:$PATH
```

然后在终端输入以下命令以重新安装即可。

```
python3 -m pip install --upgrade --force-reinstall pip
```

## 下载加速

源地址如下。

```
https://pypi.tuna.tsinghua.edu.cn/simple/
http://pypi.mirrors.ustc.edu.cn/simple/
http://mirrors.aliyun.com/pypi/simple/
https://pypi.mirrors.ustc.edu.cn/simple/
http://pypi.douban.com/simple/
```

### 暂时加速

```
pip install [包名] -i [镜像源地址]
```

### 直接换源

```
pip install pip -U
pip config set global.index-url [镜像源地址]
```

## 相关包

### Frida

到以下网站下载Frida的安装包并放到用户目录，为egg格式。

```
https://pypi.org/project/frida/#files
```

打开终端并输入以下命令安装，其中路径为刚才下载的安装包。

```
easy_install [路径]
```

# 常见问题

## 构建Xcode时提示「xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer directory」

在终端输入以下命令即可。

```
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer/ 
xcode-select --print-path
// 输出/Applications/Xcode.app/Contents/Developer
```

## App在Catalina下提示已损坏

在终端输入以下命令即可。

```
// /Applications/xxxx.app换为所需App路径，推荐直接将app文件拖入终端
sudo xattr -d com.apple.quarantine /Applications/xxxx.app
```

## 因AppleGFXHDADriver重启

将SLE下的AppleGFXHDA.kext删除或更改后缀即可。

## 旧Mac电脑无法开启高分辨率

可使用mac-pixel-clock-patch-V2补丁，仓库如下。

```
https://github.com/Floris497/mac-pixel-clock-patch-V2
```

# 参考教程

## pyenv

```
https://github.com/pyenv/pyenv
https://github.com/pyenv/pyenv-installer
https://github.com/jiansoung/issues-list/issues/13
```

## `为什么创建点下划线._文件，如何避免使用它们？`

```
https://qastack.cn/apple/14980/why-are-dot-underscore-_-files-created-and-how-can-i-avoid-them
```

## ItChat 系列 0 - 初识 ItChat

```
https://www.playpi.org/2019020701.html
```

## Homebrew国内如何自动安装（国内地址）

```
https://zhuanlan.zhihu.com/p/111014448
```

## 在Mac系统中如何显示和隐藏文件

```
https://www.jianshu.com/p/a1c2495b02aa
```

## 如何删除GIT中的.DS_Store

```
https://www.jianshu.com/p/fdaa8be7f6c3
```

## Mac 重装系统教程(二)：网络在线重装

```
https://m.sohu.com/a/325579861_654244
```

## OS X 默认字体的讨论与修改方法

```
https://blog.royli.dev/2018/os-x-mo-ren-zi-ti-de-tao-lun-yu-xiu-gai-fang-fa-2d38abb1
```

## 这篇 iTerm2 + Oh My Zsh 教程手把手让你成为这条街最靓的仔

```
https://segmentfault.com/a/1190000022813972
```

## 镜像快速安装Homebrew教程

```
https://brew.idayer.com/
```

## 删除pip安装的所有软件包的最简单方法是什么？

```
https://www.imooc.com/wenda/detail/605683
```

## 删除启动台（LaunchPad）残留的图标

```
https://zhuanlan.zhihu.com/p/55866195
```

## python - Warning: pip is being invoked by an old script wrapper - Stack Overflow

```
https://stackoverflow.com/questions/60029215/warning-pip-is-being-invoked-by-an-old-script-wrapper
```

## Mac terminal 清屏快捷键

```
https://blog.csdn.net/u010164190/article/details/78622884
```
