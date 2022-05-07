---
title: 我为什么选择 Flarum 作为我的论坛框架，以及一个免费的 Flarum 托管：FreeFlarum
date: 2022-05-07 12:50:10
cover: https://img.cubik65536.top/flarum-cover.png
categories:
 - 解决方案
tags:
 - 论坛
 - Flarum
---

最近我投入了大量的时间精力到我的新 Minecraft 服务器 [MC@HMU](https://mc.hmu.ac.cn) 上（网站仍未完成），而我也需要为这个服务器建立一个论坛以提供审核申请等服务。在警告一些考虑之后，我选择了 [Flarum](https://flarum.org)（[GitHub](https://github.com/flarum)） 作为我的论坛框架，接下来我来讲讲为什么我做出这个选择，以及介绍一个简单快速的 Flarum 托管平台，[FreeFlarum](https://github.com/flarum)。

<!-- more -->

## Flarum 的优点

首先，我想来讲讲 Flarum 的优点，我稍后还会讲讲 Flarum 的缺点，你可以依照我的情况综合考虑。

1. 美观：

    以 [Flarum 中文社区论坛](https://discuss.flarum.org.cn/) 为例：
    
    {% image https://img.cubik65536.top/flarum-cn-discuss.png %}

    相对于大部分其他论坛框架来说，Flarum 是相对简洁美观的，而这是我选择论坛框架的时候考虑的首要因素，因为我认为网站的美观性会很影响使用和浏览体验。

2. 超强的自定义能力：

    Flarum 支持使用 HTML 自定义页眉页脚，支持自定义 CSS，也支持在各种需要图标的地方使用 [FontAwsome](https://fontawesome.com)，所以这是很吸引人的。

3. 强大的插件能力：

    Flarum 支持各种插件，你可以加入各种语言，以及可以加入文章标签，FancyBox，头衔，查看编辑记录甚至是封禁 IP 等功能的支持，这是一个非常优秀的特性。而 Flarum 还拥有完善的插件开发群体，让这个体验进一步优化。

4. 轻量化：

    Flarum 在设计之初就考虑到了轻量化，而这点还会与下一点相结合。

5. 速度：

    由于 Flarum 是很轻量化的，其加载速度本就拥有优势，如果使用合适的 CDN，其响应速度是非常优秀的。

6. 部署：

    Flarum 和其插件可以使用 docker pull 或者 composer 安装，所以整体部署是相对简单的。

## Flarum 的缺点

以上就是 Flarum 的优点，但对应的，Flarum 也有不少缺点，你还是需要综合考虑你的需求来决定。

1. 过于依赖插件

    是的，拥有插件是个好事，但是 Flarum 本体其实缺少很多功能，虽然这些功能可以通过插件实现，但是过于依赖插件可能会在开发者停止维护或者出现兼容性问题时造成严重的问题。

2. 缺少一些设计

    继续上一点，如果没有插件，Flarum 甚至缺少让用户将图片上传到你服务器的功能，而这种严重的功能缺失还有很多，就不列举了。

3. 兼容性问题

    Flarum 插件在我自行部署的时候出现过不少兼容性问题，而解决这些问题相当令人头大。


## 为什么选择 Flarum？

为什么选择 Flarum？原因就是，我很喜欢 Flarum 的这些优点，而我也相对可以忍受这些缺点，所以 Flarum 被我选择成为我的主要论坛框架了。

## FreeFlarum

[FreeFlarum](https://github.com/flarum) 是一个免费的 Flarum 托管服务。你只要拥有一个邮箱就可以注册并拥有你自己的 Flarum 论坛而不需要经历繁琐的服务器配置。[MC@HMU](https://mc.hmu.ac.cn) 的论坛 [MC@HMU BBS](https://bbs.mc.hmu.ac.cn) 就托管在此。

FreeFlarum 在初始化时为你建成的论坛也包含了大部分你会需要的插件，而不需要你自己去找到并安装。返回到前两点缺点，FreeFlarum 直接提供了插件的服务相对的缓解了这些缺点的带来的麻烦。同时，Flarum 本体和插件的更新由 FreeFlarum 负责，所以也相对的缓解了第三点缺点带来的问题。同时，FreeFlarum 还提供了免费 SSL，以及一个 Cloudflare CDN（实测速度还可以接受），所以 FreeFlarum 对于懒得折腾的人是个不错的选择。

但 FreeFlarum 也有缺点，例如你需要赞助才可以移除 `A free forum powered by FreeFlarum` 的页脚或者添加非内置的插件。而你的论坛也可能会因为长期无访问而被存档（可以恢复）。但是考虑到 FreeFlarum 是公益项目，而且服务器等资源确实需要资金，所以我认为可以接受这些缺点。

------

希望本文给你提供了一些参考，GL&HF！
