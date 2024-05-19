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
---

<link rel="stylesheet" href="/css/book.css" />

{% about %}

{% quot 书籍/漫画 el:h2 %}

{% tabs active:2 align:center %}

<!-- tab 想读 -->

| 封面 | 标题 | 作者 | 出版社 | ISBN | 类型  |
|:----:|:----:|:----:|:------:| :--: | :---: |
|  xx  |  xx  |  xx  |   xx   |  xx  |  xx   |

<!-- tab 在读 -->

{% note color:blue "正在重读的书籍不包含在此列表中" %}

<div class="page">
  <!-- 每个li标签内容代表一本书籍的所有信息 -->
  <div class="info">
    <div class="image" title="无职转生">
      <img
        src="https://cdn.jsdelivr.net/npm/xiansakana-blog-assets/blog-assets/%E6%97%A0%E8%81%8C%E8%BD%AC%E7%94%9F/images/cover.jpg"
      />
    </div>
    <div class="info-card">
      <!-- <a href="https://cdn.jsdelivr.net/npm/xiansakana-blog-assets@1.0.1/blog-assets/%E6%97%A0%E8%81%8C%E8%BD%AC%E7%94%9FWEB_compressed.pdf" target="_Blank">                           -->
      <h2>
        <a href="无职转生/无职转生.html" target="_Blank"> 《无职转生》 </a>
      </h2>
      <h4>作者：理不尽な孫の手</h4>
      <h4>出版时间：</h4>
      <p>2014-01-24——2022-11-25</p>
      <h4>
        <span>推荐指数：</span>
        <span>
          <!-- 推荐指数，多少个星星就有以下多少个i标签 -->
          <i class="fas fa-star"></i><i class="fas fa-star"></i
          ><i class="fas fa-star"></i><i class="fas fa-star"></i
          ><i class="fas fa-star"></i>
        </span>
      </h4>
      <p class="text">
        34岁无职童贞尼特身无分文地被赶出家门，发现自己的人生已经完全走投无路。刚刚产生后悔的想法，他就被卡车撞死了。然后醒来的地方居然是——剑与魔法的异世界！！重生为名叫卢迪乌斯的婴儿的他下定决心，“这次一定要认真地活下去……！”，一定要度过一段不会后悔的人生。他利用前世的智力很快使得自己的魔法的才能开花结果，结果一位年轻的女孩子成了自己的家庭教师。并且又与一位有着绿宝石般美丽秀发的四分一血统的精灵相遇。他崭新的人生开始前进。——让人憧憬的转生型奇幻小说，在这里开始。
      </p>
    </div>
  </div>
</div>

<hr />

<div class="page">
  <!-- 每个li标签内容代表一本书籍的所有信息 -->
  <div class="info">
    <div class="image" title="无职转生">
      <img
        src="https://cdn.jsdelivr.net/npm/xiansakana-blog-assets/blog-assets/%E6%97%A0%E8%81%8C%E8%BD%AC%E7%94%9F/images/cover.jpg"
      />
    </div>
    <div class="info-card">
      <!-- <a href="https://cdn.jsdelivr.net/npm/xiansakana-blog-assets@1.0.1/blog-assets/%E6%97%A0%E8%81%8C%E8%BD%AC%E7%94%9FWEB_compressed.pdf" target="_Blank">                           -->
      <h2>
        <a href="无职转生/无职转生.html" target="_Blank"> 《无职转生》 </a>
      </h2>
      <h4>作者：理不尽な孫の手</h4>
      <h4>出版时间：</h4>
      <p>2014-01-24——2022-11-25</p>
      <h4>
        <span>推荐指数：</span>
        <span>
          <!-- 推荐指数，多少个星星就有以下多少个i标签 -->
          <i class="fas fa-star"></i><i class="fas fa-star"></i
          ><i class="fas fa-star"></i><i class="fas fa-star"></i
          ><i class="fas fa-star"></i>
        </span>
      </h4>
      <p class="text">
        34岁无职童贞尼特身无分文地被赶出家门，发现自己的人生已经完全走投无路。刚刚产生后悔的想法，他就被卡车撞死了。然后醒来的地方居然是——剑与魔法的异世界！！重生为名叫卢迪乌斯的婴儿的他下定决心，“这次一定要认真地活下去……！”，一定要度过一段不会后悔的人生。他利用前世的智力很快使得自己的魔法的才能开花结果，结果一位年轻的女孩子成了自己的家庭教师。并且又与一位有着绿宝石般美丽秀发的四分一血统的精灵相遇。他崭新的人生开始前进。——让人憧憬的转生型奇幻小说，在这里开始。
      </p>
    </div>
  </div>
</div>
<!-- tab 已读 -->

{% note color:blue "正在重读的书籍会被包含在此列表中" %}

| 封面 | 标题 | 作者 | 出版社 | ISBN | 类型  |
|:----:|:----:|:----:|:------:| :--: | :---: |
|  xx  |  xx  |  xx  |   xx   |  xx  |  xx   |

{% endtabs %}

{% endabout %}
