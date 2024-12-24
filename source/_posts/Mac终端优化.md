---
title: Mac终端优化
categories: Mac
abbrlink: Mac-Terminal-Optimization
date: 2019-12-23 14:12:29
tags:
---

![](topic.jpg)

Mac终端优化。

<!-- more -->

# 快捷键

|  快捷键   | 作用 |
|-----------|------|
| Command+K | 清屏     |

# 终端优化

## Homebrew

Homebrew是类似于Linux的apt-get软件包管理器。

### 安装与卸载

#### 官方途径

在终端输入以下命令即可。

```
# 安装
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 卸载
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall.sh)"
```

#### 国内途径

##### 最新方法

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

##### 失效教程

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

### 更换默认源

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

### 常用命令

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

### 常用包

#### tree

用树状结构表示当前目录下的文件层级。

#### tcl

Python的图形化界面依赖。

### 操作技巧

#### 安装被disable的包

执行以下命令。

```
brew tap homebrew/core --force
brew edit [需要安装的包]
```

在打开的编辑器中，去除类似以下内容的行。

```
disable! date: "2022-07-31", because: :unmaintained
```

保存后用以下命令安装即可。

```
HOMEBREW_NO_INSTALL_FROM_API=1 brew install [需要安装的包]
```

### 常见问题

#### brew install xxx 404

bottles镜像地址更新，须在~/.zshrc或~/.bash_profile中修改HOMEBREW_BOTTLE_DOMAIN的值。可通过更换默认源中的链接获取最新的bottles镜像地址。

#### command not found: brew

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

## 设置zsh为默认Shell

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

## oh-my-zsh

### 安装

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


### 外置主题

除使用oh-my-zsh内置主题，也可安装其它外置主题。

#### Powerlevel10k

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

### 更新

在终端输入以下命令即可。

```
upgrade_oh_my_zsh
```

### 插件

#### zsh-syntax-highlighting

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

#### zsh-autosuggestions

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

#### auto-jump

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

#### incr

自动补全插件。打开下面网站下载incr插件，并放置于`/.oh-my-zsh/customs/plugins/incr`中（若无则新建）。

```
https://mimosa-pudica.net/zsh-incremental.html
```

修改~/.zshrc，在文件末尾添加以下代码即可。

```
source $ZSH/custom/plugins/incr/incr*.zsh
```

## proxychains-ng

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

# 终端命令

macOS中用`\40`代表空格。

## 磁盘工具添加调试菜单

```
defaults write com.apple.DiskUtility DUDebugMenuEnabled 1
```

## 显示Spotlight的搜索标题

```
mdimport /usr/include
mdimport /usr/local/include
mdimport /System/Library/Frameworks
```

## 安装Xcode Command Line

```
xcode-select --install
```

若提示`不能下载该软件，因为网络出现问题`，可打开以下链接，并搜索`Command Line`。可以通过在Mac App Store搜索Xcode，通过查看适合本机系统的Xcode版本以确定应当使用的Command Line版本。下载后安装即可。

```
https://developer.apple.com/download/more/
```

## 文件比较

```
diff [文件1] [文件2]
```

## 十进制转十六进制

```
printf '%x\n' [十进制数]
```

## 获取开机日志

```
log show --predicate "processID == 0" --start $(date "+%Y-%m-%d") --debug > ~/Desktop/bootlog.txt
```

## 应用多开

```
open -n /Applications/xxxx.app
```

## 测量系统放电瓦数

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

## 终端的电源活动监视器

若想保留此函数的功能，则需将此函数写入`~/.profile`中。

```
// 创建函数
powertop() {
  top -stats pid,command,power -o power
}

// 调用
powertop
```

## 查询CPU运行情况

```
sudo powermetrics --show-process-energy
```

## 把安装应用调成任意来源

```
sudo spctl --master-disable
```

## 获取某磁盘的UUID

```
diskutil info diskXsY | grep -i "Partition UUID" | rev | cut -d' ' -f 1 | rev
```

## 取消4位数密码限制

```
pwpolicy -clearaccountpolicies
passwd
```

## Catalina挂载系统分区

```
sudo -S mount -uw / && killall Finder
```

## 查看睡眠时间

```
sysctl -a | grep sleeptime
```

## 查看睡眠唤醒时间

```
sysctl -a | grep waketime
```

## 查看最近一天睡眠唤醒原因

```
log show --last 1d | grep "Wake reason"
```

## 检查AppleALC和Lilu是否工作正常

```
log show --predicate 'process == "kernel" AND (eventMessage CONTAINS "AppleALC" OR eventMessage CONTAINS "Lilu")' --style syslog –source
```

## 关闭系统更新提醒红点

```
defaults write com.apple.systempreferences AttentionPrefBundleIDs 0
```

## 删除权限不足的文件

```
sudo rm -r -f [你要删除的文件（可以从Finder拖进来，支持多个）]
```

## 关机

```
sudo shutdown -h now
```

## 开启原生SSD Trim功能

```
sudo trimforce enable
```

## 一键查询硬件信息

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/daliansky/dell7000/master/Tools/archey)"
```

## 笔记本插电源出提示音

```
// 开启
defaults write com.apple.PowerChime ChimeOnAllHardware -bool true; open /System/Library/CoreServices/PowerChime.app &

// 关闭
defaults write com.apple.PowerChime ChimeOnAllHardware -bool false; killall PowerChime
```

## 手动安装kexts

### 移动kexts

```
// 删除重名/错误的kexts
sudo rm -Rf /Library/Extensions/KextToInstall.kext
sudo rm -Rf /System/Library/Extensions/KextToInstall.kext

// 把新kext移动到LE或SLE
sudo cp -R KextToInstall.kext /Library/Extensions
sudo cp -R KextToInstall.kext /System/Library/Extensions
```

### 修复权限和重建缓存

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

## 让AirDrop支持有线传输

```
defaults write com.apple.NetworkBrowser BrowseAllInterfaces 1 && killall Finder
```

## 清空iCloud、iMassage和FaceTime缓存

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

## 终端ftp工具

```
// 安装
brew install inetutils

// 使用
ftp
```

## 设置macOS电源相关

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

## 清空bootercfg存储在NVRAM的变量

```
sudo nvram bootercfg="log=0 debug=0"
```

## 将逻辑卷转换为物理卷

```
sudo diskutil corestorage revert /
```

## 查看加载驱动

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

## Finder显示完整文件路径

```
// 显示
defaults write com.apple.finder _FXShowPosixPathInTitle -bool TRUE && killall Finder

// 取消显示
defaults write com.apple.finder _FXShowPosixPathInTitle -bool FALSE && killall Finder
```

## 用Finder打开终端当前路径

```
open .
```

## 终端调用Chrome

```
// 打开Chrome浏览器
open -a Google\ Chrome --args -disable-web-security

// 直接打开网站
open www.baidu.com
```

## 屏蔽系统更新

```
sudo softwareupdate --ignore "macOS Catalina"
```

## 去掉应用的小红点提示

```
defaults write com.apple.systempreferences AttentionPrefBundleIDs 0 && killall Dock
```

## 配置Safari的开发功能

或打开Safari后选择`偏好设置`，勾选`高级`选项卡下的`在菜单栏显示开发菜单`，也可达到相同效果。

```
// 开启
defaults write com.apple.Safari IncludeInternalDebugMenu 1

// 关闭
defaults write com.apple.Safari IncludeInternalDebugMenu 0
```

## 制作Mac安装盘

以Sierra安装包为例。

```
sudo /Applications/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/Sierra --applicationpath /Applications/Install\ macOS\ Sierra.app --nointeraction
```

## ._文件删除

```
dot_clean [路径]
// 或
find . -type f -name '._*' -delete
```

## 删除.DS_Store

```
find . -name '*.DS_Store' -type f -delete
```

## 禁止/启用.DS_Store自动生成

```
// 禁止
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool TRUE

// 启用
defaults delete com.apple.desktopservices DSDontWriteNetworkStores
```

## zsh导入bash环境变量

向`~/.zshrc`添加以下代码。

```
source ~/.bash_profile
```

在终端执行以下命令即可。

```
source .zshrc
```

## 暂时在zsh中使用bash

```
exec /bin/bash
```

## 查看EFI版本

EFI32或EFI64。

```
ioreg -l -p IODeviceTree | grep firmware-abi
```

# 终端替代

可使用以下软件。

## iTerm2

```
https://iterm2.com/
```

## Hyper

```
https://hyper.is/
```

# 参考教程

## Mac terminal 清屏快捷键

```
https://blog.csdn.net/u010164190/article/details/78622884
```

## Homebrew国内如何自动安装（国内地址）

```
https://zhuanlan.zhihu.com/p/111014448
```

## 这篇 iTerm2 + Oh My Zsh 教程手把手让你成为这条街最靓的仔

```
https://segmentfault.com/a/1190000022813972
```

## 镜像快速安装Homebrew教程

```
https://brew.idayer.com/
```

## Can you install disabled Homebrew packages?

```
https://stackoverflow.com/questions/73586208/can-you-install-disabled-homebrew-packages/73595534
```
