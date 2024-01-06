---
title: "[LCTT 原创翻译转载] 新闻｜macOS 替代品 helloSystem 0.7.0 正在增强稳定性"
poster:
  topic: LCTT 原创翻译转载
  headline: "新闻｜macOS 替代品 helloSystem 0.7.0 正在增强稳定性"
  caption: 随着 helloSystem 0.7.0 的发布和更多内部工作，helloSystem 正在增强稳定性，为 macOS 提供一个“开源”的替代方案。
  color: 标题颜色
date:  2022-11-29 16:04:00
cover: https://img.linux.net.cn/data/attachment/album/202211/29/160730l55qqz753b5bfhhq.jpg
categories:
 - LCTT
 - 转载
 - 翻译
tags:
 - macOS
 - helloSystem
 - BSD
license: 本文采用 [署名 - 相同方式共享 4.0 国际](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 许可协议，转载请注明出处。
---

> 原文：[macOS Alternative helloSystem (0.7.0) is moving towards stability](https://www.debugpoint.com/hellosystem-0-7-0/)
> 首发：[macOS 替代品 helloSystem 0.7.0 正在增强稳定性](https://linux.cn/article-15300-1.html) @[Linux 中国](https://linux.cn/)
> 作者：[Arindam](https://www.debugpoint.com/hellosystem-0-7-0/)
> 译者：[LCTT](https://linux.cn/lctt/) [Cubik](https://linux.cn/lctt/Cubik65536)

<!-- more -->

## macOS 替代品 helloSystem 0.7.0 正在增强稳定性

> 随着 helloSystem 0.7.0 的发布和更多内部工作，helloSystem 正在增强稳定性，为 macOS 提供一个“开源”的替代方案。

helloSystem 是一个基于 FreeBSD 的轻量级操作系统，旨在为 macOS 提供一个“开源”的替代方案。helloSystem 的主要目标是提供一个易于安装和使用的，真正“开源”的 FreeBSD 替代方案。此外，该团队还专注于从 macOS 转换过来的用户，他们可以舒适的使用类似的桌面，而不会受到系统锁定或 Linux 发行版的复杂性的影响。

考虑到 BSD 系统中的硬件支持，开发这样的操作系统需要时间。团队正在努力从零创建一个桌面 - “hellodesktop”。用 C++ 编写的 hellodesktop 和其他改进正在进行中。

![helloSystem 0.7.0 桌面][1]

### helloSystem 0.7.0

在 2021 年底，该团队发布了最新一个稳定版本，基于 FreeBSD 13.0 的 [helloSystem 0.7.0][2]，并且目前正在将系统移植到 FreeBSD 13.1 和 14 版本。

除此之外，一些新功能也可以在系统中工作了；这是一个总览：

- 由 FreeBSD 13.0 驱动
- 通过更新的启动时间和压缩的 UFS 文件系统改进的<ruby>现场介质<rt>Live Media</rt></ruby>
- 将 ISO 镜像大小减少到 800 MB，以适合 CD
- 与 Ventoy USB creator 兼容
- 支持英伟达 GPU，包括旧型号
- 在目标分区安装时添加了 exFAT 支持
- 增加了 KDE 开发的 Falkon 浏览器，附带了下载和安装 Chromium 和 Firefox 的附加菜单项
- 系统提示音
- 支持通过笔记本电脑键盘的专用键控制亮度和音量

![helloSystem 0.7.0 中的菜单与应用][3]

除了上述内容之外，helloSystem 0.7.0 中还添加了一个新部分，以让你可以提前了解团队正在开发的功能。未来版本中将到来的一些最酷的功能包括：

- BSD 中的 Debian 运行时安装程序，以在 BSD 中运行 Linux 应用程序！
- 一个独立的应用程序，用于下载应用程序。
- 新的更新实用程序

此外，0.7.0 中还修复了一些错误并更新了翻译。

![安装 Linux 运行时正在开发中][4]

话虽如此，但它仍然需要大量的时间才能成为易于使用的 BSD 变体和 macOS 的“开源”替代方案。自从我 [首次报道][5] 以来，已经在图形安装程序，桌面应用程序和错误修复方面进行了巨大的更新。我希望它在随着 2023 年移植到 FreeBSD 14 之后，会变得更加主流。

### 下载

如果你想在真实的硬件上尝试它，请先进行备份然后尝试。此外，helloSystem 现在完全兼容 [VirtualBox][6] 之类的虚拟机。你可以试试。如果你在 VirtualBox 中尝试，请确保将 CPU 更改为 64 位。

官方下载链接（包括 helloSystem 0.7.0 稳定版）可在 [此页面][7] 上找到，或者您可以使用下面的链接获取 ISO。

> **[下载 helloSystem 0.7.0][8]**

如果你想为测试、开发或任何其他事情做出贡献，请 [访问 GitHub 页面以获取详细信息][9]。

--------------------------------------------------------------------------------

via: https://www.debugpoint.com/hellosystem-0-7-0/

作者：[Arindam][a]
选题：[lkxed][b]
译者：[Cubik65536](https://github.com/Cubik65536)
校对：[wxy](https://github.com/wxy)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux 中国](https://linux.cn/) 荣誉推出

--------------------------------------------------------------------------------

## 预告

另外，我最近开始尝试 [Arc Browser](https://thebrowser.company/) 作为主要浏览器来使用，我会在接下来几天内发布一个文章讲讲第一印象，我也会尝试使用 Arc 作为主要浏览器一段时间，并带来一个更完整的体验文章。

[a]: https://www.debugpoint.com/author/admin1/
[b]: https://github.com/lkxed
[1]: https://www.debugpoint.com/wp-content/uploads/2022/11/helloSystem-0.7.0-Desktop.jpg
[2]: https://github.com/helloSystem/ISO/releases/tag/r0.7.0
[3]: https://www.debugpoint.com/wp-content/uploads/2022/11/Menu-and-apps-in-helloSystem-0.7.0.jpg
[4]: https://www.debugpoint.com/wp-content/uploads/2022/11/Installing-Linux-Runtime-is-under-development.jpg
[5]: https://www.debugpoint.com/tag/hellosystem
[6]: https://www.debugpoint.com/tag/virtualbox
[7]: https://github.com/helloSystem/ISO/releases
[8]: https://github.com/helloSystem/ISO/releases/download/r0.7.0/hello-0.7.0_0G160-FreeBSD-13.0-amd64.iso
[9]: https://github.com/helloSystem
