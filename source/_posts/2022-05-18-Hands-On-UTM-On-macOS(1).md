---
title: macOS 版 UTM 虚拟机上手（一）
date: 2022-05-18 20:15:37
cover: https://img.cubik65536.top/UTM-on-macOS-screenshot.png
categories:
  - 日常
  - 折腾
tags:
  - 上手
  - 虚拟机
  - UTM
  - macOS
---

最近我有了在我的 macOS 设备上运行 Linux 的需求，而我认为 Parallel Desktop 和 VMware Fusion 都体积较大。但我突然想到了 UTM 软件。它以可以在 iOS 上运行虚拟机而闻名，但是这个软件也有 macOS 版本，而且相对其他虚拟机软件来说，它更轻量化，所以我打算尝试一下该软件。

<!-- more -->

## 安装 UTM

前往 UTM app 的 [GitHub Release](https://github.com/utmapp/UTM/releases) 页面即可下载最新版本的 dmg 文件。然后就和我们都很熟悉的 macOS 软件安装方法一样了：打开 dmg，将 app 拖入 Application（软件）目录下即可。

## 启动 UTM

{% image https://img.cubik65536.top/UTM-First-Time-Start.png %}

初次启动 UTM 之后，你将可以看到这个页面，你分别可以创建新虚拟机，浏览虚拟机预设，查看用户手册或者获得支持。

## 创建新虚拟机

在点击 `Create a New Virtual Machine` 之后，你将能看见这个页面：

{% image https://img.cubik65536.top/UTM-New-VM.png %}

你可以选择虚拟化（Virtualize），这个模式下虚拟机效率会更高，但是只能模拟同 CPU 架构的虚拟机（例如在我的 Intel Mac 上，它就只能虚拟 x86 架构的虚拟机）。

或者你也可以选择模拟化（Emulate），虽然速度会更慢，但是你可以建立跨架构的模拟机（例如在 Intel Mac 上模拟 arm 架构芯片的虚拟机）。

根据我的需求，我这次会选择虚拟化，不过我可能会在以后为各位带来模拟化模式的体验。

------

然后在选择 Virtualize 之后，你会需要选择操作系统。正如下图所示，你可以选择预配置好的 Windows 或者 Linux，你也可以选择其他，这里我会选择 Linux。

{% image https://img.cubik65536.top/UTM-New-VM-OS.png %}

然后你会看到这个页面：
  
{% image https://img.cubik65536.top/UTM-New-VM-OS-Linux.png %}

在这里，你可以选择是否使用 Apple 的虚拟化（这个模式仍然比较的不完善，所以仍然推荐不选择，使用自带的 QEMU，这也是 UTM 推荐的模式）。你还可以选择是否要从 Linux 内核启动（我这里会保持默认，则不从内核启动），然后你需要导入启动 ISO 文件，我这里使用了 Fedora 36 + KDE Desktop 的 ISO。没错，我又从 Manjaro 换回了 Fedora，我会在近期更新的另一篇博客中更加详细的解释。

------

然后进入硬件页面（如下图）：
  
{% image https://img.cubik65536.top/UTM-New-VM-Hardware.png %}

我选择了让 CPU 核心保持默认，将内存分配从默认的 4096 MB 增加到了 8192 MB，仍然在试验阶段的 OpenGL 加速保持默认的关闭状态。

然后选择硬盘大小（如下图）：
  
{% image https://img.cubik65536.top/UTM-New-VM-Storage.png %}

我目前不需要太大的存储空间，而且我们可以回收虚拟机所不占用的空间，所以我将其维持默认的 64 GB 设置。

然后选择共享的路径：

{% image https://img.cubik65536.top/UTM-New-VM-SharedDirectory.png %}

然后来到 Summary 页面：

{% image https://img.cubik65536.top/UTM-New-VM-Summary.png %}

这里你可以再检查一遍虚拟机配置并改变你虚拟机的名称，然后点击 `Save` 创建虚拟机。

## 主页面

{% image https://img.cubik65536.top/UTM-Home.png %}

然后你会来到这个页面，点击左上角的三角运行即可

## 安装操作系统

然后 UTM 理论上会引导到你的 ISO 中，正常安装操作系统到虚拟磁盘中即可。

## 在外置磁盘上存储 UTM 的虚拟机

目前 UTM 只会从 `~/Library/Containers/com.utmapp.UTM/Data/Documents` 读取虚拟机文件，目前的一个解决方案是使用终端将移动硬盘手动挂载到该目录下。例如：

``` bash
sudo diskutil mount -mountPoint ~/Library/Containers/com.utmapp.UTM/Data/Documents /dev/disk4s3
```

{% note color:orange 注意 此处的 `/dev/disk4s3` 应当被替换为你的外置硬盘的标识符，标识符可以通过 `diskutil list` 查看。 %}

------

未完待续... 我将会在不久之后发布体验博客，请敬请期待。
