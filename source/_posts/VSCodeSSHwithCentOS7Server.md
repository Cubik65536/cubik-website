---
title: 使用 Visual Studio Code SSH 远程访问控制 CentOS 7 服务器
date: 2021-01-10 14:16:47
cover: https://cdn.jsdelivr.net/gh/Cubik65536/Cubik-Image-Hosting-Service/public/assets/img/20210110155525.png
categories:
  - 日常
  - 折腾
tags:
  - Linux
  - CentOS
  - 服务器
  - Visual Studio Code
---

## 一、配置服务器

### 服务器配置

操作系统：CentOS 7 x86_64 版

网卡：Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller

### 配置服务器

直接安装 CentOS 即可，CentOS 7 已经预装了我们所需的所有程序。但是你仍然需要在防火墙策略内放行 SSH 服务所需要的 22 端口。

## 二、配置 Visual Studio Code

打开 Visual Studio Code 并在扩展商店内搜索 Remote，找到 Remote Development 并安装，它会同时安装我们需要的 Remote - SSH 等插件。

![](https://cdn.jsdelivr.net/gh/Cubik65536/Cubik-Image-Hosting-Service/public/assets/img/20210110155525.png)

至此，我们就完成了 Visual Studio 的配置。

## 三、访问服务器

点击右下角的远程访问按钮并选择 Remote SSH：Connect to Host...

![](https://cdn.jsdelivr.net/gh/Cubik65536/Cubik-Image-Hosting-Service/public/assets/img/20210110155733.png)

![](https://cdn.jsdelivr.net/gh/Cubik65536/Cubik-Image-Hosting-Service/public/assets/img/20210110155904.png)

选择 Add New SSH Host，输入『用户名@ip 地址』并回车，之后选择一个要更新的 SSH 配置文件，然后在右下角的添加成功弹窗处选择 Connect 连接远程服务器。

![](https://cdn.jsdelivr.net/gh/Cubik65536/Cubik-Image-Hosting-Service/public/assets/img/20210110160156.png)

在上方弹窗提示 Enter password for『用户名@ip 地址』时，输入该用户的密码并回车。至此你就已经成功连接到你的远程服务器上了！此时你的右下角远程连接提示处应该会显示你的远程主机 ip。

![](https://cdn.jsdelivr.net/gh/Cubik65536/Cubik-Image-Hosting-Service/public/assets/img/20210110160645.png)
