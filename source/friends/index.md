---
robots: noindex,nofollow
sitemap: false
menu_id: friends
title: 友链
seo_title: 友链
toc_title: 友链索引
comments: false
---

## 海内存知己 天涯若比邻

感谢人生旅途中的每一份真挚的友谊。列表按结识先后顺序排列：

{% friends 海内存知己 天涯若比邻 %}

## 常去的地方

经常访问的一些优秀博客，见识到了很多新事物，也学习到了新的知识。列表按结识先后顺序排列：

{% friends 常去的地方 %}

## 开发大佬

特别感谢各位的各种优秀项目，也学到了很多知识。列表按结识先后顺序排列：

{% friends 开发大佬 %}

## 来自 GitHub 的朋友

以下友链通过 [GitHub Issue](https://github.com/Cubik65536/friends-api/issues/) 提交，按 issue 最后更新时间排序：

{% friends api:https://friends-api.blog.cubik65536.top/v2/data.json %}

## 关于友链

**我可以交换友链吗？**

当然可以！但是您的网站应满足以下条件：

- 已添加本站到您的友链中
- 合法的、非营利性、无商业广告
- 有高质量，原创的内容
- `HTTPS` 站点，一些使用免费域名（例如 `cf`, `tk`，但不包括 `vercel.app` 这类托管平台提供的域名）的站点可能会被拒绝


### 如何自助添加友链？

{% timeline %}

<!-- node 第一步：新建 Issue -->

新建 [GitHub Issue](https://github.com/Cubik65536/friends-api/issues/new/choose) 按照模板格式填写并提交。

为了提高图片加载速度，建议优化头像：
1. 将您的头像图片尺寸调整到 `96px`。
2. 将压缩后的图片上传到你希望使用的图床并使用此图片链接作为头像。

<!-- node 第二步：添加友链并等待管理员审核 -->

请添加本站到您的友链中<br/>**如果您也使用 issue 作为友链源，只需要告知您的友链源仓库即可**。

本站友链信息：

{% codeblock lang:yaml %}
title: Cubik的小站
avatar: https://img.cubik65536.top/CubikLogo.png
url: https://cubik65536.top
screenshot: https://img.cubik65536.top/Cubik65536.github.io-Screenshot.png
description: RECOMMENDED BY DR.CREATIVE
{% endcodeblock %}

待管理员审核通过，添加了 `active` 标签后，回来刷新即可生效。

{% endtimeline %}

如果您需要更新自己的友链，请直接修改 issue 内容，大约 3 分钟内生效，无需等待博客更新。如果无法修改，可以重新创建一个
