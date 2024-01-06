---
title: 更改 git commit 的日期
date: 2023-02-11 02:05:53
cover:
categories:
  - 解决方案
  - 折腾
tags:
  - git
---

本文将简单介绍一下如何更改一个 git 提交的时间戳。

{% note color:warning 注意，本站不建议随意执行本文中提到的任何命令，请仅在必要的情况下使用。 %}

## 将最新的 commit 的时间设置为当前时间

```bash
GIT_COMMITTER_DATE="$(date)" git commit --amend --no-edit --date "$(date)"
```

## 将最新的 commit 的时间设置为指定时间

```bash
GIT_COMMITTER_DATE="Thu Jan 1 00:00:00 1970 -0500" git commit --amend --no-edit --date "Thu Jan 1 00:00:00 1970 -0500"
```

## 将任意 commit 的时间设置为当前或指定时间

1. 使用 `git rebase <commit-hash>^ -i` 命令进入交互式 rebase 模式，其中 `<commit-hash>` 为你想要更改的 commit 的 hash 值
2. 将第一个 commit 开头的 `pick` 改为 `e` (edit)
3. {% kbd esc %} 并 `:wq` 保存退出
4. 更改 commit 的时间戳

    ```bash
    GIT_COMMITTER_DATE="$(date)" git commit --amend --no-edit --date "$(date)"
    ```

    或者

    ```bash
    GIT_COMMITTER_DATE="Thu Jan 1 00:00:00 1970 -0500" git commit --amend --no-edit --date "Thu Jan 1 00:00:00 1970 -0500"
    ```

5. `git rebase --continue` 完成
