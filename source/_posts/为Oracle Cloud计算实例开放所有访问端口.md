---
title: 为Oracle Cloud计算实例开放所有访问端口
date: 2021-01-11 21:37:08
categories:
  - 折腾记录
tags:
  - 服务器
  - Oracle Cloud
---

我最近在家里搭建了一个CentOS服务器（即为上一篇文章，[《使用Visual Studio Code SSH远程访问控制CentOS 7服务器》](https://blog.cubik65536.top/pages/c77340/)里提到的服务器），但是由于没有很好的公网方案，所以我需要一个内网穿透来实现外网访问服务器。经过一段时间的考虑，我决定使用Oracle Cloud免费提供的VM实例来做穿透服务端。但是Oracle Cloud默认只放行远程SSH控制所使用的的22号端口的流量。而不管是内网穿透服务端，或是被穿透的服务本身都需要各种不同的端口，所以我将在此处说一下如何允许所有端口的入站流量。

{% noteblock color:yellow Tips %}

请注意，此处我是为了方便以后的操作开放所有端口，如果你确定你只需要几个端口，为了安全性考虑，我仍然建议您仅开放您所需要的端口。

我在本文中只会提到如何开放所有端口。

{% endnoteblock %}

## 一、前往Oracle Cloud相关设置

登陆Oracle Cloud，点击左上角的三个横杠，选择网络 > 虚拟云网络

![](https://cdn.jsdelivr.net/gh/Cubik65536/Cubik-Image-Hosting-Service/public/assets/img/20210111220113.png)

然后选择实例所使用的虚拟云网络，选择实例所处的子网。如果你不确定的话你可以前往你的计算实例的详细信息页面，直接点击“主要VNIC”下的“子网”后面的子网名称。

## 二、设置安全规则

点击子网页面下方正在使用的安全列表。

![](https://cdn.jsdelivr.net/gh/Cubik65536/Cubik-Image-Hosting-Service/public/assets/img/20210111220606.png)

查看下方的入站规则，由于此处我已进行设置，所以应该和你的界面不同。选择“IP协议”为TCP的那项，点击右边的三个点，选择编辑。

![](https://cdn.jsdelivr.net/gh/Cubik65536/Cubik-Image-Hosting-Service/public/assets/img/20210111220720.png)

在右侧的“IP协议”选择框中，选择“所有协议”（应该是第一个），然后保存更改。

![](https://cdn.jsdelivr.net/gh/Cubik65536/Cubik-Image-Hosting-Service/public/assets/img/20210111221009.png)

你接下来会发现最开始只允许TCP的那项的“IP协议”变成“所有协议”且“允许“一栏显示”所有端口的所有流量“。至此你就完成了所有设置啦！