---
robots: noindex,nofollow
sitemap: false
menu_id: notes
title: "Git 提交信息格式"
comments: false
---

{% about avatar:https://img.cubik65536.top/hello-cubik.png height:96px %}

<br/>

{% navbar active:3 [笔记](/notes/) [网址收藏](/bookmarks/) [Git提交信息格式](/commit-message-conventions/) %}

{% endabout %}

{% note color:light "本页面将简单描述本博客仓库的 git 提交信息格式。" %}

{% note color:warning "commit 中可以包含不完全符合要求情况的更改，但是需要更改与主要符合情况的更改相关（例如为增加文件更改配置文件，commit 信息需要按照增加文件编写）" %}

{% quot 文章的变更 el:h2 %}

在提交文章变更时，提交信息应该遵循以下格式：

```text
[变更类型][文章系列]: 文章标题

(可选) 变更描述
```

其中，变更类型可以是以下几种：

- ADD：添加文章
- DEL：删除文章
- FIX：修复文章
- UPD：更新文章

文章系列通常为 `POST`，即该文章不属于任何系列。如果文章属于某个系列，例如 `月总结`、`LCTT 原创翻译转载`，则此处填写系列名。

文章标题为文章的标题，例如 `使用终端命令别名快速编译运行代码`。如果文章需要前缀，则使用 `[]` 包裹前缀并在后面加空格，例如 `[新闻] Kate 文本编辑器增加了四个非常棒的新功能`。

{% quot 知识库内容的变更 el:h2 %}

{% note color:warning 即将更新 %}

{% quot 单独页面的变更 el:h2 %}

在提交单独页面的变更时，提交信息应该遵循以下格式：

```text
[变更类型][页面标题]: 变更描述
```

其中，变更类型可以是以下几种：

- ADD：添加页面
- DEL：删除页面
- FIX：修复页面
- UPD：更新页面

页面标题为页面的标题，例如 `主页`、`联系我`、`Git 提交信息格式` 等。

{% quot 更新主题 el:h2 %}

更新主题时使用以下内容：

```text
build: Update theme
```

{% quot "更新 Hexo 配置/主题配置" el:h2 %}

在更新 Hexo 配置或主题配置时使用以下内容：

```text
chore(配置文件名): 更改内容描述
```

配置文件名应为 `_config.yml` 或 `_config.stellar.yml`。

{% quot 更新数据文件 el:h2 %}

在更新数据文件时，提交信息应该遵循以下格式：

```text
chore(_data/文件名): 更改内容描述
```

其中文件名应为在 `_data` 目录下的文件名，例如 `widgets.yml`。

{% quot 更新其他文件 el:h2 %}

对于其他文件的更新，假如文件属于内容类型文件（README、LICENSE 等不属于内容类型），则使用以下格式：

```text
[变更类型][文件名]: 变更描述
```

其中，变更类型可以是以下几种：

- ADD：添加文件
- DEL：删除文件
- FIX：修复文件
- UPD：更新文件

如果文件属于 README、LICENSE 或代码类型文件（.gitignore、.editorconfig 等属于代码类型），则依照 [约定式提交](https://www.conventionalcommits.org/zh-hans/v1.0.0/) 编写提交信息。
