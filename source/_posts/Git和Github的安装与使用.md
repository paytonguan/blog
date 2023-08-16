---
title: Git和Github的安装与使用
categories: Skill
abbrlink: Git-Installation
date: 2019-11-25 17:50:29
tags:
---

![](topic.jpg)

Git是一个常用的工具，与github一同使用。

<!-- more -->

# Git

Git是提交记录的集合。

## 安装

Mac下已自带git。

Windows需自行下载。到官网下载`git for windows`，双击安装包进行安装，注意在Terminal Emulator的设置中选择Use Windows' default console window，这样就可以在cmd中使用git命令。

打开cmd，输入git，检查是否安装成功。

```
https://gitforwindows.org/
```

## 新手使用

以下为创建/克隆一个仓库并完成提交的过程，按照命令顺序执行即可。

### 连接到Github账号

只需配置一次。打开终端输入以下代码，一路回车生成密钥。

```
git config --global user.name "github的用户名"
git config --global user.email "github的注册邮箱地址"
ssh-keygen -t rsa -C "github的注册邮箱地址" 
```

密钥默认生成在用户目录下的`.ssh`文件夹内。打开id_rsa.pub，复制所有内容后，在网页上登录Github，点击头像，选择Settings-SSH and GPG keys，选择New SSH key，名称任取，内容把刚才复制的粘贴上去，保存即可。

### 本地新建仓库或克隆现有仓库

#### 本地新建仓库

即仓库初始化，只需在第一次新建仓库时执行。在终端切换到工作目录，然后执行以下命令以创建本地仓库。

```
git init
```

#### 克隆现有仓库

克隆仓库则无需对仓库进行初始化。

```
# 创建本地仓库的克隆版本
git clone /path/to/repository

# 创建远程服务器仓库的克隆版本
git clone git@github.com:[用户名]/[仓库名].git
```

### 设定远程仓库地址

只需在第一次新建仓库时执行，需要在Github网页版上先新建好仓库。对于同一个本地仓库而言，该命令只在初始化仓库时执行。

```
git remote add origin git@github.com:[用户名]/[仓库名].git
```

其余可用命令如下。

|               命令               |           含义           |
|----------------------------------|--------------------------|
| git remote add origin <url>      | 注册远程版本库           |
| git remote set-url origin <url>  | 修改远程版本库地址       |
| git remote rename origin origin1 | 重命名远程版本库         |
| git remote rm origin             | 删除远程版本库           |
| git remote update                | 更新所有注册的远程版本库 |

### 工作流处理

每次进行修改后均要执行。工作流有三个部分，其中工作目录用于存放原始文件，暂存区用于临时保存改动，HEAD指向最后一次提交的结果。

#### 提交到暂存区

添加所有文件的命令如下。

```
git add .
```

其余可用命令如下。

|           命令           |                                含义                                |
|--------------------------|--------------------------------------------------------------------|
| git add [文件名]         | 添加特定文件                                                       |
| git add -i               | 交互式添加文件                                                     |
| git add -u               | 将（被版本库追踪的）本地文件的变更（修改、删除）全部记录到暂存区中 |
| git add -A               | 将工作区中的所有改动及新增文件添加到暂存区                         |
| git add -p [文件名]      | 选择文件中需要commit的修改（修改文件后git会进行记录，运行本命令时会显示所有改动并询问哪些改动需要被提交）                                           |
| git rm -r [文件名]       | 删除文件                                                           |
| git rm --cached [文件名] | 直接从暂存区删除文件                                               |
| git clean -fd            | 清理未跟踪的文件和目录                                             |
| git clean -nd            | 测试上面命令会删除哪些文件                                         |

#### 提交到HEAD

每执行一次本命令，当前修改即被打包为一个提交记录，且前一个提交记录成为当前提交记录的父节点，当前分支头HEAD则指向当前分支。

```
git commit -m "..."
```

提交前后示意如下，其中main为分支，星号表示当前处于该分支。注意commit只会移动当前分支，而不影响其它分支。

```
# 执行前
C0───C1
      │
      main*

# 执行后
C0───C1───C2
           │
           main*
```

该命令参数含义如下。

|      参数      |                  含义                  |
|----------------|----------------------------------------|
| -m "message"   | 提交说明                               |
| -a             | 提交所有修改过的文件                   |
| --allow-empty  | 允许空白提交                           |
| --amend        | 对上一次的提交进行修补，不生成新的提交 |
| --reset-author | 同步提交者信息                         |
| -c [名称]      | 使用[名称]的提交说明                   |

#### 由HEAD推送到远程仓库

```
# 推送到master分支
git push -u origin master
```

## 分支管理

分支指向特定的提交记录。

### 新建与切换分支

```
# 在本地新建bugFix分支，<ref>为分支指向的位置，可省略，默认指向HEAD
git branch bugFix <ref>

# 切换到本地bugFix分支
git checkout bugFix
```

以上两条命令等同于以下一条命令。

```
# 在本地新建bugFix分支并切换到本分支
git checkout -b bugFix
```

执行以上命令前后提交记录如下。

```
# 执行前
C0───C1
      │
      main*

# 执行后
C0───C1
      │
      main
      bugFix*
```

### 合并分支

```
# 将bugFix分支合并到当前分支
git merge bugFix
```

执行以上命令前后提交记录如下。

```
# 执行前
           bugFix
           │
C0───C1───C2
      │
      └───C3
           │
           main*

# 执行后
           bugFix
           │
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
                 │
                 main*
```

注意，若需将bugFix分支移动到main，则运行以下命令。由于main继承自bugFix，因此Git会将bugFix移动到main处。

```
git checkout bugFix
git merge main
```

执行以上命令前后提交记录如下。

```
# 执行前
           bugFix
           │
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
                 │
                 main*

# 执行后
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
                 │
                 main
                 bugFix*
```

merge的方法分为三种，分别为普通merge、rebase merge和squash merge，区别如下。

```
# 执行前
# 假设需要将bugFix的分支合并到main上
                    bugFix
                    │
C0───C1───C2───C3───C4
      │
      └───C5
           │
           main*


# 普通merge
# 开始时在main分支
# git merge bugFix

                    bugFix
                    │
C0───C1───C2───C3───C4──┐
      │                 │
      └───C5────────────┴─C6
                           │
                           main*



# squash merge
# 开始时在main分支
# git merge --squash bugFix
# git commit
# C6包含了bugFix分支的所有修改（C2-C4），合并成一个commit
# author是main分支的，因为做完squash merge还要手动commit
# bugFix分支保持原样，这里没画出来

C0───C1
      │
      └───C5───C6
                │
                main*

# rebase merge
# 开始时在main分支
# 
# git checkout bugFix
# git rebase -i main [1]
# git checkout main
# git merge bugFix [2]
# 
# C6包含了bugFix分支的所有修改（C2-C4），合并成一个commit
# 且由于在进行rebase的时候分支位于bugFix，所以author是bugFix分支的

[1]

C0───C1
      │
      └───C5───C6
           │    │
        main    bugFix*


[2] (未绘制bugFix)

C0───C1
      │
      └───C5───C6
                │
                main*
```

其余可用命令如下。

|                   命令                   |      含义      |
|------------------------------------------|----------------|
| `git diff <source_branch> <target_branch>` | 预览差异       |
| `git add <filename>`                       | 标记为合并成功 |

### 变基

变基是另一种合并分支的方法，实际上就是取出一系列的提交记录，复制它们，然后在另外一个地方逐个的放下去。

```
# 把bugFix分支里的工作直接移到main分支
git rebase main
```

执行以上命令前后提交记录如下。注意，分支应指向最新的提交记录，运行rebase时，Git默认将从两者的公共节点出发，断开本分支与公共节点的连接，并指向指定分支以使其成为父节点。

```
# 执行前
           bugFix*
           │
C0───C1───C3
      │
      └───C2
           │
           main

# 执行后
# bugFix分支断开与公共节点C1的连接，并将其指向main处即C2
C0───C1─ ─C3
      │
      └───C2───C3'
           │    │
        main    bugFix*
```

若bugFix分支的变化与main分支的变化发生冲突，将会提示发生冲突，需要手动处理。处理完成后按照提示，用`git add/rm [文件]`标志为已解决冲突，然后输入以下命令继续变基。

```
git rebase --continue
```
若中途希望终止rebase，则输入以下命令。

```
git rebase --abort
```

经过rebase后的分支推送到远程分支时，需要用`-f`参数强制覆盖远程分支。

注意，若需将main分支移动到bugFix，则运行以下命令。由于bugFix继承自main，因此Git会将main移动到bugFix处。

```
git checkout main
git rebase bugFix
```

执行以上命令前后提交记录如下。

```
# 执行前
C0───C1─ ─C3
      │
      └───C2───C3'
           │    │
       main*    bugFix

# 执行后
C0───C1─ ─C3
      │
      └───C2───C3'
                │
                main*
                bugFix
```

其余可用命令如下。

|                     命令                     |                                含义                                |
|----------------------------------------------|--------------------------------------------------------------------|
| git rebase -i <分支名>                       | 将当前分支移动到分支名所在位置，并交互式调整该分支名以下的提交记录 |
| `git rebase --onto <newbase> <since> <till>` | 把`<since>`（不含）到`<till>`（包含）之间的所有提交嫁接到`<newbase>`上       |
| git rebase --skip                             | 跳过某次提交                                                       |

### 指定分支指向

```
# 将main分支强制指向HEAD
git branch -f main HEAD
```

### 查看分支

```
git branch -a
```

输出示例如下。其中*为当前分支，前面有remotes/origin/的为远程分支，没有路径的为本地分支。

```
  dev
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
```

### 删除分支

```
# 删除本地dev分支
git branch -d dev

# 强行删除分支
# 一般为该分支还没有merge到其他分支时使用
# 删除该分支会丢失分支上的所有更改
git branch -D dev
```

### 从现有远程分支拉取新分支

#### 与远程分支建立映射关系

```
git checkout -b [本地分支名] origin/[远程分支名]
```

#### 不与远程分支建立映射关系

```
git fetch origin [远程分支名]:[本地分支名]
git checkout [本地分支名]
```

## 提交树移动

### HEAD

HEAD指向最后一次提交的结果，一般指向分支名。可通过以下命令查看HEAD的指向。

```
# 查看HEAD指向
cat .git/HEAD

# 若HEAD指向一个引用，则使用此命令
git symbolic-ref HEAD
```

分离的HEAD即为让HEAD指向某个具体的提交记录而非分支名。通过哈希值可指定提交记录，命令如下。哈希值不必全部填写，填写至Git能唯一识别该提交记录时的长度即可。

```
# 让HEAD指向提交记录C1而非分支
git checkout [C1的哈希值]
```

### 相对引用

相对引用符号含义如下。相对引用可连续使用。

|   符号   |               含义               |
|----------|----------------------------------|
| ^        | 向上移动一个提交记录，可连续使用 |
| `~<num>` | 向上移动多个提交记录，如~3       |
| `^<num>` | 移动到第`<num>`个父节点，如^2^2  |

以以下命令为例。

```
git checkout main^
git checkout HEAD^

# 以上两条命令等同于以下一条命令
git checkout main~2
```

执行以上命令前后提交记录如下。

```
# 执行前
C0───C1───C2
           │
           main*

# 执行第一行命令后
C0───C1───C2
      │    │
   HEAD    main

# 执行第二行命令后
C0───C1───C2
 │         │
 HEAD      main
```

对于`^<num>`，需注意此处的`<num>`与`~<num>`的不同，示例如下。

```
git checkout main^2
```

假设C3为C4的第一个父节点，C2为C4的第二个父节点。执行以上命令前后提交记录如下。

```
# 执行前
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
                 │
                 main*

# 执行后
           HEAD
           │
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
                 │
                 main*
```

### 撤销变更

#### reset命令

git reset通过把分支记录回退几个提交记录来实现撤销改动。该命令对本地分支起作用。

```
git reset HEAD~1
```

执行以上命令前后提交记录如下。

```
# 执行前
C0───C1───C2
           │
           main*

# 执行后
C0───C1─ ─C2
      │
      main*
```

其余可用命令如下。`[<commit>]`可以省略，默认为HEAD。

|                  命令                  |                                            含义                                           |
|----------------------------------------|-------------------------------------------------------------------------------------------|
| `git reset [-q][<commit>][-] <paths>` | 不会重置引用和工作区，而是用commit下的文件替换暂存区文件，相当于撤销`git add <paths>`的操作 |
|   git reset[--soft\|--mixed\|--hard\|--merge\|--keep][-q][<commit>]|     会重置引用，但会根据不同的参数从而影响工作区或者暂存区            |

各参数含义如下。

|   参数  |                    含义                    |
|---------|--------------------------------------------|
| --hard  | 工作区、引用、暂存区全部替换为commit       |
| --soft  | 只更改引用，工作区和暂存区不影响           |
| --mixed | 默认参数，只更改引用和暂存区，不影响工作区 |

#### revert命令

git revert用于撤销更改并实现共享。

```
git revert HEAD
```

执行以上命令前后提交记录如下。其中C2'的内容与C1一致。

```
# 执行前
C0───C1───C2
           │
           main*

# 执行后
C0───C1───C2───C2'
                │
                main*
```

## 提交记录管理

### 提取特定提交记录

将提交记录复制到当前分支。

```
git cherry-pick C2 C4
```

执行以上命令前后提交记录如下。

```
# 执行前
                     side
                     │
C0───C1───C2───C3───C4
      │
      └───C5
           │
           main*

# 执行后
                     side
                     │
C0───C1───C2───C3───C4
      │
      └───C5───C2'───C4'
                      │
                      main*
```

注意，对于以下情况，应该使用cherry-pick而不是rebase。

```
# 执行前
          main side
          │    │
C0───C1───C2───C3

# 执行后
          main
          │
C0───C1───C2
      │
      └───C3
          │
          side
```

### 修改先前提交记录

可先通过rebase命令将要修改的提交记录移到最前，完成修改并commit后，再用rebase命令调整为原来的顺序。

也可通过cherry-pick命令将要修改的提交记录单独复制出来，完成修改并commit后，再用cherry-pick命令复制该提交记录以后的提交记录。

修改之前的提交记录可使用`--amend`参数。注意推送的时候需要`-f`参数。

```
git commit --amend
git push -f
```

### 标签

即tag，也称为里程碑，用于标记某个特定的提交记录。该tag不会随着新的提交移动，永久指向该提交记录。可以通过checkout命令切换到指定的tag。

```
git tag V1 C1
```

执行以上命令前后提交记录如下。

```
# 执行前
C0───C1───C2
           │
           main*

# 执行后
C0───C1───C2
      │    │
      v1   main*
```

通过以下命令可查找离指定位置最近的标签，其中`<ref>`为任何能被Git识别成提交记录的引用，不指定则默认为HEAD。--dirty参数可选，表示如果工作区有文件修改，将会显示出来。

```
git describe <ref> [--dirty]
```

输出如下。其中tag表示离ref最近的标签，numCommits表示这个ref与tag相差有多少个提交记录，hash表示给定的ref所表示的提交记录哈希值的前几位。

注意，当ref提交记录上有某个标签时，则只输出标签名称。

```
<tag>_<numCommits>_g<hash>
```

其余可用命令如下。

|                    命令                    |                                     含义                                     |
|--------------------------------------------|------------------------------------------------------------------------------|
| git tag                                    | 显示当前版本库标签列表                                                       |
| `git tag <tagname> [<commit>]`             | 创建轻量级标签                                                               |
| `git tag -a <tagname> [<commit>]`          | 创建带说明的标签                                                             |
| `git tag -m <msg> <tagname> [<commit>]`    | 创建带说明的里程碑，通过-m参数提供里程碑创建说明                             |
| `git tag -s <tagname> [<commit>] `         | 创建带GnuPG签名的标签                                                        |
| `git tag -u <key-id> <tagname> [<commit>]` | 创建带GnuPG签名的标签，通过-m参数提供签名ID                                  |
| git tag -n1                                | 在显示标签的时候同时显示说明，使用`-n<num>`参数，显示最多`<num>`行标签的说明 |
| git tag -l shaofan*                        | 只显示名称和通配符相符的标签                                                 |
| git log --oneline --decorate               | --decorate参数可以看到提交对应的标签及其他引用                               |
| git tag -d tagname                         | 删除本地标签                                                                 |

## 远程仓库管理

### 克隆远程仓库

clone命令在本地仓库增加`<remote name>/<branch name>`分支，其中Git默认将远程仓库命名为origin。

```
# --depth参数可选
# --depth=1即只下载最近一次的版本
git clone [--depth=1] git@github.com:[用户名]/[仓库名].git
```

执行以上命令前后提交记录如下。

```
# 执行前
## 远程端
C0───C1
      │
      main*

# 执行后
## 远程端
C0───C1
      │
      main*
## 本地端
C0───C1
      │
      main*
      origin/main
```

注意，对远程分支进行commit时，Git会进入分离HEAD状态，不会更新origin/main分支。origin/main分支只有在远程仓库中相应的分支更新了以后才会更新。

```
git checkout origin/main
git commit
```

### 从远程仓库获取提交

```
# <place>可以为<destination>或<source>:<destination>
# <source>:<destination>为refspecs，表示Git可识别的位置
# <source>可留空
# <remote>和<place>可省略
git fetch <remote> <place>
```

命令示例如下。

```
# 下载所有的提交记录到本地各个远程分支
# 即origin/<branchname>
git fetch

# 将远程main分支内容下载到本地origin/main分支
git fetch origin main

# 将远程foo父节点对应的分支下载到本地bar分支
# 注意不是本地origin/bar分支，因为bar分支可能在远程仓库中不存在
# 若本地bar分支不存在则自动创建
git fetch origin foo~1:bar

# 将远程空分支下载到本地bar分支
# 由于没有分支，事实上创建了本地bar分支
git fetch origin :bar
```

执行以上命令前后提交记录如下。注意本地的main分支不会被更新。

```
# 执行前
## 远程端
C0───C1───C2───C3
                │
                main*
## 本地端
C0───C1
      │
      main*
      origin/main

# 执行后
## 远程端
C0───C1───C2───C3
                │
                main*
## 本地端
C0───C1───C2───C3
      │         │
      main*     origin/main
```

若希望更新本地分支，可使用fetch命令后使用以下命令之一。

```
git cherry-pick origin/main
# 或
git rebase origin/main
# 或
git merge origin/main
```

他们之间的区别如下。

```
# 执行完git fetch后的本地端
           main*
           │
C0───C1───C2
      │
      └───C3
           │
           origin/main


# cherry-pick
# 将origin/main的内容复制到main后面，并使main指向它

                main*
                │
C0───C1───C2───C3'
      │
      └───C3
           │
           origin/main

# rebase
# 将main的内容全部移到origin/main的后面

C0───C1───C3───C2
           │    │
 origin/main    main*

# merge
# 将main分支和origin/main分支合并

C0───C1───C2──┐
      │       │
      └───C3──┴─C4
           │     │
 origin/main     main*
```

也可以直接使用pull命令。使用pull命令则无需再使用fetch命令。git pull等同于git fetch加上git merge，其参数使用与前面完全一致。

```
# <place>可以为<destination>或<source>:<destination>
# <source>:<destination>为refspecs，表示Git可识别的位置
# <source>可留空
# <remote>和<place>可省略
git pull <remote> <place>
```

命令示例如下。

```
# 将远程main分支内容下载到本地origin/main分支，并完成分支合并
git pull origin main

# 将远程main分支内容下载到本地foo分支，并完成分支合并
# 若本地foo分支不存在则自动创建
git pull origin main:foo
```

执行以上命令前后提交记录如下。

```
# 执行前
## 远程端
C0───C1───C3
           │
           main*
## 本地端
           main*
           │
C0───C1───C2
      │    
      origin/main

# 执行后
## 远程端
C0───C1───C3
           │
           main*
## 本地端
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
           │     │
 origin/main     main*
```

### 提交本地变更

push命令将变更上传到指定的远程仓库，并在远程仓库上合并新提交记录。若分支在远程仓库中不存在，Git将会自动创建。

```
# <place>可以为<destination>或<source>:<destination>
# <source>:<destination>为refspecs，表示Git可识别的位置
# <remote>和<place>可省略
# 不带任何参数时的行为与Git的一个名为push.default的配置有关
# 不带任何参数时，一般为推送HEAD所在分支
git push <remote> <place>
```

命令示例如下。

```
# 将本地main分支推送到远程origin/main分支
git push origin main

# 将本地foo父节点对应的分支推送到远程origin/main分支
git push origin foo^:main

# 将本地空分支推送到远程origin/foo分支
# 由于没有分支，事实上删除了远程origin/foo分支
git push origin :foo
```

执行以上命令前后提交记录如下。

```
# 执行前
## 远程端
C0───C1
      │
      main*
## 本地端
           main*
           │
C0───C1───C2
      │    
origin/main

# 执行后
## 远程端
C0───C1───C2
           │
           main*
## 本地端
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
           │     │
 origin/main     main*
```

### 历史偏离处理

假设当前提交记录如下。此时执行git push将失败，因为C3基于C1，但远程仓库中已经变成了C2，Git无法确定是需要先合并分支还是直接回滚到C1。

```
## 远程端
C0───C1───C2
           │
           main*

## 本地端
           main*
           │
C0───C1───C3
      │    
origin/main
```

此时可先更新本地的远程分支，然后通过rebase命令将C3移到最新的提交记录下，最后再提交即可。

```
git fetch
git rebase origin/main
git push

# git fetch和git rebase可合并为以下命令
git pull --rebase
```

执行以上命令后提交记录如下。

```
## 远程端
C0───C1───C2───C3
                │
                main*
## 本地端
      ┌───C3
      │
C0───C1───C2───C3'
                │ 
                main*
                origin/main
```

也可用merge代替rebase。

```
git fetch
git merge origin/main
git push

# git fetch和git merge可合并为以下命令
git pull
```

执行以上命令后提交记录如下。

```
## 远程端
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
                 │
                 main*

## 本地端
C0───C1───C2──┐
      │       │
      └───C3──┴─C4
                 │
                 main*
                 origin/main
```

若需要用本地分支覆盖远程分支，则需要使用`-f`参数。

```
# 强制推送
# 当前分支只有自己开发时可安全使用
git push -f

# 检查其他人在该分支上是否有提交，然后再强制推送
# 当前分支有多人开发时使用
git push --force-with-lease
```

### 关联远程仓库

```
git remote set-url origin git@github.com:[用户名]/[仓库名].git
```

### 远程追踪分支

#### 新建分支并关联远端仓库

```
# 新建一个新分支totallyNotMain，该分支跟踪远程分支origin/main
git checkout -b totallyNotMain origin/main
```

#### 修改/重新关联远端仓库

```
# 将现有分支totallyNotMain设置为跟踪远程分支origin/main
# 若当前即在totallyNotMain分支，可在以下命令中省略totallyNotMain
git branch -u o/main totallyNotMain
```

以上例子中，原来本地的main分支与远程的origin/main分支关联。执行以上命令后提交记录如下。

```
## 远程端
C0───C1───C2
           │
           main*

## 本地端
C0───C1
      │ 
      main
      totallyNotMain*
      origin/main
```

若执行git pull，则变为如下。注意main分支未被更新，git push同理。

```
## 远程端
C0───C1───C2
           │
           main*

## 本地端
C0───C1───C2
      │    │
   main    totallyNotMain*
           origin/main
```

## 文件管理与改动

### 查看文件状态

```
git status

# 显示精简的状态信息
git status -s
```

### 文件忽略

git文件忽略需要在根目录新建.gitignore文件，作用范围是其所处的目录及其子目录，无法使用add添加。只对未加入跟踪的文件有效。

文件内容示例如下。

```
# 忽略所有以.a为扩展名的文件
**.a

# 但lib.a文件或目录不要忽略
!lib.a

# 只忽略此目录下的TODO文件，子目录的TODO文件不忽略
/TODO

# 忽略所有build/目录下的文件
build/

# 忽略文件如doc/notes.txt，但是文件如doc/server/arch.txt不被忽略
doc/*.txt
```

常用配置如下。

```
# dependencies  npm包文件
/node_modules

# production  打包文件
/build

# misc 
.DS_Store

npm-debug.log*
```

可通过以下命令查看被忽略的文件。

```
git status --ignored -s
```

### 目录显示

```
# 显示现在.git目录所在位置
git rev-parse --git-dir

# 显示工作区根目录
git rev-parse --show-toplevel
```

### 大文件上传

git对于超过100MB的文件默认不能上传，出现`GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com.`错误。

可通过安装git-lfs解决，切换到项目目录并执行以下命令即可。注意无法对安装该工具之前的提交应用该工具。完成安装后按照正常的仓库提交流程进行即可。

```
git lfs install
git lfs track "*.psd" // psd为需要上传的大文件后缀
git add .gitattributes
```

### 用HEAD中的内容替换本地改动

```
git checkout -- <fliename>
```

### 丢弃本地改动并从服务器获取最新版本

```
git fetch origin
git reset --hard origin/master
```

### 查找文件

```
git grep "content"
```

### 查看版本库中的文件列表

```
git ls-files --with-tree=HEAD
```

### 拣选

```
# 显示领先提交（未被推送到上游跟踪分支中）
git cherry
```

### 恢复进度

| 命令                                        | 含义                                                                                                                                                                                               |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| git stash                                 | 保存工作进度，会分别对暂存区和工作区进行保存                                                                                                                                                                           |
| git stash -p                              | 交互式保存工作进度，可以保存部分文件                                                                                                                                                                               |
| git stash list                            | 显示进度列表                                                                                                                                                                                           |
| `git stash pop [--index] [<stash>]`       | 恢复最新一条保存进度，如果提供`<stash>`参数（来自于git stash list显示的列表）则从该`<stash>`中恢复，恢复完毕也将从进度列表中删除`<stash>`。选项--index除了恢复工作区的文件外，还尝试恢复暂存区                                                                          |
| git stash save "message..."               | 保存进度时候指定说明。使用参数--patch会显示工作区和HEAD的差异，使用-k或--keep-index参数在保存进度后不会将暂存区重置                                                                                                                           |
| `git stash drop [<stash>] `               | 删除一个存储的进度，默认删除最新的进度                                                                                                                                                                              |
| `git stash apply [--index] [<stash>]`     | 除了不删除恢复的进度之外，其余和git stash drop命令一样                                                                                                                                                               |
| git stash clear                           | 删除所有存储的进度                                                                                                                                                                                        |
| `git stash branch <branchname> <stash>`   | 基于进度创建分支                                                                                                                                                                                         |

### 反转提交

```
# 将HEAD的提交反向再提交一次
git rever HEAD
```

### 补丁

```
# 最近三次提交生成补丁文件
git format-patch -s HEAD~3..HEAD

# 应用补丁
cat *.patch | git am
```

### 添加子模块

即在本仓库下链接另一个仓库。以下命令均在项目根目录下执行。

```
# 添加
git submodule add [子模块URL] [子模块存储的目录路径]
git commit
git push origin master

# 使用
# 克隆项目后默认子模块目录下无任何内容，需执行如下命令完成子模块的下载
git submodule init
git submodule update
# 以上两行命令可合为以下命令
# git submodule update --init --recursive
```

## 日志管理

### 查看日志

```
git log
```

可加以下参数。

```
--pretty=fuller
--pretty=oneline
```

### 查看操作日志

```
# 显示master分支最近五次操作日志
git reflog show master | head -5

# 显示HEAD分支最近一次操作日志
git reflog -1
```

## 主程序管理

### 配置SSH密钥

配置SSH密钥可在拉取代码时避免输入用户名和密码。终端输入以下命令，一路回车以使用默认配置。

```
ssh-keygen -t rsa -C "[Github邮箱地址]"
```

然后输入以下命令查看生成的SSH密钥。

```
cat ~/.ssh/id_rsa.pub
```

在Github网站上登录自己的账号，然后进入Settings-SSH and GPG keys，点击New SSH Key，将刚才的内容复制进来并确定即可。

### 更新

```
git update-git-for-windows
```

### 彩色git输出

```
git config color.ui true
```

### git配置

```
git config
```

### 设置缓存区

```
# 增加git缓存区大小
git config --global http.postBuffer 2000000000

# 压缩配置
git config --global core.compression -1 

# 修改配置文件
export GIT_TRACE_PACKET=1
export GIT_TRACE=1
export GIT_CURL_VERBOSE=1
```

### 证书认证管理

一般在出现server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none错误时，可尝试关闭证书认证。

```
# 关闭证书认证
git config --global http.sslverify false

# 开启证书认证
git config --global http.sslverify true
```

## 下载加速

国内DNS把Github的域名基本上都解析到了美国的服务器，所以访问起来会比较慢。

### 通过Gitee

打开以下链接并完成注册和登录。然后创建仓库，在新建仓库页选择`导入已有仓库`，复制需要下载的git链接，点击创建，然后下载即可。

```
https://gitee.com/
```

### 通过镜像站

可通过以下网站代替github.com进行访问。

```
https://github.iapk.cc
https://hub.fastgit.org
https://github.bajins.com
https://github.com.cnpmjs.org
https://github.wuyanzheshui.workers.dev
```

也可使用Fast Github，游猴脚本如下。

```
https://greasyfork.org/zh-CN/scripts/397419
```

### 通过代下载

可通过以下网站进行代下载。

```
https://g.widora.cn/
https://www.toolwa.com/github/
https://git.best/
https://g.ioiox.com/
https://shrill-pond-3e81.hunsh.workers.dev/
https://gh.api.99988866.xyz/
https://github.zhlh6.cn/
https://gitclone.com/
http://gitd.cc/
https://githubd.com/#/
https://d.serctl.com/
http://www.toolzl.com/tools/githubjiasu.html
https://github.zhlh6.cn/
```

### 通过插件

#### Chrome插件

可通过`Github加速`插件，下载链接如下。

```
https://fhefh2015.github.io/Fast-GitHub/
https://ltribe.lanzoui.com/i3iAGi1xxdi
https://chrome.google.com/webstore/detail/github%E5%8A%A0%E9%80%9F/mfnkflidjnladnkldfonnaicljppahpg
```

也可使用FasterHosts。

```
https://github.com/gauseen/faster-hosts
```

#### 游猴插件

可通过`Github增强`插件。

```
https://greasyfork.org/scripts/412245
```

### 通过CDN

通过以下链接即可。

```
https://cdn.jsdelivr.net/gh/[Github用户名]/[GitHub仓库名]@[版本号，没版本号可以不填]/file
```

也可将raw.githubusercontent.com替换为raw.staticdn.net。

### 通过自建服务器

#### 部署到Cloudflare Worker

打开以下链接并登录，新建一个Worker。

```
https://workers.cloudflare.com
```

复制以下代码到左侧代码框后进行部署，即可通过Worker所分配的网站进入部署好的Github加速站点。

```
'use strict'

/**
 * static files (404.html, sw.js, conf.js)
 */
const ASSET_URL = 'https://hunshcn.github.io/gh-proxy/'
// 前缀，如果自定义路由为example.com/gh/*，将PREFIX改为 '/gh/'，注意，少一个杠都会错！
const PREFIX = '/'
// git使用cnpmjs镜像、分支文件使用jsDelivr镜像的开关，0为关闭，默认开启
const Config = {
    jsdelivr: 1,
    cnpmjs: 1
}

/** @type {RequestInit} */
const PREFLIGHT_INIT = {
    status: 204,
    headers: new Headers({
        'access-control-allow-origin': '*',
        'access-control-allow-methods': 'GET,POST,PUT,PATCH,TRACE,DELETE,HEAD,OPTIONS',
        'access-control-max-age': '1728000',
    }),
}

/**
 * @param {any} body
 * @param {number} status
 * @param {Object<string, string>} headers
 */
function makeRes(body, status = 200, headers = {}) {
    headers['access-control-allow-origin'] = '*'
    return new Response(body, {status, headers})
}


/**
 * @param {string} urlStr
 */
function newUrl(urlStr) {
    try {
        return new URL(urlStr)
    } catch (err) {
        return null
    }
}


addEventListener('fetch', e => {
    const ret = fetchHandler(e)
        .catch(err => makeRes('cfworker error:\n' + err.stack, 502))
    e.respondWith(ret)
})


/**
 * @param {FetchEvent} e
 */
async function fetchHandler(e) {
    const req = e.request
    const urlStr = req.url
    const urlObj = new URL(urlStr)
    let path = urlObj.searchParams.get('q')
    if (path) {
        return Response.redirect('https://' + urlObj.host + PREFIX + path, 301)
    }
    // cfworker 会把路径中的 `//` 合并成 `/`
    path = urlObj.href.substr(urlObj.origin.length + PREFIX.length).replace(/^https?:\/+/, 'https://')
    const exp1 = /^(?:https?:\/\/)?github\.com\/.+?\/.+?\/(?:releases|archive)\/.*$/i
    const exp2 = /^(?:https?:\/\/)?github\.com\/.+?\/.+?\/(?:blob)\/.*$/i
    const exp3 = /^(?:https?:\/\/)?github\.com\/.+?\/.+?\/(?:info|git-).*$/i
    const exp4 = /^(?:https?:\/\/)?raw\.githubusercontent\.com\/.+?\/.+?\/.+?\/.+$/i
    if (path.search(exp1) === 0 || !Config.cnpmjs && (path.search(exp3) === 0 || path.search(exp4) === 0)) {
        return httpHandler(req, path)
    } else if (path.search(exp2) === 0) {
        if (Config.jsdelivr){
            const newUrl = path.replace('/blob/', '@').replace(/^(?:https?:\/\/)?github\.com/, 'https://cdn.jsdelivr.net/gh')
            return Response.redirect(newUrl, 302)
        }else{
            path = path.replace('/blob/', '/raw/')
            return httpHandler(req, path)
        }
    } else if (path.search(exp3) === 0) {
        const newUrl = path.replace(/^(?:https?:\/\/)?github\.com/, 'https://github.com.cnpmjs.org')
        return Response.redirect(newUrl, 302)
    } else if (path.search(exp4) === 0) {
        const newUrl = path.replace(/(?<=com\/.+?\/.+?)\/(.+?\/)/, '@$1').replace(/^(?:https?:\/\/)?raw\.githubusercontent\.com/, 'https://cdn.jsdelivr.net/gh')
        return Response.redirect(newUrl, 302)
    } else {
        return fetch(ASSET_URL + path)
    }
}


/**
 * @param {Request} req
 * @param {string} pathname
 */
function httpHandler(req, pathname) {
    const reqHdrRaw = req.headers

    // preflight
    if (req.method === 'OPTIONS' &&
        reqHdrRaw.has('access-control-request-headers')
    ) {
        return new Response(null, PREFLIGHT_INIT)
    }

    let rawLen = ''

    const reqHdrNew = new Headers(reqHdrRaw)

    let urlStr = pathname
    if (urlStr.startsWith('github')) {
        urlStr = 'https://' + urlStr
    }
    const urlObj = newUrl(urlStr)

    /** @type {RequestInit} */
    const reqInit = {
        method: req.method,
        headers: reqHdrNew,
        redirect: 'follow',
        body: req.body
    }
    return proxy(urlObj, reqInit, rawLen, 0)
}


/**
 *
 * @param {URL} urlObj
 * @param {RequestInit} reqInit
 */
async function proxy(urlObj, reqInit, rawLen) {
    const res = await fetch(urlObj.href, reqInit)
    const resHdrOld = res.headers
    const resHdrNew = new Headers(resHdrOld)

    // verify
    if (rawLen) {
        const newLen = resHdrOld.get('content-length') || ''
        const badLen = (rawLen !== newLen)

        if (badLen) {
            return makeRes(res.body, 400, {
                '--error': `bad len: ${newLen}, except: ${rawLen}`,
                'access-control-expose-headers': '--error',
            })
        }
    }
    const status = res.status
    resHdrNew.set('access-control-expose-headers', '*')
    resHdrNew.set('access-control-allow-origin', '*')

    resHdrNew.delete('content-security-policy')
    resHdrNew.delete('content-security-policy-report-only')
    resHdrNew.delete('clear-site-data')

    return new Response(res.body, {
        status,
        headers: resHdrNew,
    })
}
```

#### 部署到VPS

连接到服务器并执行以下命令部署docker，其中第一个80表示要暴露的端口。

```
docker run -d --name="gh-proxy-py" -p 0.0.0.0:80:80 --restart=always hunsh/gh-proxy-py:latest
```

通过以下命令安装依赖，注意应当使用python 3。

```
pip install flask requests
```

新建main.py并填写以下内容，其中部分选项根据注释进行修改。修改完成后保存并运行该脚本即可。

```
# -*- coding: utf-8 -*-
import re

import requests
from flask import Flask, Response, redirect, request
from requests.exceptions import (
    ChunkedEncodingError,
    ContentDecodingError, ConnectionError, StreamConsumedError)
from requests.utils import (
    stream_decode_response_unicode, iter_slices)
from urllib3.exceptions import (
    DecodeError, ReadTimeoutError, ProtocolError)

# config
# git使用cnpmjs镜像、分支文件使用jsDelivr镜像的开关，0为关闭，默认开启
jsdelivr = 1
cnpmjs = 1
size_limit = 1024 * 1024 * 1024 * 999  # 允许的文件大小，默认999GB，相当于无限制了 https://github.com/hunshcn/gh-proxy/issues/8
HOST = '127.0.0.1'  # 监听地址，建议监听本地然后由web服务器反代
PORT = 80  # 监听端口
ASSET_URL = 'https://hunshcn.github.io/gh-proxy'  # 主页

app = Flask(__name__)
CHUNK_SIZE = 1024 * 10
index_html = requests.get(ASSET_URL, timeout=10).text
exp1 = re.compile(r'^(?:https?://)?github\.com/.+?/.+?/(?:releases|archive)/.*$')
exp2 = re.compile(r'^(?:https?://)?github\.com/.+?/.+?/(?:blob)/.*$')
exp3 = re.compile(r'^(?:https?://)?github\.com/.+?/.+?/(?:info|git-).*$')
exp4 = re.compile(r'^(?:https?://)?raw\.githubusercontent\.com/.+?/.+?/.+?/.+$')


@app.route('/')
def index():
    if 'q' in request.args:
        return redirect('/' + request.args.get('q'))
    return index_html


def iter_content(self, chunk_size=1, decode_unicode=False):
    """rewrite requests function, set decode_content with False"""

    def generate():
        # Special case for urllib3.
        if hasattr(self.raw, 'stream'):
            try:
                for chunk in self.raw.stream(chunk_size, decode_content=False):
                    yield chunk
            except ProtocolError as e:
                raise ChunkedEncodingError(e)
            except DecodeError as e:
                raise ContentDecodingError(e)
            except ReadTimeoutError as e:
                raise ConnectionError(e)
        else:
            # Standard file-like object.
            while True:
                chunk = self.raw.read(chunk_size)
                if not chunk:
                    break
                yield chunk

        self._content_consumed = True

    if self._content_consumed and isinstance(self._content, bool):
        raise StreamConsumedError()
    elif chunk_size is not None and not isinstance(chunk_size, int):
        raise TypeError("chunk_size must be an int, it is instead a %s." % type(chunk_size))
    # simulate reading small chunks of the content
    reused_chunks = iter_slices(self._content, chunk_size)

    stream_chunks = generate()

    chunks = reused_chunks if self._content_consumed else stream_chunks

    if decode_unicode:
        chunks = stream_decode_response_unicode(chunks, self)

    return chunks


@app.route('/<path:u>', methods=['GET', 'POST'])
def proxy(u):
    u = u if u.startswith('http') else 'https://' + u
    u = u.replace(':/g', '://g', 1)  # uwsgi会将//传递为/
    if jsdelivr and exp2.match(u):
        u = u.replace('/blob/', '@', 1).replace('github.com', 'cdn.jsdelivr.net/gh', 1)
        return redirect(u)
    elif cnpmjs and exp3.match(u):
        u = u.replace('github.com', 'github.com.cnpmjs.org', 1) + request.url.replace(request.base_url, '', 1)
        return redirect(u)
    elif jsdelivr and exp4.match(u):
        u = re.sub(r'(\.com/.*?/.+?)/(.+?/)', r'\1@\2', u, 1)
        u = u.replace('raw.githubusercontent.com', 'cdn.jsdelivr.net/gh', 1)
        return redirect(u)
    else:
        if exp2.match(u):
            u = u.replace('/blob/', '/raw/', 1)
        headers = {}
        r_headers = {}
        for i in ['Range', 'User-Agent']:
            if i in request.headers:
                r_headers[i] = request.headers.get(i)
        r_headers['Accept-Encoding'] = request.headers.get('Accept-Encoding', 'identity')
        try:
            url = u + request.url.replace(request.base_url, '', 1)
            if url.startswith('https:/') and not url.startswith('https://'):
                url = 'https://' + url[7:]
            r = requests.request(method=request.method, url=url, data=request.data, headers=r_headers, stream=True)
            for i in ['Content-Range', 'Content-Type']:
                if i in r.headers:
                    headers[i] = r.headers.get(i)
            if r.status_code == 200:
                headers = dict(r.headers)
            try:
                headers.pop('Transfer-Encoding')
            except KeyError:
                pass

            if 'Content-length' in r.headers and int(r.headers['Content-length']) > size_limit:
                return redirect(u + request.url.replace(request.base_url, '', 1))

            def generate():
                for chunk in iter_content(r, chunk_size=CHUNK_SIZE):
                    yield chunk

            return Response(generate(), headers=headers, status=r.status_code)
        except Exception as e:
            headers['content-type'] = 'text/html; charset=UTF-8'
            return Response('server error ' + str(e), status=500, headers=headers)
    # else:
    #     return Response('Illegal input', status=403, mimetype='text/html; charset=UTF-8')


if __name__ == '__main__':
    app.run(host=HOST, port=PORT)
```

### 通过FastGit

#### 基础使用

各命令替换如下。

对于`https://raw.githubusercontent.com/`，可替换为`https://raw.fastgit.org/`。

```
# 克隆
# 假设仓库为https://github.com/author/repo
git clone https://hub.fastgit.org/author/repo

# Release
# 假设下载链接为https://github.com/A/A/releases/download/1.0/1.0.tar.gz
wget https://download.fastgit.org/A/A/releases/download/1.0/1.0.tar.gz

# Codeload
# 假设下载链接为 https://hub.fastgit.org/A/A/archive/master.zip
# 或者 https://codeload.github.com/A/A/zip/master
wget https://download.fastgit.org/A/A/archive/master.zip
```

#### fgit

可通过安装fgit，在克隆项目时直接完成链接替换。下载以下项目并解压。

```
https://github.com/FastGitORG/fgit
```

完成解压后，对于Linux，终端切换到解压好的目录后执行以下命令。

```
sudo cp ./bin/fgit.sh /usr/local/bin
```

对于Windows则将fgit/scripts加入到环境变量(PATH)中。

完成后即可在终端通过以下命令克隆项目。

```
fgit https://github.com/author/repo
```

#### 油猴脚本

Fastgit油猴脚本链接如下。

```
https://greasyfork.org/zh-CN/scripts/402301-fastgit
```

### 通过Microsoft Azure Notebooks

打开以下链接并登录微软账号。新建一个notebook环境并预装Git，然后使用git或者wget命令将Github资源下载到微软服务器，再从微软下载。

```
https://notebooks.azure.com/
```

### 通过反向代理

可使用以下通过反向代理加速的工具。

```
# dev-sidecar
https://gitee.com/docmirror/dev-sidecar

# FastGithub
https://github.com/dotnetcore/FastGithub

# steamcommunity 302
https://www.dogfight360.com/blog/686/
```

### 通过翻墙

翻墙后查看翻墙工具的本地端口。以1080为例，可通过以下命令设置代理。

```
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'
```

可通过以下命令取消。

```
git config --global --unset https.proxy 'socks5://127.0.0.1:1080'
git config --global --unset http.proxy 'socks5://127.0.0.1:1080'
```

### 通过hosts

#### 自动修改

##### 通过UsbEAm Hosts Editor

下载链接如下。

```
https://www.dogfight360.com/blog/475/
```

##### 通过SwitchHosts!

可通过订阅hosts规则的方式，避免手动更新域名和IP。点击+号，Type选择Remote，URL如下，Auto Refresh建议选择1 hour。

```
# 选择其一即可
https://cdn.jsdelivr.net/gh/ineo6/hosts/hosts
https://raw.hellogithub.com/hosts
```

#### 手动修改

##### 查询IP

通过以下网站查询`global-ssl.fastly.Net`和`github.com`的公网地址，选择一个稳定且延迟较低的IP并记录。注意不要直接在终端ping该网址，因为经常会发生DNS污染。

```
http://tool.chinaz.com/dns/
https://www.ipaddress.com/
```

也可通过以下链接直接获取。

```
https://github.com/ineo6/hosts
```

##### 添加配置

###### 通过直接修改

将以下代码添加到`hosts`文件，其中xxx部分替换为上面所查到的IP地址。

```
// Windows下hosts文件位于C:\Windows\System32\drivers\etc\hosts
// Mac/Linux下hosts文件位于/etc/hosts

xxx.xxx.xxx.xxx github.com
xxx.xxx.xxx.xxx github.global.ssl.fastly.net  
```

重启浏览器，或在终端（命令行）输入以下命令以刷新DNS缓存即可。

```
// Linux/Mac
sudo /etc/init.d/networking restart

// Windows
ipconfig /flushdns
```

###### 通过SwitchHosts!

打开后新建配置文件，填写上面查询到的IP和域名，然后开启该配置文件即可。

### 通过加速器

网易UU加速器可免费加速Github。

```
https://uu.163.com/
```

## 常见问题

### 每次pull代码都要输入密码

打开终端，在项目目录下输入以下命令。

```
git config --global credential.helper store
```

然后pull一次，输入密码即可。注意这里的密码应该是token。

若无效，则先执行以下命令清理之前的配置，再输入以上命令。

```
git gc
git prune
```

# Github

## 分支管理

### 新建分支

在仓库中点击Branch，在搜索框中输入要新建的分支的名字，系统会自动弹出Create branch的窗口，点击即可。

### 设置默认分支

在仓库中点击Settings，在侧栏选择Branches，选择新的默认分支即可。

### Pull Request

即PR。部分仓库不允许直接push，需要通过Pull Request提交更新。对代码进行修改后通知仓库作者并请求作者合并自己的修改，即完成一次Pull Request。

在Github上fork项目，并进行修改。切换到需要PR的分支，点击New pull request即可。

## Github Action

### 准备工作

打开Github，点击Settings-Seveloper settings-Personal access tokens，选择`Generate new token`，勾选`repo`，`admin:repo_hook`，`workflow`后点击`Generate token`即可。

完成Github Action的设置后，可通过随意commit或点击`Star`以激活功能。

### 可用项目

#### 签到与打卡

签到一般需要获取完整Cookie。

若需要完整Cookie，可登录官网后点击F12，进入Network-Doc，刷新网页后点击以名称中以网站为名的选项，右侧即可复制Cookie。

若需要部分Cookie，则进入Application-Cookie，点击其下子项后，即可在右侧看到Cookie的详细内容。

如果有服务器，也可使用青龙部署。

```
https://github.com/whyour/qinglong
```

也可使用已经搭建好的服务。

```
https://qiandao.today/
https://auto.oldwu.top/
http://umini.eu.org/
http://qd.acy.moe/index.php?mod=login
```

##### 天翼云盘

打开以下链接并Fork项目。

```
https://github.com/Wang-KH/Cloud189Checkin-Actions
```

点击Settings-Secrets，添加以下Secrets。

```
名称 / USER
内容 / 账号（可用空格分隔多个账号）

名称 / PWD
内容 / 密码（可用空格分隔多个账号）
```

保存后点击Actions，并开启功能。然后打开README.md，随意编辑后提交更改即可。

##### Sitoi

包括爱奇艺、全民K歌、腾讯视频、有道云笔记、网易云音乐、一加手机社区官方论坛、百度贴吧、Bilibili、V2EX、咔叽网单、什么值得买、AcFun、天翼云盘、WPS、吾爱破解、芒果TV、联通营业厅、Fa米家、小米运动、百度搜索资源平台、每日天气预报、每日一句、哔咔漫画、和彩云、智友邦、微博、CSDN、王者营地。

fork以下项目，并转到Settings-Secrets。

```
https://github.com/Sitoi/dailycheckin
```

内容根据以下文档配置。

需要说明的是，王者营地的数据可用Loon的抓包模式获得，注意当前只支持QQ登录的账号。

```
https://sitoi.github.io/dailycheckin/settings/
https://sitoi.github.io/dailycheckin/github-actions/
```

对于使用Bark推送，填写BARK_URL即可。

对于使用Telegram推送，仅需填写TG_BOT_TOKEN、TG_USER_ID和TG_API_HOST，其中TG_API_HOST为搭建的反向代理地址，可在Cloudflare新建Worker并使用以下代码进行配置。

```
const whitelist = ["/bot"];
const tg_host = "api.telegram.org";
addEventListener('fetch', event => {
event.respondWith(handleRequest(event.request))
})
function validate(path) {
for (var i = 0; i < whitelist.length; i++) {
if (path.startsWith(whitelist[i]))
return true;
}
return false;
}
async function handleRequest(request) {
var u = new URL(request.url);
u.host = tg_host;
if (!validate(u.pathname))
return new Response('Unauthorized', {
status: 403
});
var req = new Request(u, {
method: request.method,
headers: request.headers,
body: request.body
});
const result = await fetch(req);
return result;
}
```

<details>
<summary>【进阶】腾讯云函数部署</summary>

该项目也可使用腾讯云函数部署。打开以下链接，选择云产品-云函数。

```
https://console.cloud.tencent.com/
```

点击函数服务-新建，创建方式选择`自定义创建`，函数类型选择`事件函数`，函数名称及地域任意，部署方式选择`代码部署`，运行环境选择`Python3.6`。

在下方删除默认的代码，并替换为以下内容。

```
# -*- coding: utf-8 -*-
from dailycheckin.main import checkin

def main_handler(event, context):
  checkin()
```

展开`高级配置`，将执行超时时间改为900，其他保持默认。

展开`触发器配置`，切换到自定义创建，定时任务的名称任意，触发周期改为自定义触发周期，在Cron表达式处填入`45 8 * * *`。点击最下方的完成以创建云函数。

创建完成后点击函数管理-函数代码，然后点击终端-新终端，在终端输入以下命令。

```
cd src
pip3 install dailycheckin --upgrade -t .
```

在Cloud Studio中，在src目录下创建config文件夹，在config文件夹下创建config.json文件，文件内容如下，根据需要填入需要签到的账号的Cookie。

```
{
  "DINGTALK_SECRET": "",
  "DINGTALK_ACCESS_TOKEN": "",
  "SCKEY": "",
  "SENDKEY": "",
  "BARK_URL": "",
  "QMSG_KEY": "",
  "QMSG_TYPE": "",
  "TG_BOT_TOKEN": "",
  "TG_USER_ID": "",
  "TG_API_HOST": "",
  "TG_PROXY": "",
  "COOLPUSHSKEY": "",
  "COOLPUSHQQ": true,
  "COOLPUSHWX": true,
  "COOLPUSHEMAIL": true,
  "QYWX_KEY": "",
  "QYWX_CORPID": "",
  "QYWX_AGENTID": "",
  "QYWX_CORPSECRET": "",
  "QYWX_TOUSER": "",
  "PUSHPLUS_TOKEN": "",
  "PUSHPLUS_TOPIC": "",
  "CITY_NAME_LIST": [
    "上海"
  ],
  "MOTTO": true,
  "IQIYI_COOKIE_LIST": [
    {
      "iqiyi_cookie": "__dfp=xxxxxx; QP0013=xxxxxx; QP0022=xxxxxx; QYABEX=xxxxxx; P00001=xxxxxx; P00002=xxxxxx; P00003=xxxxxx; P00007=xxxxxx; QC163=xxxxxx; QC175=xxxxxx; QC179=xxxxxx; QC170=xxxxxx; P00010=xxxxxx; P00PRU=xxxxxx; P01010=xxxxxx; QC173=xxxxxx; QC180=xxxxxx; P00004=xxxxxx; QP0030=xxxxxx; QC006=xxxxxx; QC007=xxxxxx; QC008=xxxxxx; QC010=xxxxxx; nu=xxxxxx; __uuid=xxxxxx; QC005=xxxxxx;"
    },
    {
      "iqiyi_cookie": "多账号 cookie 填写，请参考上面，cookie 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）"
    }
  ],
  "VQQ_COOKIE_LIST": [
    {
      "auth_refresh": "https://access.video.qq.com/user/auth_refresh?vappid=xxxxxx&vsecret=xxxxxx&type=qq&g_tk=&g_vstk=xxxxxx&g_actk=xxxxxx&callback=xxxxxx&_=xxxxxx",
      "vqq_cookie": "pgv_pvid=xxxxxx; pac_uid=xxxxxx; RK=xxxxxx; ptcz=xxxxxx; tvfe_boss_uuid=xxxxxx; video_guid=xxxxxx; video_platform=xxxxxx; pgv_info=xxxxxx; main_login=xxxxxx; vqq_access_token=xxxxxx; vqq_appid=xxxxxx; vqq_openid=xxxxxx; vqq_vuserid=xxxxxx; vqq_refresh_token=xxxxxx; login_time_init=xxxxxx; uid=xxxxxx; vqq_vusession=xxxxxx; vqq_next_refresh_time=xxxxxx; vqq_login_time_init=xxxxxx; login_time_last=xxxxxx;"
    },
    {
      "auth_refresh": "多账号 refresh url，请参考上面，以实际获取为准",
      "vqq_cookie": "多账号 cookie 填写，请参考上面，cookie 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）"
    }
  ],
  "YOUDAO_COOKIE_LIST": [
    {
      "youdao_cookie": "JSESSIONID=xxxxxx; __yadk_uid=xxxxxx; OUTFOX_SEARCH_USER_ID_NCOO=xxxxxx; YNOTE_SESS=xxxxxx; YNOTE_PERS=xxxxxx; YNOTE_LOGIN=xxxxxx; YNOTE_CSTK=xxxxxx; _ga=xxxxxx; _gid=xxxxxx; _gat=xxxxxx; PUBLIC_SHARE_18a9dde3de846b6a69e24431764270c4=xxxxxx;"
    },
    {
      "youdao_cookie": "多账号 cookie 填写，请参考上面，cookie 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）"
    }
  ],
  "KGQQ_COOKIE_LIST": [
    {
      "kgqq_cookie": "muid=xxxxxx; uid=xxxxxx; userlevel=xxxxxx; openid=xxxxxx; openkey=xxxxxx; opentype=xxxxxx; qrsig=xxxxxx; pgv_pvid=xxxxxx;"
    },
    {
      "kgqq_cookie": "多账号 cookie 填写，请参考上面，cookie 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）"
    }
  ],
  "MUSIC163_ACCOUNT_LIST": [
    {
      "music163_phone": "18888xxxxxx",
      "music163_password": "Sitoi"
    },
    {
      "music163_phone": "多账号 手机号",
      "music163_password": "多账号 密码"
    }
  ],
  "ONEPLUSBBS_COOKIE_LIST": [
    {
      "oneplusbbs_cookie": "acw_tc=xxxxxx; qKc3_0e8d_saltkey=xxxxxx; qKc3_0e8d_lastvisit=xxxxxx; bbs_avatar=xxxxxx; qKc3_0e8d_sendmail=xxxxxx; opcid=xxxxxx; opcct=xxxxxx; oppt=xxxxxx; opsid=xxxxxx; opsct=xxxxxx; opbct=xxxxxx; UM_distinctid=xxxxxx; CNZZDATA1277373783=xxxxxx; www_clear=xxxxxx; ONEPLUSID=xxxxxx; qKc3_0e8d_sid=xxxxxx; bbs_uid=xxxxxx; bbs_uname=xxxxxx; bbs_grouptitle=xxxxxx; opuserid=xxxxxx; bbs_sign=xxxxxx; bbs_formhash=xxxxxx; qKc3_0e8d_ulastactivity=xxxxxx; opsertime=xxxxxx; qKc3_0e8d_lastact=xxxxxx; qKc3_0e8d_checkpm=xxxxxx; qKc3_0e8d_noticeTitle=xxxxxx; optime_browser=xxxxxx; opnt=xxxxxx; opstep=xxxxxx; opstep_event=xxxxxx; fp=xxxxxx;"
    },
    {
      "oneplusbbs_cookie": "多账号 cookie 填写，请参考上面，cookie 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）"
    }
  ],
  "BAIDU_URL_SUBMIT_LIST": [
    {
      "data_url": "https://cdn.jsdelivr.net/gh/Sitoi/Sitoi.github.io/baidu_urls.txt",
      "submit_url": "http://data.zz.baidu.com/urls?site=https://sitoi.cn&token=xxxxxx",
      "times": 10
    },
    {
      "data_url": "多账号 data_url 链接地址，以实际获取为准",
      "submit_url": "多账号 submit_url 链接地址，以实际获取为准",
      "times": 10
    }
  ],
  "FMAPP_ACCOUNT_LIST": [
    {
      "fmapp_token": "xxxxxx.xxxxxx-xxxxxx-xxxxxx.xxxxxx-xxxxxx",
      "fmapp_cookie": "sensorsdata2015jssdkcross=xxxxxx",
      "fmapp_blackbox": "eyJlcnJxxxxxx",
      "fmapp_device_id": "xxxxxx-xxxx-xxxx-xxxx-xxxxxx",
      "fmapp_fmversion": "xxxxxx",
      "fmapp_os": "xxxxxx",
      "fmapp_useragent": "xxxxxx"
    },
    {
      "fmapp_token": "多账号 token 填写，请参考上面，以实际获取为准",
      "fmapp_cookie": "多账号 cookie 填写，请参考上面，cookie 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）",
      "fmapp_blackbox": "多账号 blackbox 填写，请参考上面，blackbox 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）",
      "fmapp_device_id": "多账号 device_id 填写，请参考上面，以实际获取为准",
      "fmapp_fmversion": "多账号 fmVersion 填写，请参考上面，以实际获取为准",
      "fmapp_os": "多账号 os 填写，请参考上面，以实际获取为准",
      "fmapp_useragent": "多账号 User-Agent 填写，请参考上面，以实际获取为准"
    }
  ],
  "TIEBA_COOKIE_LIST": [
    {
      "tieba_cookie": "BIDUPSID=xxxxxx; PSTM=xxxxxx; BAIDUID=xxxxxx; BAIDUID_BFESS=xxxxxx; delPer=xxxxxx; PSINO=xxxxxx; H_PS_PSSID=xxxxxx; BA_HECTOR=xxxxxx; BDORZ=xxxxxx; TIEBA_USERTYPE=xxxxxx; st_key_id=xxxxxx; BDUSS=xxxxxx; BDUSS_BFESS=xxxxxx; STOKEN=xxxxxx; TIEBAUID=xxxxxx; ab_sr=xxxxxx; st_data=xxxxxx; st_sign=xxxxxx;"
    },
    {
      "tieba_cookie": "多账号 cookie 填写，请参考上面，cookie 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）"
    }
  ],
  "BILIBILI_COOKIE_LIST": [
    {
      "bilibili_cookie": "_uuid=xxxxxx; rpdid=xxxxxx; LIVE_BUVID=xxxxxx; PVID=xxxxxx; blackside_state=xxxxxx; CURRENT_FNVAL=xxxxxx; buvid3=xxxxxx; fingerprint3=xxxxxx; fingerprint=xxxxxx; buivd_fp=xxxxxx; buvid_fp_plain=xxxxxx; DedeUserID=xxxxxx; DedeUserID__ckMd5=xxxxxx; SESSDATA=xxxxxx; bili_jct=xxxxxx; bsource=xxxxxx; finger=xxxxxx; fingerprint_s=xxxxxx;",
      "coin_num": 0,
      "coin_type": 1,
      "silver2coin": true
    },
    {
      "bilibili_cookie": "多账号 cookie 填写，请参考上面，cookie 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）",
      "coin_num": 0,
      "coin_type": 1,
      "silver2coin": true
    }
  ],
  "V2EX_COOKIE_LIST": [
    {
      "v2ex_cookie": "_ga=xxxxxx; __cfduid=xxxxxx; PB3_SESSION=xxxxxx; A2=xxxxxx; V2EXSETTINGS=xxxxxx; V2EX_REFERRER=xxxxxx; V2EX_LANG=xxxxxx; _gid=xxxxxx; V2EX_TAB=xxxxxx;",
      "v2ex_proxy": "使用代理的信息，无密码例子: http://127.0.0.1:1080 有密码例子: http://username:password@127.0.0.1:1080"
    },
    {
      "v2ex_cookie": "多账号 cookie 填写，请参考上面，cookie 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）",
      "v2ex_proxy": "使用代理的信息，无密码例子: http://127.0.0.1:1080 有密码例子: http://username:password@127.0.0.1:1080"
    }
  ],
  "WWW2NZZ_COOKIE_LIST": [
    {
      "www2nzz_cookie": "YPx9_2132_saltkey=xxxxxx; YPx9_2132_lastvisit=xxxxxx; YPx9_2132_sendmail=xxxxxx; YPx9_2132_con_request_uri=xxxxxx; YPx9_2132_sid=xxxxxx; YPx9_2132_client_created=xxxxxx; YPx9_2132_client_token=xxxxxx; YPx9_2132_ulastactivity=xxxxxx; YPx9_2132_auth=xxxxxx; YPx9_2132_connect_login=xxxxxx; YPx9_2132_connect_is_bind=xxxxxx; YPx9_2132_connect_uin=xxxxxx; YPx9_2132_stats_qc_login=xxxxxx; YPx9_2132_checkpm=xxxxxx; YPx9_2132_noticeTitle=xxxxxx; YPx9_2132_nofavfid=xxxxxx; YPx9_2132_lastact=xxxxxx;"
    },
    {
      "www2nzz_cookie": "多账号 cookie 填写，请参考上面，cookie 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）"
    }
  ],
  "SMZDM_COOKIE_LIST": [
    {
      "smzdm_cookie": "__jsluid_s=xxxxxx; __ckguid=xxxxxx; device_id=xxxxxx; homepage_sug=xxxxxx; r_sort_type=xxxxxx; _zdmA.vid=xxxxxx; sajssdk_2015_cross_new_user=xxxxxx; sensorsdata2015jssdkcross=xxxxxx; footer_floating_layer=xxxxxx; ad_date=xxxxxx; ad_json_feed=xxxxxx; zdm_qd=xxxxxx; sess=xxxxxx; user=xxxxxx; _zdmA.uid=xxxxxx; smzdm_id=xxxxxx; userId=xxxxxx; bannerCounter=xxxxxx; _zdmA.time=xxxxxx;"
    },
    {
      "smzdm_cookie": "多账号 cookie 填写，请参考上面，cookie 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）"
    }
  ],
  "MIMOTION_ACCOUNT_LIST": [
    {
      "mimotion_phone": "18888xxxxxx",
      "mimotion_password": "Sitoi",
      "mimotion_min_step": "10000",
      "mimotion_max_step": "20000"
    },
    {
      "mimotion_phone": "多账号 手机号填写，请参考上面",
      "mimotion_password": "多账号 密码填写，请参考上面",
      "mimotion_min_step": "多账号 最小步数填写，请参考上面",
      "mimotion_max_step": "多账号 最大步数填写，请参考上面"
    }
  ],
  "ACFUN_ACCOUNT_LIST": [
    {
      "acfun_phone": "18888xxxxxx",
      "acfun_password": "Sitoi"
    },
    {
      "acfun_phone": "多账号 手机号填写，请参考上面",
      "acfun_password": "多账号 密码填写，请参考上面"
    }
  ],
  "CLOUD189_ACCOUNT_LIST": [
    {
      "cloud189_phone": "18888xxxxxx",
      "cloud189_password": "Sitoi"
    },
    {
      "cloud189_phone": "多账号 手机号填写，请参考上面",
      "cloud189_password": "多账号 密码填写，请参考上面"
    }
  ],
  "POJIE_COOKIE_LIST": [
    {
      "pojie_cookie": "htVD_2132_client_token=xxxxxx; htVD_2132_connect_is_bind=xxxxxx; htVD_2132_connect_uin=xxxxxx; htVD_2132_nofavfid=xxxxxx; htVD_2132_smile=xxxxxx; Hm_lvt_46d556462595ed05e05f009cdafff31a=xxxxxx; htVD_2132_saltkey=xxxxxx; htVD_2132_lastvisit=xxxxxx; htVD_2132_client_created=xxxxxx; htVD_2132_auth=xxxxxx; htVD_2132_connect_login=xxxxxx; htVD_2132_home_diymode=xxxxxx; htVD_2132_visitedfid=xxxxxx; htVD_2132_viewid=xxxxxx; KF4=xxxxxx; htVD_2132_st_p=xxxxxx; htVD_2132_lastcheckfeed=xxxxxx; htVD_2132_sid=xxxxxx; htVD_2132_ulastactivity=xxxxxx; htVD_2132_noticeTitle=xxxxxx;"
    },
    {
      "pojie_cookie": "多账号 cookie 填写，请参考上面，cookie 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）"
    }
  ],
  "MGTV_PARAMS_LIST": [
    {
      "mgtv_params": "uuid=xxxxxx&uid=xxxxxx&ticket=xxxxxx&token=xxxxxx&device=iPhone&did=xxxxxx&deviceId=xxxxxx&appVersion=6.8.2&osType=ios&platform=iphone&abroad=0&aid=xxxxxx&nonce=xxxxxx&timestamp=xxxxxx&appid=xxxxxx&type=1&sign=xxxxxx&callback=xxxxxx"
    },
    {
      "mgtv_params": "多账号 请求参数填写，请参考上面"
    }
  ],
  "PICACOMIC_ACCOUNT_LIST": [
    {
      "picacomic_email": "Sitoi",
      "picacomic_password": "xxxxxx"
    },
    {
      "picacomic_email": "多账号 账号填写，请参考上面",
      "picacomic_password": "多账号 密码填写，请参考上面"
    }
  ],
  "MEIZU_COOKIE_LIST": [
    {
      "meizu_cookie": "aliyungf_tc=xxxxxx; logined_uid=xxxxxx; acw_tc=xxxxxx; LT=xxxxxx; MZBBS_2132_saltkey=xxxxxx; MZBBS_2132_lastvisit=xxxxxx; MZBBSUC_2132_auth=xxxxxx; MZBBSUC_2132_loginmember=xxxxxx; MZBBSUC_2132_ticket=xxxxxx; MZBBS_2132_sid=xxxxxx; MZBBS_2132_ulastactivity=xxxxxx; MZBBS_2132_auth=xxxxxx; MZBBS_2132_loginmember=xxxxxx; MZBBS_2132_lastcheckfeed=xxxxxx; MZBBS_2132_checkfollow=xxxxxx; MZBBS_2132_lastact=xxxxxx;",
      "draw_count": "1"
    },
    {
      "meizu_cookie": "多账号 cookie 填写，请参考上面，cookie 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）",
      "draw_count": "多账号 抽奖次数设置"
    }
  ],
  "ZHIYOO_COOKIE_LIST": [
    {
      "zhiyoo_cookie": "ikdQ_9242_saltkey=xxxxxx; ikdQ_9242_lastvisit=xxxxxx; ikdQ_9242_onlineusernum=xxxxxx; ikdQ_9242_sendmail=1; ikdQ_9242_seccode=xxxxxx; ikdQ_9242_ulastactivity=xxxxxx; ikdQ_9242_auth=xxxxxx; ikdQ_9242_connect_is_bind=xxxxxx; ikdQ_9242_nofavfid=xxxxxx; ikdQ_9242_checkpm=xxxxxx; ikdQ_9242_noticeTitle=1; ikdQ_9242_sid=xxxxxx; ikdQ_9242_lip=xxxxxx; ikdQ_9242_lastact=xxxxxx"
    },
    {
      "zhiyoo_cookie": "多账号 cookie 填写，请参考上面，cookie 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）"
    }
  ],
  "WEIBO_COOKIE_LIST": [
    {
      "weibo_show_url": "https://api.weibo.cn/2/users/show?wm=xxxxxx&launchid=xxxxxx&b=xxxxxx&from=xxxxxx&c=xxxxxx&networktype=xxxxxx&v_p=xxxxxx&skin=xxxxxx&v_f=xxxxxx&lang=xxxxxx&sflag=xxxxxx&ua=xxxxxx&ft=xxxxxx&aid=xxxxxx&has_extend=xxxxxx&uid=xxxxxx&gsid=xxxxxx&sourcetype=&get_teenager=xxxxxx&s=xxxxxx&has_profile=xxxxxx"
    },
    {
      "weibo_show_url": "多账号 show_url 填写，请参考上面，show_url 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）"
    }
  ],
  "DUOKAN_COOKIE_LIST": [
    {
      "duokan_cookie": "user_id=xxxxxx; token=xxxxxx; user_gender=xxxxxx; device_id=xxxxxx; app_id=xxxxxx; build=xxxxxx; short_version=xxxxxx"
    },
    {
      "duokan_cookie": "多账号 cookie 填写，请参考上面，cookie 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）"
    }
  ],
  "CSDN_COOKIE_LIST": [
    {
      "csdn_cookie": "uuid_tt_dd=xxxxxx; _ga=xxxxxx; UserName=xxxxxx; UserInfo=xxxxxx; UserToken=xxxxxx; UserNick=xxxxxx; AU=768; UN=xxxxxx; BT=xxxxxx; p_uid=xxxxxx; Hm_up_6bcd52f51e9b3dce32bec4a3997715ac=xxxxxx; Hm_ct_6bcd52f51e9b3dce32bec4a3997715ac=xxxxxx; Hm_lvt_6bcd52f51e9b3dce32bec4a3997715ac=xxxxxx dc_sid=xxxxxx; c_segment=xxxxxx; dc_session_id=xxxxxx; csrfToken=xxxxxx; c_first_ref=xxxxxx; c_first_page=xxxxxx; c_page_id=xxxxxx; announcement-new=xxxxxx; log_Id_click=xxxxxx; c_pref=xxxxxx; c_ref=xxxxxx; dc_tos=xxxxxx; log_Id_pv=xxxxxx; log_Id_view=xxxxxx"
    },
    {
      "csdn_cookie": "多账号 cookie 填写，请参考上面，cookie 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）"
    }
  ],
  "WZYD_DATA_LIST": [
    {
      "wzyd_data": "areaId=xxxxxx&roleId=xxxxxx&gameId=xxxxxx&serverId=xxxxxx&gameOpenid=xxxxxx&userId=xxxxxx&appVersion=xxxxxx&cClientVersionName=xxxxxx&platid=xxxxxx&source=xxxxxx&algorithm=xxxxxx&version=xxxxxx&timestamp=xxxxxx&appid=xxxxxx&openid=xxxxxx&sig=xxxxxx&encode=2&msdkEncodeParam=xxxxxx&cSystem=xxxxxx&h5Get=xxxxxx&msdkToken=&appOpenid=xxxxxx"
    },
    {
      "wzyd_data": "多账号 data 填写，请参考上面，data 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）"
    }
  ],
  "WOMAIL_URL_LIST": [
    {
      "womail_url": "https://nyan.mail.wo.cn/cn/sign/index/index?mobile=xxxxxx&userName=&openId=xxxxxx"
    },
    {
      "womail_url": "多账号 url 填写，请参考上面，url 以实际获取为准（遇到特殊字符如双引号\" 请加反斜杠转义）"
    }
  ]
}
```

该文件具体说明如下。

```
https://sitoi.github.io/dailycheckin/settings/
```

填写完成后点击下面的`测试`，显示测试成功后再点击`部署`即可。
</details>

##### BlueSkyClouds

包括爱奇艺、腾讯视频、哔哩哔哩、电信、V2ex、哔咔漫画、百度贴吧。

```
https://github.com/BlueSkyClouds/My-Actions
```

Secrets获取方式如下。对于使用Telegram推送，仅需填写TG_BOT_TOKEN和TG_USER_ID。

|   应用   |     Name     |                                                      Value                                                      |
|----------|--------------|-----------------------------------------------------------------------------------------------------------------|
| 爱奇艺   | IQIYI_COOKIE | 电脑打开爱奇艺官网并登录后，按F12开启控制台，点击Network后刷新，搜索authcookie并点开，复制authcookie=后面的部分 |
| WPS      | WPS_KEY      | 电脑打开https://open.wps.cn/，登录后取Cookie中的wps_sid=部分                                                    |
| 腾讯视频 | V_REF_URL    | 电脑打开腾讯视频官网并登录后，按F12开启控制台，点击Network后刷新，搜索auth并点开第三个，复制整段Request URL     |
| 腾讯视频 | V_COOKIE     | 同上，搜索auth_refresh并点开第三个，复制整段Cookie                                                              |
| 百度贴吧 | BDUSS        | 电脑打开百度贴吧官网，登录账号后打开控制台，点击Application-Cookies下的子项，右侧找到BDUSS并复制其内容          |

##### fashionzzZ

包括V2EX、百度贴吧、京东、什么值得买、CSDN、网易云游戏。

```
https://github.com/fashionzzZ/sign-actions
```

##### mkdir700

包括BiliBili抽奖、葫芦侠签到、吾爱破解论坛打卡、EduCoder签到、学习通-健康上报、学习通-课堂签到、小One易统计打卡。

```
https://github.com/mkdir700/sign_in
```

##### mengshouer

包括天翼云盘每日签到、最终幻想14积分商城签到、什么值得买网页每日签到、52pojie每日签到、网易云音乐每日签到、有道云笔记签到、V2EX签到、恩山论坛签到、智友邦签到。

```
https://github.com/mengshouer/CheckinBox
```

##### Dingugu

爱奇艺、腾讯视频、芒果tv、网易云音乐、天翼网盘、52破解论坛、精易论坛、乐易论坛签到。

```
https://github.com/Dingugu/SCF_Sign
```

##### 原神签到小助手

米游社原神每日签到、微博超话签到、原神超话监测和领码。

```
https://github.com/y1ndan/genshinhelper
https://www.yindan.me/tutorial/genshin-impact-helper.html
```

##### 完美校园健康打卡

```
https://github.com/FNDHSTD/WanxiaoHealthyCheckOnTencentCloud
http://blog.renzexuan.com/index.php/archives/3/
```

##### V2EX

```
https://github.com/Wenmoux/V2ex-Auto-Sign
```

##### Hostloc论坛

```
https://github.com/inkuang/hostloc-auto-get-points
```

##### 百度贴吧

```
https://github.com/srcrs/TiebaSignIn
```

##### oshwhub社区

```
https://github.com/seishinkouki/oshwhub_autosign
```

##### Hao4K论坛

```
https://github.com/AlanLang/hao4k-auto-sign-in
```

##### 鱼塘热榜

```
https://github.com/Pooc-J/auto-checkin
```

##### 学习通

```
https://github.com/mzdluo123/chaoxing_action
```

##### 超星学习通

```
https://github.com/yuban10703/chaoxingsign
https://yuban10703.xyz/archives/527
```

##### 咪咕爱看

```
https://github.com/QikaiXu/migu-sign
```

##### 时光相册

```
https://github.com/winterssy/EverPhotoCheckin
```

##### 京东

```
https://github.com/xcqlucky/JD_Sign_Action
https://github.com/lukesyy/jd_yun
```

##### 网易云音乐

```
https://github.com/secriy/CloudMusic-LevelUp
```

<details>
<summary>【进阶】腾讯云函数部署</summary>

该项目也可使用腾讯云函数部署。打开以上链接，下载仓库并解压。进入云函数管理台，新建云函数，运行环境必须为python 3，函数代码提交方法选择本地上传文件夹，选择刚才解压好的文件夹。将高级配置中的环境配置-内存改为64MB，执行超时时间改为900，其它保持默认，点击创建。

完成后进入函数管理页面，在在线IDE中打开一个终端并输入以下命令安装依赖。

```
cd src/ && pip3 install -r requirements.txt -t .
```

然后修改index.py文件，将其中的infos变量各值修改为脚本所需参数，完成后点击部署。然后进入触发管理，点击创建触发器，新建一个触发器以实现定时运行。
</details>

##### 哔哩哔哩

```
https://github.com/yanghaa/BiliBiliTask_rebase
```

##### 平安复旦

```
https://github.com/yew/fudanDaily
```

## 账号管理

### 关闭双重身份验证

网页登录github账号后，点击Settings-Account Security，在Two-factor Authentication一栏点击Disable即可。

### 获取Token

网页登录github账号后，点击Settings-Developer settings-Personal access tokens，点击Generate Token即可。注意生成后需马上复制该Token，否则第二次将无法复制。

## 仓库管理

### 被禁用后删除

不能自行删除，需要联系工作人员。

## 仓库收集

### chinese-independent-developer

开源项目合集。

```
https://github.com/1c7/chinese-independent-developer
```

### SingleFile

将网页保存为HTML文件。

```
https://github.com/gildas-lormeau/SingleFile
```

### Handright

模拟中文手写的轻量级Python库。

```
https://github.com/Gsllchb/Handright
```

### CC-attack

进行DDOS攻击。

```
https://github.com/Leeon123/CC-attack
```

### manim

3b1b使用的数学绘图库。

```
https://github.com/3b1b/manim
```

### wkhtmltopdf

将HTML渲染为PDF和各种图像格式的命令行工具。

```
https://github.com/wkhtmltopdf/wkhtmltopdf
https://wkhtmltopdf.org/
```

### RW_Password

提取收集以往泄露的密码中符合条件的强弱密码。

```
https://github.com/r35tart/RW_Password
```

### raspberry-pi-sync

在树莓派上安装Resilio Sync。

```
https://github.com/willjasen/raspberry-pi-sync
```

### WechatMomentScreenshot

朋友圈转发截图生成工具。

```
https://akarin.dev/WechatMomentScreenshot/
https://github.com/TransparentLC/WechatMomentScreenshot
```

### croc

任意两台电脑互传文件。

```
https://github.com/schollz/croc
```

# Github Gist

随时编写代码并记录。可设为仅自己可见即secret，或全体可见即public。

```
https://gist.github.com/
```

可通过Lepton管理自己的Github Gist。

```
https://github.com/hackjutsu/Lepton
```

# 工具

可使用工具进行Git的图形化操作。

## Sourcetree

完全免费。

```
https://www.sourcetreeapp.com/
```

## GitKraken

### 下载与破解

GitKraren从6.5.1版本开始收费。可通过下载6.5.0及更早版本以免费使用，或下载7.0.1及以前版本并通过GitCracken破解。

下载链接如下，改版本号即可下载特定的旧版本。

```
# Linux-deb
https://release.axocdn.com/linux/GitKraken-v7.5.1.deb

# Linux-rpm
https://release.axocdn.com/linux/GitKraken-v7.5.1.rpm

# Linux-tar.gz
https://release.axocdn.com/linux/GitKraken-v7.5.1.tar.gz

# Win64
https://release.axocdn.com/win64/GitKrakenSetup-7.5.1.exe

# Mac
https://release.axocdn.com/darwin/GitKraken-v7.5.1.zip
```

以Mac为例，若下载了6.5.2-7.0.1版本，需要先打开一次软件，关闭后在终端输入以下命令破解。执行前需要先下载yarn和node，其中yarn可以通过Homebrew下载，node可通过nvm下载。

```
git clone https://gitcode.net/mirrors/5cr1pt/GitCracken.git
cd GitCracken/GitCracken/
yarn install
yarn build
node dist/bin/gitcracken.js patcher
```

完成后在Hosts文件添加以下内容以屏蔽更新。

```
0.0.0.0 release.gitkraken.com
0.0.0.0 api.gitkraken.com
0.0.0.0 gloapi.gitkraken.com
```

## GitExtensions

仅适用于Windows。

```
http://gitextensions.github.io/
```

## Fork

收费。

```
https://git-fork.com/
```

## SmartGit

收费。

```
https://www.syntevo.com/smartgit/download/
```

## GitX

仅适用于Mac 10.5-10.6。

```
http://gitx.frim.nl/index.html
```

# 参考教程

## gitbook

```
https://git-scm.com/book/zh/v2
```

## git的基本用法

```
https://www.cnblogs.com/manjin666/p/9634324.html
```

## GIT命令汇总

```
http://shaofan.org/git-2/
```

## Git Large File Storage

```
https://git-lfs.github.com/
```

## .gitignore

```
https://www.jianshu.com/p/699ed86028c2
```

## hunshcn/gh-proxy

```
https://github.com/hunshcn/gh-proxy
```

## FastGit简体中文指南

```
https://doc.fastgit.org/zh-cn/
```

## 如何提高国内访问Github的速度到2MB/s以上 | 评布客博客

```
https://code.seniortesting.club/blog/2020/How-To-Speed-Github.html
```

## Learn Git Branching

```
https://learngitbranching.js.org/?locale=zh_CN
```

## Pull Request 的命令行管理

```
http://www.ruanyifeng.com/blog/2017/07/pull_request.html
```

## 如何解决类似 curl: (7) Failed to connect to raw.githubusercontent.com port 443: Connection refused 的问题 #10

```
https://github.com/hawtim/blog/issues/10
```

## 如何获取爱奇艺authcookie，并签到

```
https://www.bilibili.com/read/cv7437179
```

## 如何获取百度BDUSS

```
https://jingyan.baidu.com/article/95c9d20d073afbec4e7561d4.html
```

## 无需动手每天就能签到，这招没多少人知道！

```
https://mp.weixin.qq.com/s/RDpjSNfx1i0H_Rz8THDYdw
```

## 腾讯的良心服务，趁没收费赶快来白嫖！

```
https://mp.weixin.qq.com/s/9ihXeVvRaAAluKOfazK-LA
```

## 腾讯云函数部署CloudMusic-LevelUp脚本

```
https://blog.secriy.com/2021/06/12/cktskyuao0031rcnwgd7vc7tl/
```

## 彻底搞懂 Git-Rebase

```
http://jartto.wang/2018/12/11/git-rebase/
```

## 解决git rebase操作后推送远端分支不成功的问题

```
https://blog.csdn.net/ManyPeng/article/details/81095744
```

## sourcetree 拉取 一直让输入密码

```
https://www.cnblogs.com/jcz1206/p/10818427.html
```

## Git 的 commit message 写错了，有办法进行修改么？

```
https://segmentfault.com/q/1010000000761908
```

## merge squash 和 merge rebase 区别

```
https://www.jianshu.com/p/684a8ae9dcf1
https://liqiang.io/post/difference-between-merge-squash-and-rebase
```

## git拉取远程分支并创建本地分支

```
https://blog.csdn.net/tterminator/article/details/52225720
```

## 【Git学习笔记】强行删除还没有merge的分支

```
https://blog.csdn.net/liuchunming033/article/details/40823549
```

## git中submodule子模块的添加、使用和删除

```
https://blog.csdn.net/guotianqing/article/details/82391665
```

## server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none

```
https://www.cnblogs.com/stone1989/p/7000406.html
```

## git clone的时候提示输入密码

```
https://www.cnblogs.com/wscw/p/14118675.html
```

## How do I delete a forked GitHub repo that's unavailable due to DCMA takedown?

```
https://stackoverflow.com/questions/25354045/how-do-i-delete-a-forked-github-repo-thats-unavailable-due-to-dcma-takedown
```

## GitKraKen 7.5.1｜6.5.0 - 安装包&破解

```
https://blog.csdn.net/wanzheng_96/article/details/104692476
```

## 技术/破解最新版GitKraken 7.x.ipynb · Retrying/Modules-Learn - Gitee.com

```
https://gitee.com/wanzheng_96/Modules-Learn/blob/master/%E6%8A%80%E6%9C%AF/%E7%A0%B4%E8%A7%A3%E6%9C%80%E6%96%B0%E7%89%88GitKraken%207.x.ipynb
```

## 上万良心软件都在GitHub，你却还卡在无法访问？

```
https://mp.weixin.qq.com/s/C5XUgzL3_KGIga2X-LHJuw
```

## 禁用个人帐户的双重身份验证

```
https://docs.github.com/cn/authentication/securing-your-account-with-two-factor-authentication-2fa/disabling-two-factor-authentication-for-your-personal-account
```

## Git 重新关联远程仓库地址的三种方法

```
https://www.jianshu.com/p/286437fae916
```

## 没了腾讯云，那就白嫖阿里云，继续实现多平台自动签到！

```
https://mp.weixin.qq.com/s/sExOgDi8l8QuXIz_bucE8Q
```
