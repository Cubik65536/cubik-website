---
title: 使用Visual Studio Code SSH远程访问控制CentOS 7服务器
date: 2021-01-10 14:16:47
categories:
  - 折腾记录
tags:
  - Linux
  - CentOS
  - 服务器
  - Visual Studio Code
---

## 一、配置服务器

### 服务器配置

操作系统：CentOS 7 x86_64版

网卡：Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller

### 配置服务器

直接安装CentOS即可，CentOS 7已经预装了我们所需的所有程序。但是你仍然需要在防火墙策略内放行SSH服务所需要的22端口。

## 二、配置Visual Studio Code

打开Visual Studio Code并在扩展商店内搜索Remote，找到Remote Development并安装，它会同时安装我们需要的Remote - SSH等插件。

![](https://cdn.jsdelivr.net/gh/Cubik65536/Cubik-Image-Hosting-Service/public/assets/img/20210110155525.png)

至此，我们就完成了Visual Studio的配置。

## 三、访问服务器

点击右下角的远程访问按钮并选择Remote SSH：Connect to Host...

![](https://cdn.jsdelivr.net/gh/Cubik65536/Cubik-Image-Hosting-Service/public/assets/img/20210110155733.png)

![](https://cdn.jsdelivr.net/gh/Cubik65536/Cubik-Image-Hosting-Service/public/assets/img/20210110155904.png)

选择Add New SSH Host，输入『用户名@ip地址』并回车，之后选择一个要更新的SSH配置文件，然后在右下角的添加成功弹窗处选择Connect连接远程服务器。

![](https://cdn.jsdelivr.net/gh/Cubik65536/Cubik-Image-Hosting-Service/public/assets/img/20210110160156.png)

在上方弹窗提示Enter password for『用户名@ip地址』时，输入该用户的密码并回车。至此你就已经成功连接到你的远程服务器上了！此时你的右下角远程连接提示处应该会显示你的远程主机ip。

![](https://cdn.jsdelivr.net/gh/Cubik65536/Cubik-Image-Hosting-Service/public/assets/img/20210110160645.png)
