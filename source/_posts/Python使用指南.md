---
title: Python使用指南
categories: Skill
abbrlink: Instructions-Of-Python
date: 2019-12-29 14:12:29
tags:
---

![](topic.jpg)

对于Mac而言，Big Sur已自带python3，Catalina及以下自带python 2。

<!-- more -->

# 安装

## 通过pyenv

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

<details>
<summary>旧方法</summary>

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

# pip

pip是python的安装包工具。

## 安装

在终端输入以下命令即可。

```
sudo easy_install pip
```

## 更新

注意不要用sudo。

```
pip install --user --upgrade pip
```

## 卸载

在终端输入以下命令即可。

```
sudo pip uninstall pip
```

## 包管理

### 卸载所有由pip安装的包

```
pip uninstall -y -r <(pip freeze)
```

## 常见问题

### 安装后出现Warning: pip is being invoked by an old script wrapper

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

# 下载加速

源地址如下。

```
https://pypi.tuna.tsinghua.edu.cn/simple/
http://pypi.mirrors.ustc.edu.cn/simple/
http://mirrors.aliyun.com/pypi/simple/
https://pypi.mirrors.ustc.edu.cn/simple/
http://pypi.douban.com/simple/
```

## 暂时加速

```
pip install [包名] -i [镜像源地址]
```

## 直接换源

```
pip install pip -U
pip config set global.index-url [镜像源地址]
```

# 相关包

## Frida

到以下网站下载Frida的安装包并放到用户目录，为egg格式。

```
https://pypi.org/project/frida/#files
```

打开终端并输入以下命令安装，其中路径为刚才下载的安装包。

```
easy_install [路径]
```

# 参考教程

## pyenv

```
https://github.com/pyenv/pyenv
https://github.com/pyenv/pyenv-installer
https://github.com/jiansoung/issues-list/issues/13
```

## 删除pip安装的所有软件包的最简单方法是什么？

```
https://www.imooc.com/wenda/detail/605683
```

## python - Warning: pip is being invoked by an old script wrapper - Stack Overflow

```
https://stackoverflow.com/questions/60029215/warning-pip-is-being-invoked-by-an-old-script-wrapper
```

