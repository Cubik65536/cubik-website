---
title: 在 macOS 中将外置键盘的 Option 和 Command 键左右调转
date: 2022-08-01 22:04:27
cover: https://img.cubik65536.top/FD-Command-Key-Gear-Patrol-Lead-Full.jpg
categories:
 - 解决方案
 - 折腾
tags:
 - macOS
 - 外置键盘
 - 键位映射
---

长期以来，我一直为我的 Mac 着一个 Windows 键位格式的外接机械键盘。众所周知，在使用 Windows 键位的时候，Option 和 Command 键是相互对调的。幸好苹果在系统设置中为我们提供了将例如 Option 和 Command 等修饰键重新映射的功能（见下图）。但是，在近期的一些 macOS 版本中，使用当前方法会导致修饰键左右翻转（例如左侧 Command 输出为右侧 Command，右侧 Command 输出为左侧 Command），而这个问题需要进行解决。

{% image https://img.cubik65536.top/macOS-Modifier-Keys.png width:75% %}

<!-- more -->

{% grid color:orange 提示 %}

在本文中，如果没有提到**左/右**侧按键，则默认为两个均可。

{% endgrid %}

{% grid color:orange 提示 %}

本文内容使用英语操作系统的界面

{% endgrid %}

## 提前准备

你需要准备以下东西/软件/网站来帮助你进行接下来的操作

1. 基础的知识与认知
2. 一个可以查看并编辑 macOS plist 文件的工具，比如 [Xcode](https://developer.apple.com/xcode/)，目前极少有其他免费的可视化编辑工具，当然，如果你愿意的话，你也可以尝试 [VScode 的 Binary Plist](https://marketplace.visualstudio.com/items?itemName=dnicolson.binary-plist) 插件，或者 [TextMate for macOS](https://macromates.com) 等软件。但是后两者虽然同样是免费的，但不提供可视化编辑工具，它们分别会以一个类似于 XML 和 Json 的格式显示 plist 的内容，所以你需要相关知识。
3. 一个测试按键功能的网站，例如 ["Key-Test" - keyboard test online](https://en.key-test.ru)。
4. 一个十六进制转十进制的工具。或者你也可以使用命令行完成这个任务（见下章）。

那让我们开始吧！

## 使用 bash 命令行将十六进制数转换为十进制

打开你的命令行（比如 Terminal），输入以下命令：

```bash
echo $((16#046D))
```

随后输出的结果即为 `0x046D` 的十进制表示。

## 找到你键盘的 VendorID（厂商ID）与 ProductID（硬件/产品ID）

{% grid color:orange 提示 %}

我使用的是一个 USB 无线 + 有线均可使用的键盘，蓝牙键盘可能需要其他方法找到 ID

{% endgrid %}

{% grid color:orange 提示 %}

从这一步开始，你需要接入你的外接键盘

{% endgrid %}

这一步我们的目的是找到键盘在系统中的标识符，以便我们找到键盘映射设置。

按住 {% kbd ⌥ %}，点击屏幕左上角的 Apple 图标，点击 `System Information`（第一个选项）。

在左侧菜单中找到 `Hardware` 下方的 `USB`，点击它，在右侧就会显示已连接到电脑的 USB 设备，找到你的键盘（作为参考，在我这里有线模式显示为 `G915 WIRELESS RGB MECHANICAL GAMING KEYBOARD`，无线模式显示为 `USB Reciver`），点击它。

{% image https://img.cubik65536.top/KeyboardSystemInformation.png %}

你在这里会找到你的键盘的十六进制 VendorID 和 ProductID。将其转换成十进制并记录下来。

{% grid color:orange 提示 %}

假如你的键盘同时支持有线和无线模式，那么 USB 线和 USB 无线接收器会显示为两个不同的 ID，需要单独设置，所以两者均需要记录

{% endgrid %}

## 编辑 plist 文件

前往 `~/Library/Preferences/ByHost/` 路径，找到 `.GlobalPreferences.XXXXXXXX-YYYY-ZZZZ-WWWW-VVVVVVVVVVVV.plist` 格式的 plist 文件，打开它。

{% grid color:orange 提示 %}

从此处开始，我们会提到 `Array`/`Dictionary`/`Number` 等数据类型，如果你使用 XCode，则可以通过查看 `Type` 一栏来获得数据类型，如果你使用 VSCode，则类型为 XML 标签的命名，如果你使用 TextMate，则需要按照 Json 的方法辨认这些类型。

{% endgrid %}

### 已经进行过键位设置

接下来，我们会提到多次 “找到”，但是这是建立在你已经在系统偏好设置中设置了键位的猜测下的，假如你确定你没有这一项，则跳到第二部分。

你需要在文件中找到一个到多个 `com.apple.keyboard.modifiermapping.XXXX-YYYYY-0` 格式命名的 `Array`。其中 `XXXX` 是你的键盘的十进制 VendorID，`YYYY` 是你的键盘的十进制 ProductID。其中，会有命名为 `Item Z`（`Z` 是一个数）的数个（至少四个） `Dictionary`。其中，你会找到 `HIDKeyboardModifierMappingSrc` 和 `HIDKeyboardModifierMappingDst` 两个 `Number`。其中，`HIDKeyboardModifierMappingSrc` 是你的原键位，`HIDKeyboardModifierMappingDst` 是你想要的新键位。

接下来，按照下方对应的表格将对应原键位的新键位设置到你的键盘上。注意不要看错原键位哦！

|     原键位     |     新键位     |
| -------------- | -------------- |
| 30,064,771,302 | 30,064,771,303 |
| 30,064,771,303 | 30,064,771,302 |
| 30,064,771,298 | 30,064,771,299 |
| 30,064,771,299 | 30,064,771,298 |

最后保存即可。

### 没有进行过键位设置

其实这个步骤与上一步差不多，但是，你需要自行按照相应命名创建 `Array`，`Dictionary`，`Number` 并按照上方表格自行填写 `HIDKeyboardModifierMappingSrc` 和 `HIDKeyboardModifierMappingDst` 的值。创建完成后保存即可。

## 最后

在上述步骤都完成后，重启电脑，并打开按键测试网站进行测试，然后就大功告成啦！GL&HF！
