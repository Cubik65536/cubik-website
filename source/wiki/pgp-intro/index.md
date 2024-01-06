---
layout: wiki
wiki: pgp-intro
title: PGP 介绍
---

## PGP 简介

### PGP 是什么？

PGP 是 Pretty Good Privacy 的缩写，意为“优秀的隐私保护”。是一个被设计用于加密信息（例如文件、邮件等）并保护隐私的开源软件。由 [GNU](https://www.gnu.org/) 开发。目前提到 PGP 大多是指 [OpenPGP](https://www.openpgp.org/) 标准。

### PGP 能用来做什么？

1. 能在 GitHub 上用来验证你的 git commit ~~并且让你的 commit log 更加美观~~

    由于 GitHub 判断 commit 提交者的方式仅仅是通过 git 客户端设置的 `user.name` 和 `user.email` 而不会进行额外的验证，所以其实任何知道你的用户名和邮箱的人都可以伪造你的 commit。但是如果你使用 PGP 签名你的 commit，那么 GitHub 就会在你的 commit log 旁边显示一个 `Verified` 标识，表示该 commit 是经过验证且一定是你。

2. ~~能让你看起来很酷~~ 作为你的身份标识符以及用来加密发送出去的信息

    将 PGP 指纹放置在博客、个人主页、社交媒体账号的自我介绍等地方已经成为了一种风格，当然，这也是一种代表你的身份的方式，让其他人可以增加一个可以安全联系你的方式。同样，PGP 也可以被用来加密邮件等内容，由于 PGP 能保证除了你与你的收件人之外没有人可以解密而去绝对没有被修改，所以它也成为了传输一些敏感信息的一种方式。
