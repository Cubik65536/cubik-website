---
title: Warp 终端上手第一印象
date: 2022-04-17 00:13:17
cover: https://img.cubik65536.top/warp-terminal.png
categories:
  - 日常
  - 折腾
tags:
  - 上手
  - 终端
  - macOS
---

前段时间在逛 GitHub 的时候偶然发现了 [Warp](https://github.com/warpdotdev/Warp) 这款终端，而它带来的一些例如输出块，辅助等功能也立即吸引了我，所有我立即将其下载并上手了一下。

<!-- more -->

## 登录

截至目前（2022 年 04 月 17 日），初次使用 Warp 仍然需要登录 GitHub 才可以使用，根据其隐私页面的解释：

{% quot When Warp comes out of beta, login via Github will not be required.
<br/><br/>
Right now, in our beta phase, we require a user login in order to access Warp.  This just gives us access to the email associated with your Github account. %} 

目前需要登录才可以使用是因为 Warp 目前在 Beta 阶段，而且开发者需要收集测试人员的联系方式。基于大部分软件测试都需要收集测试者的联系方式的情况，Warp 软件本身就开源的情况，以及 GitHub 在登录时仅提供邮箱的提示，我认为目前可以信任这款软件，我也相信在 Beta 结束后这款软件就不再需要登录。如果你真的非常在乎登录，那么建议等待正式版发布。

## 第一眼

在启动之后，你会看到这个页面（这并非我第一次启动的页面，而且我更改了主题）：

{% image https://img.cubik65536.top/warp-terminal-main.png %}

它包含一个我们都很熟悉的命令输入行（底部），以及两个快捷键提示。如果你只需要一个普通的终端，那么你已经可以直接使用了。

## 命令面板

在使用 {% kbd ⌘ %} + {% kbd P %} 之后，你将能够看到这个框

{% image https://img.cubik65536.top/warp-terminal-command-palette.png %}

这就是你的 Command Palette（命令面板）。在这个提示框里你能看到目前所有的功能和与其绑定的快捷键。

## 命令提示：

在使用 {% kbd ^ %} + {% kbd ⇧ %} + {% kbd R %} 之后，你将能够看到如下提示：

{% image https://img.cubik65536.top/warp-termianl-workflow.png %}

这就是 ”工作流” 页面，本质上，这就是将各种工具的常用命令集合在一起，你可以通过选中它们来选择你需要进行的操作。Warp 将会负责自动帮你补全命令的大部分内容和提示你需要填写的参数，而你只需要填写一部分参数。例如增加一个 git 远程分支：

{% image https://img.cubik65536.top/warp-termianl-workflow-git.png %}

而且用户可以自行增加其他的 “工作流”，来让 Warp 自动帮你生成所需的命令，而你每次只需要填写部分内容。

同时，使用 {% kbd ^ %} + {% kbd R %} 将会看到一个可以浏览的命令历史界面（直接点击上方向键也会显示一个略微简易一点的命令历史界面）。

## 命令块

我们使用的许多指令都会有很多输出，在 Warp 中，点击一段输出将会高亮整个命令和其输出，按上/下方向键可以前往前/后一个命令的开头。

## 辅助的使用

其实某些人是不喜欢辅助的，他们认为这会降低在无辅助工具的情况下使用各种工具的能力。其实这无不道理，目前各种 IDE 层出不穷，每个 IDE 都有自己的辅助和补全功能，而例如 GitHub Copilot 的 AI 代码生成更是发展快速。我相信不少人一下子从装满插件的 IDE 换成记事本写代码都会极其不适应。但同时，这些辅助又确实提升了我们的工作效率，见仁见智吧。

## 总结

Warp 的一些想法确实很不错，我目前仍为大量的使用 Warp，所以无法给出完整的点评，但是看得出来 Warp 的开发者是有独特的想法的。而且 Warp 以后还会主打团队协作功能，例如多人分享命令行或者直接在命令行里运行文档里的指令，这是值得期待的。

------

如果你也对这个命令行有兴趣的话，可以前往他们的[官网](https://www.warp.dev)下载（目前仅支持 macOS，但官方表示 Linux, Windows, 和 Web (WASM) 都在开发中）。GL&HF!
