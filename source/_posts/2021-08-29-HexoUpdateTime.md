---
title: Hexo 更新日期问题
date: 2021-08-29 23:13:29
cover: https://hexo.bootcss.com/icon/hexo.png
categories:
  - 解决方案
  - 教程
tags:
  - Hexo
  - blog
---

在刚刚写完并上传上一篇[《迁移 Minecraft 到微软账户》](https://blog.cubik65536.top/2021-08-29-TransferMinecraftToMicrosoft/)之后，令我困扰许久的另外一个问题又出现了。我博客左侧的《最近更新》的时间排名是错误的。实际显现出来的问题是：我不管是否更新了的文章的更新日期都被更新为部署的时间了。

## 寻找解决方案

在一阵搜寻之后，我找到了这个 GitHub issue：https://github.com/YunYouJun/hexo-theme-yun/issues/60，而它，解决了所有问题。

## 如何解决该问题

### 一、确定 Hexo 版本

本设置需要 Hexo 版本为 5.0 以上才可正常工作

### 二、编辑配置文件

将 `_config.yml` 中的 `updated_option` 的值改为 `date` 即可

这时，如果你重新生成或部署 Hexo，更新日期就是正确的了！GL&HF！
