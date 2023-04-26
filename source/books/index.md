---
robots: noindex,nofollow
menu_id: home
sidebar: [ghuser, search, recent, timeline]
comments: true
comment_title: ""
comment_locale:
    placeholder: "欢迎留言（填写邮箱可以在回复时收到邮件通知）"
    sofa: "沙发！来留言吧～"
    comment: "留言"
    wordHint: "留言字数应在 $0 到 $1 字之间！\n当前字数：$2"
reaction: false
breadcrumb: false
header: false
---

{% about %}

{% quot 书籍 el:h2 %}

<br/>

{% navbar active:6 [关于](/) [联系我](/contact-me/) [我的&nbsp;Github](/my-github/) [我的&nbsp;PGP](/my-pgp/) [游戏](/games/) [书籍](/books/) [影视](/movies/) [留言板](/message-board/) %}

{% tabs active:2 align:center %}

<!-- tab 想读 -->

| 封面 | 标题 | 作者 | 出版社 |
|:----:|:----:|:----:|:------:|
|  xx  |  xx  |  xx  |   xx   |

<!-- tab 在读 -->

{% note color:blue "正在重读的书籍不包含在此列表中" %}

| 封面 | 标题 | 作者 | 出版社 | 推荐指数 |
|:----:|:----:|:----:|:------:|:--------:|
|  xx  |  xx  |  xx  |   xx   |  ★★★★☆  |

<!-- tab 已读 -->

{% note color:blue "正在重读的书籍会被包含在此列表中" %}

| 封面 | 标题 | 作者 | 出版社 | 推荐指数 |
|:----:|:----:|:----:|:------:|:--------:|
|  xx  |  xx  |  xx  |   xx   |  ★★★★☆  |

{% endtabs %}

{% endabout %}
