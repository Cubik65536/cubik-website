---
title: 在 Cloudflare Pages 上部署 Hexo 站点
date: 2021-09-02 18:21:55
cover: https://blog.cloudflare.com/content/images/2020/12/Cloudflare-Pages.png
categories:
  - 解决方案
  - 教程
tags:
  - Hexo
  - Cloudflare Pages
---

我一直以来都是使用 Vercel 部署我的 Hexo 博客，但是前几天我需要为我的 GitHub 组织创建一个 Hexo 的站点，而 Vercel 部署组织仓库是需要付费的。想当然，像我这种~~白嫖怪~~资深用户是不可能付费的，所以我选择了自带 CDN，主要是免费额度还不少的 Cloudflare Pages。接下来我们来看看如何在不使用 GitHub Action 的情况下在 Cloudflare Pages 上部署 Hexo 站点：

## 为什么不使用 GitHub Action

其实 marketplace 有很多很不错的 Hexo 部署 Action，但是我需要将我主要的 GitHub Action 运行时间分配给其他仓库，而 Cloudflare Pages 免费部署额度是按照调用次数计算的，还算较为划算。

## 配置 package.json

由于 Cloudflare 的部署命令不支持 `&&`，所以我们没有办法直接输入 `npm install && hexo generate`，为了解决这个问题，我们需要在 `package.json` 中添加以下命令：

``` json
"scripts": {
    "build": "hexo clean && hexo generate"
}
```

## 配置 Cloudflare Pages

在将上述更改 commit 并 push 之后，我们就可以照常部署 Cloudflare Pages 了，注意选取正确的仓库和分支，然后在“构建命令”处填写 `npm run build` 即可。

这样我们就可以把 Hexo 部署到 Cloudflare Pages 中啦！GL&HF!
