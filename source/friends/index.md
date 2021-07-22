---
robots: noindex,nofollow
sitemap: false
menu_id: more
seo_title: 友链
toc_title: 友链索引
comments: false
header: false
---

{% friends %}

## 关于友链

**我可以交换友链吗？**

当然可以！但是您的网站应满足以下条件：

- 已添加本站到您的友链中
- 合法的、非营利性、无商业广告
- 有实质性原创内容的 `HTTPS` 站点

{% folding 如何自助添加友链？ %}

{% timeline %}

<!-- node 第一步：新建 Issue -->

新建 [GitHub Issue](https://github.com/Cubik65536/friends-api/issues/new/choose) 按照模板格式填写并提交。

为了提高图片加载速度，建议优化头像：
1. 将您的头像图片尺寸调整到 `96px`。
2. 将压缩后的图片上传到你希望使用的图床并使用此图片链接作为头像。

<!-- node 第二步：添加友链并等待管理员审核 -->

请添加本站到您的友链中<br/>**如果您也使用 issue 作为友链源，只需要告知您的友链源仓库即可**。

本站友链信息：
```yaml
title: Cubik的小站
avatar: https://cdn.jsdelivr.net/gh/Cubik65536/cubik-favicons@main/CubikLogo.png
url: https://cubik65536.top
screenshot: https://img.cubik65536.top/Cubik65536.github.io-Screenshot.png
description: RECOMMENDED BY DR.CREATIVE
```

待管理员审核通过，添加了 `active` 标签后，回来刷新即可生效。

{% endtimeline %}

{% endfolding %}

如果您需要更新自己的友链，请直接修改 issue 内容，大约 3 分钟内生效，无需等待博客更新。如果无法修改，可以重新创建一个。