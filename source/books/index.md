---
robots: noindex,nofollow
menu_id: books
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
rightbar: false
---

{% about %}

{% quot 书籍/漫画 el:h2 %}

{% tabs active:2 %}

<!-- tab 想读 -->

| 封面 | 标题 | 作者 | 出版社 | ISBN | 类型  |
|:----:|:----:|:----:|:------:| :--: | :---: |
|  xx  |  xx  |  xx  |   xx   |  xx  |  xx   |

<!-- tab 在读 -->

{% note color:blue "正在重读的书籍不包含在此列表中" %}

{% library test %}

<!-- tab 已读 -->

{% note color:blue "正在重读的书籍会被包含在此列表中" %}

| 封面 | 标题 | 作者 | 出版社 | ISBN | 类型  |
|:----:|:----:|:----:|:------:| :--: | :---: |
|  xx  |  xx  |  xx  |   xx   |  xx  |  xx   |

{% endtabs %}

{% endabout %}
