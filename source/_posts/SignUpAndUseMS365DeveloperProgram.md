---
title: 注册 MS 365 Developer Program 并确保它可以长期使用
date: 2021-07-22 21:03:40
cover: https://img.cubik65536.top/EnterYourInfo.png
categories:
  - 日常
  - 折腾
tags:
  - Microsoft 365
  - 常用工具
---

{% note color:yellow 提示 本文有部分参考、引用 [Restent Ou](https://blog.restent.win) 的 [《如何续订 MS 365 E5 开发者计划》](https://blog.restent.win/2021/07/22/Renew-MS-365-E5-plan/)一文。
本文有使用该《如何续订 MS 365 E5 开发者计划》一文的图片。~~才不是懒得自己截图了呢~~
《如何续订 MS 365 E5 开发者计划》一文基于[CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/)协议 %}

相信不少人都听说过 Microsoft 365 Developer Program，或者说 Microsoft E5。这是微软为开发人员推出的计划，目的是可以为开发者提供全套的 Microsoft 365 套件。当然不少人（包括我的朋友 Restent）都依靠着这个计划为自己团队提供带有域名的邮箱访问。

在本文中，我将提到如何注册这个计划，以及如何一直延续这个项目的可用性。

## 一、注册 Microsoft 365 Developer Program

### 进入官网

首先，前往 [Microsoft 365 Developer Program 官网](https://developer.microsoft.com/zh-cn/microsoft-365/dev-program) 并点击`立即加入`

### 登录你的 Microsoft 账户

在这里，你需要选择一个微软账户（你的 hotmail 或 outlook 账户）并登录

### 设置你的 E5 订阅

现在，你应该会进到`Microsoft 365 开发人员计划`页面，点击`设置 E5 订阅`。然后你会看到下方这样的页面：

![SetupE5.png](https://img.cubik65536.top/SetupE5.png)

### 填入你的基本信息

在此处，你应该会看到一个让你填写信息的页面，选择你的国家，填写你的用户名和域。注意，这里的域会生成一个 `你输入的东西`+`.onmicrosoft.com` 组成的域名，如果你希望使用你自己的域名，需要等到注册完成之后进入管理页面设置。然后写好你的密码。这里填写的信息就是你的 E5 订阅管理员的账户信息。

然后验证你的手机号码。

然后你会看到下方这样的页面：

![EnterYourInfo.png](https://img.cubik65536.top/EnterYourInfo.png)

{% note color:red 注意 我的账户**已经**与我的 GitHub 账户进行了关联，所以上方会有 GitHub 徽标，新注册用户是没有该徽标的！ %}

到这里你就设置成功了，你接下来可以到 [Microsoft 365 Admin Center](https://admin.microsoft.com) 处登录你刚刚注册的，以`onmicrosoft.com`结尾的账户。在那个站点上，你可以添加用户，域名以及做出其他设置。这些以后再讲。

## 二、续期你的 E5 订阅

微软不可能就这么往外送订阅对吧？微软给 Microsoft 365 Developer Program 成员续期是有要求的。目前已知要求是“持续有开发活动”。但我并未找到任何资料直接解释如何衡量开发活动是否足够。

或许你知道以前有各种 GitHub 仓库来帮助你续期该计划，但是这种东西说挂就挂。所以接下来解释一个更靠谱一点的方案：关联 GitHub 活动

### 关联 GitHub

如果你尚未关联 GitHub，则你应该可以在你的仪表盘界面上方看到这样的提示：

> 新增内容！将开发人员帐户与 GitHub 帐户链接。GitHub 活动将订阅续订 Microsoft 365 开发人员版。

点击 `链接账户` ，或者点击右上方的小齿轮，再点击左侧的 `已链接的账户`

![设置](https://i.yecdn.com/images/2021/07/22/8b0c8d5b03f5c7fc11bed2ba6628f7ab.jpg)

![已链接的账户](https://i.yecdn.com/images/2021/07/22/2358b10f0d2aabb4877d76f7aef922c5.jpg)

点击 `同意并链接 Github 账户`，并在 Github 认证即可。认证完成后你将会看到：

![认证后](https://i.yecdn.com/images/2021/07/22/8965973c8c257c9cbe2013f39b55d2ab.jpg)

这样，只要你经常有 GitHub 活动（commit，issue 和 pull request 等全部被计算在内），你大概率就会获得续期。

------

就是这样！那我们，下次再见～
