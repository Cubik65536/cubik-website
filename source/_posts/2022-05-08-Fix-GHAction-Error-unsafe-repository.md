---
title: "修复 GitHub Action 使用 action-x/commit 时引发的 fatal: unsafe repository ('/github/workspace' is owned by someone else) 错误"
date: 2022-05-08 23:06:48
cover:
categories:
 - 解决方案
tags:
 - GitHub
 - GitHub Action
---

本站友链 API 依赖于 GitHub Action 的 `action-x/commit` Action 来将生成的友链列表 commit 并 push 到 GitHub 仓库。但是在一次新的 issue 被通过过后，该 Action 却出现了 以下错误：

```
fatal: unsafe repository ('/github/workspace' is owned by someone else)
```

该错误通常会在 Fork 仓库中执行 Action 时出现，而将 `action-x/commit` 升级至 `github-actions-x/commit@v2.9` 即可解决问题。
