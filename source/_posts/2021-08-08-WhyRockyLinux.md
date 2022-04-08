---
title: 为什么我要选择 Rocky Linux 作为服务器操作系统？
date: 2021-08-08 12:38:02
cover: https://user-images.githubusercontent.com/72877496/162445577-d6801431-ffd2-49ce-be99-83f895562c28.png
categories:
  - 解决方案
tags:
  - 服务器
  - Linux
---

大家都知道，CentOS 8 将在今年（文章于2021年8月8日撰写）年底停止支持，CentOS 将不再作为 RHEL（Red Hat Enterprise Linux）的下游构建版本，而是转变为与 Fedora 类似的上构建版本。同时，CentOS 也会开始采用滚动更新，这意味着 CentOS 作为服务器操作系统的稳定性开始成为问题。而市面上随之而来的还有各种替代方案，那为什么我选择了 Rocky Linux 呢？

## 太长了！我不看！

如果你想直接知道结论，那么我选择 Rocky Linux 的原因无非就三点：靠谱，稳定，习惯。如果你还想知道我为什么得出了这个结论，请继续看下去。

## 其他替代方案

在真正开始讨论我为什么使用 Rocky Linux 之前，我想聊聊为什么**不**使用以下几种方案。

### RHEL

花钱？不可能的，这辈子都不可能花钱的。

### Ubuntu Server

由于我长期使用 Fedora 和 CentOS，已经习惯了 RHEL 系列的操作系统，有点不太能对于转变系统所带来的变化（例如包管理器的变化）

### CentOS 7

没错，这个比 CentOS 8 老的系统到2024年才结束支持，但是各种新版环境安装极为繁琐，所以放弃。

### Oracle Linux

这是一个由甲骨文公司维护的 RHEL 下游操作系统。理论上与 CentOS 并没有太大差别，而我现在甲骨文云上的计算实例也全部在运行着 Oracle Linux，但是我仅仅把它当作在 Oracle Cloud 上的专属方案使用。

### 其他发行版（包括其他 RHEL 下游）

由于我对这些发行版本不够了解，所以放弃。

### Rocky Linux

接下来我来讲讲为什么使用 Rocky Linux。

## 靠谱

Rocky Linux 由最开始的 CentOS 创始人 Gregory Kurtzer 创办，对我来说，这点足以证明 Rocky Linux 的可信性。

## 稳定

对于服务器来说，稳定性是非常重要的。而 Rocky Linux 是一个 RHEL 下游构建版本，理论上是能提供 RHEL 以及以前 CentOS 提供的稳定性的。

## 习惯

接续上一点，Rocky Linux 是一个 RHEL 下游构建版本，所以各种操作都与 RHEL 和 CentOS 没有太大差别。甚至 Rocky Linux 提供了一个脚本来帮助用户从 RHEL/CentOS 8/Oracle Linux等系统转换到 Rocky Linux，这对于大部分用户来说应当是个好消息。

## 结语

这就是为什么我选择将我的服务器操作系统换成了 Rocky Linux。其实我从 Rocky Linux 项目开始的时候就加入了 Rocky Linux CN 社区，我也贡献了一篇月更新报告的翻译，作为一个早期支持者（笑），希望 Rocky 越做越好。

-------

GL, HF.
