---
title: git 的基础使用与 FastGit 加速
date: 2021-08-12 09:46:48
categories:
  - 解决方案
tags:
  - git
---

在本文中，我将简单的提到如何使用 git，以及如何使用 FastGit 加速 GitHub

## 如何打开命令行

### Windows

使用 Win（键盘上的Windows标志） + R 按键，输入 `cmd`，或者右键开始，选择 `命令提示符` 或 `Powershell`

### macOS

Command + 空格，搜索 `终端` 或者 `Terminal`

### Linux

有桌面环境的去应用列表找找 `终端` 或者 `Terminal`。

没有的... 你没开玩笑吧？~~大佬请离开这个页面~~ 这就是你的命令行！

## Git 使用基础

### 安装 Git

首先，你需要在你的计算机上安装 git，你可以先通过在终端执行以下命令来检查 git 是否已安装：

``` bash
git --version
```

而如果输出为 `git version x.xx.x` (x.xx.x 为版本号，每人的输出可能不一样)，则代表你已经安装。

{% noteblock color:yellow 提示 %}

一般来说，如果你购买的是 Apple Mac 系列计算机，则你的系统中应该已经预装了 git

{% endnoteblock %}

如果你没有安装，请按照以下方式安装：

#### Windows

前往[下载页面](https://git-scm.com/downloads)，点击Windows。打开安装包并一直下一步即可。

#### macOS

``` bash
$ brew install git
```

#### Linux

##### 基于RPM的Linux发行版本（SUSE，RHEL等）

``` bash
$ sudo dnf install git-all
```

##### 基于Debian的Linux发行版吧（例如Ubuntu）

``` bash
$ sudo apt install git-all
```

### Git 的基础操作

注意，本章所有内容都将以 `https://github.com/author/repo` 作为演示仓库链接，在使用时记得替换为你自己的仓库。

{% noteblock color:yellow 提示 %}

本篇针对的读者是没有任何基础但是需要临时使用 git 与其他人合作的用户。故不提起 init 等其他操作。更详细的使用可以参考xaoxuu大佬的[Git实用教程](https://xaoxuu.com/wiki/git/) 和 xugaoyi大佬的[Git学习笔记](https://xugaoyi.com/note/git/)

{% endnoteblock %}

#### “登录”

准确的来说是将你的用户名和邮箱配置好，让 git 知道这是你做的。

``` bash
git config --global user.name "用户名"
git config --global user.email "邮箱"
```

使用以下命令可以检查配置是否正常：

``` bash
git config --global --list
```

由于这个身份极易被伪造，所以某些人会使用PGP来验证签名。如果你在用，你就需要进行额外操作。~~大佬请离开这个页面x2~~

#### clone

简单来说，这个操作是为了将你的文件下载到你的机器上，打开命令行，输入以下命令

``` bash
git clone https://github.com/author/repo
```

在你完成之后，所有云端文件都应该存储到你的计算机上了。如何找到文件夹？

##### Windows

查看下方截图红框内的内容，这就是你的路径。

![CMD路径](https://img.cubik65536.top/CMD)

##### macOS & Linux

一般都在你的 User 家目录下，使用了 cd 命令的各位请自行寻找 ~~大佬请离开这个页面x3~~

#### 编辑

ummm。打开你喜欢的编辑器，写吧！

推荐编辑器：Visual Studio Code（样样行），Typora（Markdown，md文件）

#### commit

这一步简单来说就是告诉软件，你要提交一个更改。先进入你的 git 路径（一般是你 clone 的路径再 `cd <文件夹名>`）

{% noteblock color:yellow 提示 %}

文件夹名一般就是 repo 名称，即为 `https://github.com/author/repo` 中的 `repo`

{% endnoteblock %}

输入以下命令

``` bash
git commit -m "信息"
```

`信息` 一般填写你更改的内容

#### push

这一步简单来说就是把你commit过的内容提交给服务器。输入：

``` bash
git push origin
```

#### pull

这一步简单来说就是把服务器有但你本机没有的更改（例如别人做的改动）弄到你机器上。输入：

``` bash
git pull origin
```

## 使用 FastGit 加速

由于某些地区 GitHub 访问受限，所以我们需要使用 FastGit 来进行加速。使用方法很简单：将链接内的 `github.com` 替换为 `hub.fastgit.org` 即可。

即为将 `https://github.com/author/repo` 替换为 `https://hub.fastgit.org/author/repo`。

其他操作可参考 [FastGit用户手册](https://doc.fastgit.org/zh-cn/guide.html#web-的使用)
