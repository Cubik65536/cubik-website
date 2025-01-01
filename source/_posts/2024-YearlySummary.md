---
title: 2024 年终总结
date: 2024-12-31 23:59:59
categories:
  - 日常
tags:
  - 生活记录
cover: https://learning-api.quantum.ibm.com/assets/52051f0b-3f04-4869-a8df-ac13bd17f9cb
---

首先，祝各位新年快乐！！！2024 最后一篇文章，来看看今年都发生了什么吧！

<!--more-->

## 2024 的 GitHub 统计

首先来看看 [postspark.app](https://postspark.app/) 为我生成的 2024 年终 GitHub 贡献统计图和 [#GitHubUnwrapped](https://githubunwrapped.com/) 的 GitHub 总结图：

{% image https://img.cubik65536.top/postspark-GitHub-Cubik65536-20241231.png "我的 2024 年终 GitHub 贡献统计图" %}

{% image https://img.cubik65536.top/githubunwrapped-Cubik65536-2024.jpeg "我的 2024 #GitHubUnwrapped 总结图" %}

由于 [#GitHubUnwrapped](https://githubunwrapped.com/) 今年没有更新视频的视觉效果，所以我就不像前两年一样分享视频了。

可见今年确实没少写代码，捡几个有趣的事情分享一下：

### Ungoogled Chromium macOS

从今年一月开始我就是 [ungoogled-chromium-macos](https://github.com/ungoogled-software/ungoogled-chromium-macos) 的维护者，而我今年提交的大部分 Pull Request 也是贡献到这个项目中的。今年我对该项目在 GitHub Action 上进行自动化构建的功能进行了一些改进，同时，更重要的是，这个项目的 App 已由我的 Apple 开发者账户签名，为用户提供了更好的体验。

更好的是，由于这个项目我今年第一次正式批量收到了来自 GitHub Sponsors 和 Buy Me a Coffee 的用户赞助！！！感谢大家的支持！

### FOSScope / 开源观察

在今年 2 月 Linux 中国的硬核老王 [宣布 Linux 中国将停止运营](https://linux.cn/article-16602-1.html) 后，我与几位志同道合的朋友一起创建了 [FOSScope](https://fosscope.org/)，并在今年 3 月正式上线。FOSScope 是一个开源相关的资讯站，目前仍处于开荒与试验阶段，但我们的目标是接过 Linux 中国翻译组的工作，继续为中文开源社区提供优质的翻译内容，并且我们也在尝试一些新的内容形式，例如原创文章、新闻栏目等。

各位可以通过以下链接访问 FOSScope 的文章站：

{% link https://fosscope.com/ FOSScope 文章站 icon:https://fosscope.com/logo/FOSScope900r.png %}

也欢迎各位通过 [GitHub](https://github.com/FOSScope) 参与到我们的工作中：

{% link https://github.com/FOSScope FOSScope 的 GitHub 组织 icon:https://github.githubassets.com/assets/GitHub-Mark-ea2971cee799.png %}

另外，为了 [为 FOSScope 设计的工具箱](https://github.com/FOSScope/Toolkit)（尚未开发完成，欢迎参与）我还专门学习了 Rust 和 Tauri，这也是今年的一大收获（因此我今年最常用的语言也变成了 Rust）。

### Typstify

我今年还开始了一个名为 Typstify 的新项目，这是一个为 iPadOS 开发的 [Typst](https://typst.app) 编辑器，各位可以通过以下链接访问 Typstify 的 GitHub 仓库：

{% link https://github.com/iXORTech/Typstify iXORTech/Typstify icon:https://github.githubassets.com/assets/GitHub-Mark-ea2971cee799.png %}

因为年初使用 Typst 写微积分课程笔记的时候发现有的时候我希望可以使用触控笔来画一些图样，但是桌面/网页版 Typst 并不支持这个功能。因此，考虑到我也比较满意 Apple Pencil 为我带来的笔记体验，所以我就开始了这个项目，目的是提供一个较为完善的 iPadOS 上的 Typst 编辑器，同时支持将 Apple Pencil 画的图转换为图片插入到 Typst 文档中。

目前这个项目还在非常初始的阶段，因为我一直在寻找合适的技术和 Swift 库来实现一些我想要的功能（例如编辑器，语法高亮等），而我认为相当合适的项目 [krzyzanowskim/STTextView](https://github.com/krzyzanowskim/STTextView) 的语法高亮等插件暂时并不支持 iOS/iPadOS，所以我目前一直在帮助这个项目的作者进行 iOS 的适配工作。触发了个会卡住主线任务而且超长的支线任务 🤣

另也要吐槽，我使用了 [mchakravarty/ProjectNavigator](https://github.com/mchakravarty/ProjectNavigator) 来当作这个项目的项目管理库（用于管理一个项目的多个文件等），但是在 iOS 18 更新后，[这个库的文件保存功能则出现了问题](https://github.com/mchakravarty/ProjectNavigator/issues/28)。然后 iOS 18.1 这个问题消失了（iOS 18.0 仍未修复），而 iOS 18.0 和 18.1 是并列存在的（分别适用于支持与不支持 Apple Intelligence 的设备），也是很神奇了。

## 《如何开始为开源项目做贡献？》

今年我校的开源 Club 按传统继续举办了一年一度的开源日，我也帮助组织了活动并做了一个关于如何开始为开源项目做贡献的分享，各位可以通过以下链接查看我为此准备的幻灯片：

{% link https://github.com/Cubik65536/VanierFLOSSDay24Talk Cubik65536/VanierFLOSSDay24Talk icon:https://github.githubassets.com/assets/GitHub-Mark-ea2971cee799.png %}

另外我还为这个活动设计了一波 3D 打印的纪念品，是各种颜色配色的电路板钥匙串，还是挺好玩的。

{% swiper effect:cards %}
![](https://img.cubik65536.top/vanierflossday24-3dswag-technicaldesignpreview.png)
![](https://img.cubik65536.top/vanierflossday24-3dswag-prototypes.jpeg)
![](https://img.cubik65536.top/vanierflossday24-3dswag-printing.jpeg)
![](https://img.cubik65536.top/vanierflossday24-3dswag-prepared.jpg)
{% endswiper %}

## ROG Ally X

今年年底我终于入到了一台 ROG Ally X 给我自己当圣诞礼物，也算是圆了个梦。

{% swiper effect:cards %}
![](https://img.cubik65536.top/AllyX-Unboxing.jpeg)
![](https://img.cubik65536.top/AllyX-Booting.jpeg)
![](https://img.cubik65536.top/AllyX-Games.jpeg)
{% endswiper %}

放假了之后其实一直在玩，顺便也快把一个游戏流程玩完了，后面再更一篇游戏体验吧。

## 大学申请

年底还按计划提交了大学申请，希望一些顺利吧。

## Racket 学习计划

今年下半年开了个 [Racket 入门系列文章](https://www.cubik65536.top/intro-to-racket/getting-started/)。这个系列是我为了学习 Racket 语言入门当中笔记写的，而最基础的内容也还算是都覆盖到了，明年计划继续更新跟着我的学习进度较为高级的内容。

## IBM Quantum Learning

{% image https://learning-api.quantum.ibm.com/assets/52051f0b-3f04-4869-a8df-ac13bd17f9cb %}

我今年还开始跟着 [IBM Quantum Learning](https://learning.quantum.ibm.com/) 平台上的课程学习量子计算，后面我也会写一写学习笔记啥的，还是挺有趣的。目前计划是 2025 年初完成基础的课程部分。

## 总结

那么就这样吧，我要去给电脑重装系统了（每年清理一下系统确保不需要的代码库和依赖什么的可以被彻底清理掉，节省一下空间），希望大家新年快乐，2025 年一切顺利！

{% quot 明年见！GL&HF！ %}
