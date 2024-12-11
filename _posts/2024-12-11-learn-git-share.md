---
layout: post
title: Git学习笔记
categories: [Git]
description: Git学习笔记,Git常用命令
keywords: Git,常用命令,学习笔记
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Git 是一个免费、开源的**分布式版本控制系统**，它使用一个特殊的叫做 Repository（仓库）的数据库来记录文件的变化，仓库中的每个文件都有对应的版本历史记录（可以看到谁，在什么时间，修改了哪些内容）。还可以在需要的时候把文件恢复到之前的某一个版本。

# Git 学习笔记

---

# 什么是 Git

Git 是一个免费、开源的**分布式版本控制系统**

---

> #### _什么是分布式？_
>
> 分布式是指**多个系统协同合作完成一个特定任务的系统**。
>
> 它是**不同的系统**部署在不同的服务器上，服务器之间相互调用。
>
> 好比 多个人一起**做不同的事**。

---

它使用一个特殊的叫做 Repository（仓库）的数据库来记录文件的变化，仓库中的每个文件都有对应的版本历史记录（可以看到谁，在什么时间，修改了哪些内容）。还可以在需要的时候把文件恢复到之前的某一个版本。

有了版本控制系统就可以跟踪每个文件的变化，对多人开发完成的项目很友好。

---

## 目前世界上的版本控制系统分类

### 1.集中式

所有文件保存到中央服务器上，每个人的电脑上保存文件副本。

修改文件时，需要从中央服务器上下载最新版本，之后添加想要的内容，修改完成后再上传回服务器。

使用简单，但中央服务器的单点故障问题（例如网络故障）是集中式版本控制系统的致命缺点。

### 2.分布式

每个人的电脑上都有一个完整的版本库，可以在本地进行修改（所以不需要考虑网络问题）。

需要将修改内容分享给其他人的时候，只需要相互同步一下就可以了。

大多数的开源项目都在使用 Git 进行版本控制，很多网站上托管的开源项目也都是使用 Git 进行版本控制（如[Github](https://github.com)）。

# Git 的安装和初始化配置

## 一、安装

1. 去[git 官网](https://git-scm.com/)进行下载并安装
2. 验证安装：在终端输入`git -v`命令

​ 看到版本信息说明安装成功

---

## 二、Git 的使用方式

### 1.命令行

最基本、最常用，在终端中使用 git 命令来操作 Git

Git 的所有命令都以`git`开头

### 2.图形化界面（GUI）

通过一些专用的图形化工具使用 Git

### 3.IDE 插件/扩展

在 IDE 中通过扩展或者插件来使用 Git（VSCode 默认集成了源码管理器）

---

# 三、初始化配置

1.使用`git config`命令配置邮箱和用户名，为了识别出是谁提交的内容

**配置用户名：**

```shell
git config --global user.name "Binfinity"
```

**配置邮箱：**

```shell
git config --global user.email 2870639124@qq.com
```

**保存用户名和密码**（这样不用每次都输入）**：**

```shell
git config --global credential.helper store
```

**查看配置信息：**

```shell
git config --global --list
```

|     参数      |           表示           |
| :-----------: | :----------------------: |
| 省略（Local） | 本地配置，只对本仓库有效 |
|   --global    |  全局配置，所有仓库生效  |
|   --system    | 系统配置，对所有用户生效 |

# 新建仓库

> 版本库 即 **仓库** 英文名 **Repository** 简称 **Pepo**
>
> 仓库中的每个文件的修改、删除、添加等操作都会被 Git 跟踪到
>
> 可以把仓库理解为一个文件夹

---

## 创建仓库的方法：

### （1）在电脑本地创建一个仓库

在合适的位置创建一个空目录，在终端中打开，输入`git init`即可创建仓库，它会在当前目录生成一个.git 的隐藏目录，用来存放当前仓库的所有信息

```shell
binfinity@binfinity:~/Git_Learn$ git init
已初始化空的 Git 仓库于 /home/binfinity/Git_Learn/.git/
binfinity@binfinity:~/Git_Learn$ l -a
./  ../  .git/
```

`git init`后面可以指定目录的名称，`git init <name>`在当前目录下创建一个新的目录作为 Git 仓库

```shell
binfinity@binfinity:~/Git_Learn$ git init my-repo
已初始化空的 Git 仓库于 /home/binfinity/Git_Learn/my-repo/.git/
binfinity@binfinity:~/Git_Learn$ cd my-repo/
binfinity@binfinity:~/Git_Learn/my-repo$ ll
总计 12
drwxrwxr-x 3 binfinity binfinity 4096  5月  2 08:45 ./
drwxrwxr-x 4 binfinity binfinity 4096  5月  2 08:45 ../
drwxrwxr-x 7 binfinity binfinity 4096  5月  2 08:45 .git/
```

注意生成的.git 目录是在 my-repo 目录下的，也就是 my-repo 是刚刚创建的仓库

### （2）从远程服务器中克隆一个已经存在的仓库

使用克隆命令从 GitHub、Gitee 这种远程服务器上来克隆一个已存在的仓库。(注意添加公钥)

`git@github.com:AaaBinfinity/Qing_Bin_Learn.git`是本仓库的地址

可使用`git clone <地址>`命令实现克隆

```shell
binfinity@binfinity:~/Git_Learn$ git clone git@github.com:AaaBinfinity/Qing_Bin_Learn.git
Cloning into 'Qing_Bin_Learn'...
remote: Enumerating objects: 981, done.
remote: Counting objects: 100% (392/392), done.
remote: Compressing objects: 100% (253/253), done.
Rremote: Total 981 (delta 228), reused 239 (delta 137), pack-reused 589
Receiving objects: 100% (981/981), 18.60 MiB | 4.01 MiB/s, done.
Resolving deltas: 100% (392/392), done.
```

---

# 工作区和文件状态

**git 的本地数据管理分为三个区域：工作区（Working Directory）、暂存区（Staging Area/Index）、本地仓库（Local Repository）**

---

#### 工作区（Working Directory）：

**_.git 所在的目录_**

也叫工作目录或本地工作目录，自己电脑上的目录，资源管理器上看到的文件夹

#### 暂存区（Staging Area/Index）：

**_.git/index_**

也成为索引，是一种临时存储区域（中间区域），用于保存即将提交到 Git 仓库的修改内容

可以通过`git ls-files`来查看暂存区的内容

#### 本地仓库（Local Repository）：

**_.git/objects_**

通过`git inti`命令创建的仓库，包含了完整的项目历史和元数据，是 Git 存储代码和版本信息的主要位置

---

### 工作区-------->暂存区------->本地仓库

**修改工作区的文件-----(`git add`）------>添加到暂存区-----(`git commit`)------>提交到本地仓库**

这个过程可以使用 git 提供的命令来查看、比较、撤销、修改

_车间--------->运输车辆---------->仓库_

---

### 文件状态

1. **未跟踪**（Untrack）

   ​ 新创建的、未被 Git 管理起来的文件

2. **未修改**（Unmodified）

   ​ 已经被 Git 管理起来，文件内容没有发生改变的文件

3. **已修改**（Modified）

   ​ 已经修改，没有添加到暂存区的文件

4. **已暂存**（Staged）

​ 修改后添加到暂存区的文件

**未跟踪（Untrack）**---(`git add`)--->**未修改（Unmodified）**----(修改文件)--->**已修改（Modified）**----(`git add`)---->**已暂存（Staged）**

**未跟踪（Untrack）**---(`git add`)--->**已暂存（Staged）**

**已暂存（Staged）**---(`git commit`)---->**未修改（Unmodified）**

**未修改（Unmodified）**---(`git rm`)--->**未跟踪（Untrack）**

**已暂存（Staged）**----(`git reset`)-->**已修改（Modified）**

**已修改（Modified）**----(`git checkout`)------>**未修改（Unmodified）**

---

## 基本概念

- `main` :默认主分支
- `origin` :默认远程仓库
- `HEAD` :指向当前分支的指针
- `HEAD^` : 上一个版本
- `HEAD~4` :上 4 个版本

---

## 特殊文件

- `.git` :Git 仓库的元数据和对象数据库
- `.gitignore` :忽略文件，不需要提交到仓库的文件
- `.gitattributes` :指定文件的属性，比如换行符
- `.gitkeep` :使空目录被提交到仓库
- `.gitmodules`:记录子模块的信息
- `.gitconfig` :记录仓库的配置信息

---

# 添加和提交文件

查看仓库的状态，可以查看当前仓库在哪个分支，有那些文件，这些文件处于一个什么样的状态

```shell
git status
```

---

添加文件到暂存区，等待后续的提交操作

```shell
git add <file>...
```

把当前目录下的所以文件都添加到暂存区里面

```shell
git add .
```

将暂存区的文件取消暂存

```shell
git rm --cached <file>...
```

---

将文件提交到仓库中，文件只有在提交到仓库中才会被真正的管理起来

```shell
git commit -m "massage"
```

**注意：**

`git commit`命令只会提交暂存区的文件，不会提交工作区的内容。

`git commit`命令在提交时需要使用`-m`参数来指定提交的信息，这个信息会被记录到仓库中

---

查看提交记录

```shell
git log
```

查看简介的提交记录

```shell
git log --oneline
```

---

---

## 分支

查看所有本地分支，当前分支前面会有一个`*`，`-r`查看远程分支，`-a`查看所有分支

```shell
git branch
```

创建一个新分支

```shell
git branch <branch-name>
```

切换到指定分支，并更新工作区

```shell
git checkout <branch-name>
```

创建一个新分支，并切换到该分支

```shell
git checkout -b <branch-name>
```

删除一个已经合并的分支

```shell
git branch -d <branch-name>
```

删除一个分支，不管是否合并

```shell
git branch -D <branch-name>
```

给当前的提交打上标签，通常用于版本发布

```shell
git tag <tag-name>
```

---

## 合并分支

合并分支 a 到分支 b，`-no-ff` 参数表示禁用 Fast forward 模式，合并后的历史有分支，能看出曾经做过合并，而`-ff`参数表示使用 Fast forward 模式，合并后的历史会变成一条直线

```shell
git merge --no-ff -m "message" <branch-name>
```

<img src="https://github.com/AaaBinfinity/Qing_Bin_Learn/blob/637b94f8f2e5f1a977a223149aeb1dddcbbe7fdb/Learn_Git/Gitimg/image-20240502105101489.png" alt="image-20240502105101489" style="zoom:67%;" />

```shell
git merge --ff -m "message" <branch-name>
```

<img src="https://github.com/AaaBinfinity/Qing_Bin_Learn/blob/637b94f8f2e5f1a977a223149aeb1dddcbbe7fdb/Learn_Git/Gitimg/image-20240502105432034.png" alt="image-20240502105432034" style="zoom:67%;" />

合并`&squash`所有提交到一个提交

```shell
git merge --squash <branch-name>
```

`rebase`不会产生新的提交，而是把当前分支的每一个提交都"复制"到目标分支上，然后再把当前分支指向目标分支，而`merge`会产生一个新的提交，这个提交有两个分支的所有修改。

---

## Rebase

Rebase 操作可以把本地未 push 的分叉提交历史整理成直线看起来更直观。但是，如果多人协作时，不要对已经推送到远程的分支执行 Rebase 操作。

```shell
git checkout <dev>
git rebase <main>
```

<img src="https://github.com/AaaBinfinity/Qing_Bin_Learn/blob/637b94f8f2e5f1a977a223149aeb1dddcbbe7fdb/Learn_Git/Gitimg/image-20240502105627014.png" alt="image-20240502105627014" style="zoom:67%;" />

## 撤销

移动一个文件到新的位置

```shell
git mv <file> <new-file>
```

从工作区和暂存区中删除一个文件，然后暂存删除操作

```shell
git rm <file>
```

只从暂存区中删除一个文件，工作区中的文件没有变化

```shell
git rm--cached <file>
```

恢复一个文件到之前的版本

```shell
git checkout <file> <commit-id>
```

创建一个新的提交，用来撤销指定的提交，后者的所有变化都将被前者抵消，并且应用到当前分支

```shell
git revert <commit-id>
```

重置当前分支的 HEAD 为之前的某个提交，并且删除所有之后的提交。`--hard` 参数表示重置工作区和暂存区，`--soft` 参数表示重置暂存区，`--mixed` 参数表示重置工作区

```shell
git reset --mixed <commit-id>
```

撤销暂存区的文件，重新放回工作区(git add 的反向操作)

```shell
git restore --staged <file>
```

---

## 查看

列出还未提交的新的或修改的文件

```shell
git status
```

查看提交历史，`--oneline` 可省略

```shell
git log --oneline
```

查看未暂存的文件更新了哪些部分

```shell
git diff
```

查看两个提交之间的差异

```shell
git diff <commit-id> <commit-id>
```

---

## Stash

Stash 操作可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作。`-u` 参数表示把所有未跟踪的文件也一并存储，`-a` 参数表示把所有未跟踪的文件和忽略的文件也一并存储，save 参数表示存储的信息，可以不写。

```shell
git stash save "message”
```

查看所有 stash

```shell
git stash list
```

恢复最近一次 stash

```shell
git stash pop
```

恢复指定的 stash,`stash@{2}`表示第三个 stash，`stash@{0}`表示最近的 stash

```shell
git stash pop stash@{2}
```

重新接受最近一次 stash

```shell
git stash apply
```

`pop` 和 `apply` 的区别是， `pop` 会把 `stash` 内容删除，而`apply` 不会。可以用 `git stash drop` 来删除 `stash`

```shell
git stash drop stash@{2}
```

删除所有 stash

```shell
git stash clear
```

---

## 远程仓库

添加远程仓库

```shell
git remote add <remote-name> <remote-url>
```

查看远程仓库

```shell
git remote -v
```

删除远程仓库

```shell
git remote rm <remote-name>
```

重命名远程仓库

```shell
git remote rename <old-name> <new-name>
```

从远程仓库拉取代码

```shell
git pull <remote-name> <branch-name>
```

fetch 默认远程仓库(origin)当前分支的代码，然后合并到本地分支

```shell
git pulL
```

将本地改动的代码 Rebase 到远程仓库最新的代码上(为了有已个干净的、线性的提交历史)

```shell
git pull --rebase
```

推送代码到远程仓库(然后再发起 pull request)

```shell
git push <remote-name> <branch-name>
```

获取所有远程分支

```shell
git fetch <remote-name>
```

查看远程分支

```shell
git branch -r
```

fetch 某一个特定的远程分支

```shell
git fetch <remote-name> <branch-name>
```

---

## Git Flow

GitFlow 是一种流程模型，用于在 Git 上管理软件开发项目。

- `主分支(master)`:代表了项目的稳定版本，每个提交到主分支的代码都应该是经过测试和审核的。
- `开发分支(develop)`:用于日常开发。所有功能分支发布分支和修补分支都应该从 develop 分支派生。
- `功能分支(feature)`:用于开发单独的功能或特性。每个功能分支应该从 develop 分支派生，并在开发完成后合并回 develop 分支。
- `发布分支(release)`:用于准备项目发布。发布分支应该从 develop 分支派生，并在准备好发布版本后合并回 master 和 develop 分支。
- `修补分支(hotfix)`:用于修复主分支上的紧急问题。修补分支应该从 master 分支派生，并在修复完成后合并回 master 和 develop 分支

---
