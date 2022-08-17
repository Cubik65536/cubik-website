---
title: 2022年8月上半月总结
date: 2022-08-16 18:33:17
categories:
- 日常
tags:
- 生活记录
---

**本文撰写于一台 Linux 计算机**

我一直都特别想尝试写一些生活类的文章，但是我一直没有找到我比较喜欢的主题。这次终于找到了这么一个机会，所以这是我的第一次尝试，希望能够越来越好，开始吧！

<!-- more -->

这个月月初填了一下 UTM 体验的坑，然后花时间比较多的地方就是学习 USACO 以及开发 [RemoteMC](https://github.com/iXORTech/RemoteMC-Core) 系列软件的模块。同时，我终于完成 iXOR Technology 组织的 GitHub 看板的配置以及一些工作流程自动化的配置。为此我还开发了一个 GitHub 机器人来完成另外一些功能，下文会提到。

## RemoteMC 系列

原本我打算在月初发布 RemoteMC 系列的 MCDR 模块，但是发现了 Core 模块的信息转发设计不能实现我想要的功能，于是我在月初重新设计了 Core 模块的信息接收与发送的工作方式实现了我想要的一些功能。同时 MCDR 模块的功能也都齐全了，用于自动插件打包与发布的 GitHub Action 也完成了。下一步就要在连接着多个 MCDR 模块的情况下进行测试并发布一些测试视频，敬请期待。

## [CubikTech/self-approval](https://github.com/CubikTech/self-approval)

同时，由于新的开发流程，我也需要向我自己的仓库提交 Pull Requests。但是由于 GitHub 并不允许 Pull Request 作者批准自己的 Pull Request,，所以我开发了这个 GitHub App，允许白名单中的用户通过发送 comment 让机器人批准自己的 Pull Request.

## MacBook Pro 维修

前一段时间我的 MacBook Pro 的 Touch Bar 不亮了，去找苹果要更换下半台电脑，太贵了。于是我找了一家修理店希望可以便宜一点修好 Touch Bar。撰写本文的今天我刚刚将电脑送去检测。这也是为什么本文是在我的 Linux 机器上撰写的。希望 MacBook Pro 可以早日回归，我也能够恢复我平常极度依赖于 macOS 的工作流。同时，因为要修电脑的原因，RemoteMC 的测试以及一些视频的制作要被延后了...

## 配置 Linux 备用机器

由于这段时间我必须依赖于我以前基本不用的 Linux 机器完成几乎所有要做的东西，所以我这几天配置了新系统、终端（Oh my zsh 以及主题和插件）、编译器、git、签名、VSCode 等日用软件。虽然还有一些小问题没有解决，不过应该可以让我能够撑过这段时间了。同时，我也将我的系统换回了 Arch 系操作系统（Artix Linux），或许以后会讲讲使用体验吧。

## 总结

那么就先这样了！这两天可能会有一些其他博客发布，敬请期待！然后我们月底再见！
