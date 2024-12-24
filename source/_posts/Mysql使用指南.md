---
title: Mysql使用指南
categories: Skill
abbrlink: Instructions-Of-Mysql
date: 2020-12-19 21:45:57
tags:
---

![](topic.jpg)

Mysql使用指南。

<!-- more -->

# 安装

从以下网站下载并安装。

```
https://dev.mysql.com/downloads/mysql/
```

# 配置环境变量

对于Mac，打开用户目录下的.bash_profile（zsh为.zshrc）文件，在末尾复制以下内容并保存。

```
export PATH=$PATH:/usr/local/mysql/bin
export PATH=$PATH:/usr/local/mysql/support-files
```

打开终端并输入以下命令。

```
// bash
source ~/.bash_profile

// zsh
source ~/.zshrc
```

# 启动服务

在终端输入以下命令。

```
sudo mysql.server start
sudo mysql.server status
```

# 进入命令行

输入以下命令即可。其中-u表示username，-p表示password。

```
mysql -u <用户名> -p [<密码>]
```

# 基本操作

## 创建数据库

```
create database <数据库名>;
```

## 创建新表

```
create table <表名>{
	<键名> <键值> (<属性>),
	(Cno tinyint not null auto_increment primary key,)
	<键名> <键值>,
	...
	<键名> <键值>
};
```

## 插入数据

注意，如果键的属性为auto_increment，则插入数据时可以不用写该值。

```
insert into <表名>(<键名>,...,<键名>) values (<键值>,...,<键值>);
```

## 更新数据

如果有外码约束，需先消除外码约束。

```
// 取消外码约束
set foreign_key_checks = 0;

// 更新数据
update <表名> set <键名>=<键值> where <条件>;

// 恢复外码约束
set foreign_key_checks = 1;
```

## 删除数据

如果有外码约束，需先消除外码约束。

```
// 取消外码约束
set foreign_key_checks = 0;

// 更新数据
delete from <表名> where <条件>;

// 恢复外码约束
set foreign_key_checks = 1;
```

# 端口号相关

## 查询

在mysql命令行输入以下命令即可。

```
show global variables like ‘port’;
```

## 修改

对于Mac，打开配置文件/Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist，修改`-port=`后面的数字即可。

# CLion集成

具体可参照以下链接。

```
https://www.jetbrains.com/help/clion/connecting-to-a-database.html
```

# 配置ODBC

以下操作均在Windows下完成。

## 安装驱动

在安装MySQL时选中ODBC组件即可。

## 配置ODBC源

打开控制面板，选择管理工具-ODBC数据源，点击添加，选择MySQL ODBC 8.0 ANSI Driver，按照信息填写即可。
