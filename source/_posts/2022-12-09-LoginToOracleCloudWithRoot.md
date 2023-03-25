---
title: 如何使用 root 用户登录 Oracle Cloud
date: 2022-12-09 23:56:19
cover:
categories:
  - 折腾记录
tags:
  - 服务器
  - Oracle Cloud
  - Linux
---

众所周知，Oracle Cloud 默认只允许用户以 opc / ubuntu 用户名登陆，但是有的时候直接使用 root 账户会更加的方便，所以本文将会简单介绍一下如何启用 Oracle Cloud 服务器的 root 用户登录。

{% note color:warning 注意 本文不会提及如何使用密码或者 `keyboardinteractive` 登陆的方法。实际上，我们非常不建议允许使用密码或者 `keyboardinteractive` 登陆 root 这种高敏感高权限账户，因为这样会使得服务器的安全性大大降低。本文在结尾也会包含如何只允许使用 ssh key 登陆的配置步骤。 %}

## 编辑 ssh 证书

首先，你需要使用你的默认用户名登陆，然后，编辑 `.ssh/authorized_keys` 文件，将文件中 `ssh-rsa` 前的所有内容全部删除，如果你本地有 ssh 公钥的话也可以直接将文件清空并替换为你的公钥内容。

对于 CentOS / Oracle Linux 等默认用户名为 `opc` 的操作系统来说，该文件在两处储存：`/home/opc/.ssh/authorized_keys` 和 `/root/.ssh/authorized_keys`，所以需要确保两个文件都只有公钥内容。

{% note color:green "***(2023-03-25 更新)***<br/> 上方的 `/home/opc` 中的 `opc` 即为操作系统默认用户名，对于默认用户名不是 `opc` 的系统来说，你需要将该路径名改为对应默认用户名（例如 Rocky Linux 系统的默认 `rocky`。<hr/> 增加本提醒的原因是发现 Oracle 直接提供了例如 Rocky 的其他系统，由于我个人偏好 Rocky，所以重新开了一个机器使用 Rocky。但是发现了不同系统的默认用户名是不一样的，所以增加了该内容。" %}

对于 Ubuntu 来说，该文件在三处储存：`/home/ubuntu/.ssh/authorized_keys`、`/home/opc/.ssh/authorized_keys` 和 `/root/.ssh/authorized_keys`，所以需要确保三个文件都只有公钥内容。（`opc` 的可以不进行更改，因为改变之后也仅仅是允许用户使用 `opc` 用户名登陆，对我们的需求影响不大。）

## 编辑 sshd 配置

然后，我们需要修改 sshd 的配置文件，使得 sshd 允许 root 用户登陆。使用以下命令编辑配置文件：

``` bash
sudo vim /etc/ssh/sshd_config
```

随后，找到以下两项配置，如果前面有 `#` 则去掉，并全部修改为以下内容：

``` bash
PermitRootLogin prohibit-password
PasswordAuthentication no
```

这样可以保证只能使用 ssh key 才可以登陆 root 用户，而且不允许使用密码登陆任何用户，保证了服务器的安全性。

## 重启

最后你可以使用以下命令重启 sshd 服务，使得配置生效：

``` bash
service sshd restart
```

或者你也可以直接使用以下命令重启服务器：

``` bash
sudo reboot
```

大功告成啦！你现在可以使用 root 用户登录你的 Oracle Cloud 服务器了！GL & HF！
