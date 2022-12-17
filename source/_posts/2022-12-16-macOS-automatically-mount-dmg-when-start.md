---
title: macOS 操作系统在启动时自动挂载 dmg 文件
date: 2022-12-16 14:48:26
cover:
categories:
  - 解决方案
  - 折腾
tags:
  - macOS
  - dmg
  - 速记
---

## 前言

在重装 macOS 操作系统的过程中，为了一些软件的兼容性考虑，我将我的主硬盘（`Macintosh HD`）格式化为了 APFS（加密）格式。但是我对存储代码的文件夹有使用大小写敏感功能的要求，于是我单独创建了一个 APFS（区分大小写）的 dmg 镜像用来存储我的代码。但是每次电脑重启之后，我们都需要手动挂载这个 dmg 文件，那么有的时候是相对麻烦的，所以我就开始寻找一个可以在 macOS 登陆之后自动挂载 dmg 文件的方法。

## 解决方案

{% note color:warning 我的操作系统语言为英语，本文中关于按钮/软件的内容也将会使用英语。请在必要时参考截图或者其他方法进行对照 %}

1. 前往 System Settings

  {% image https://img.cubik65536.top/SCR-20221216-kxl.jpeg %}

2. 搜索 `Login Items` 并点击 `Login Items` 选项

  {% image https://img.cubik65536.top/SCR-20221216-kxv.jpeg %}

3. 点击 `Open at Login` 区域下方的 `+` 按钮

  {% image https://img.cubik65536.top/SCR-20221216-t5u.jpeg %}

4. 在弹出的对话框中找到你的 dmg 文件并点击 `Open` 按钮

  {% image https://img.cubik65536.top/SCR-20221216-t6c.jpeg %}

5. 如果你的 dmg 文件出现在 `Login Items` 的列表中了，设置就完成了

  {% image https://img.cubik65536.top/SCR-20221216-t7l.jpeg %}
