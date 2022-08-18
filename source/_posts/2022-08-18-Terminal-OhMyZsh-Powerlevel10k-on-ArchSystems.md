---
title: 在 Arch 系列 Linux 操作系统与 macOS 操作系统上上使用 Oh My Zsh 终端并配置 Powerlevel10k 主题等插件
date: 2022-08-18 14:30:27
categories:
  - 折腾
  - 解决方案
tags:
  - 终端
  - Linux
  - macOS
---

本文将会简单介绍如何在 Arch 系列的 Linux 操作系统以及 macOS 操作系统上配置并使用 Oh My Zsh 终端以及 Powerlevel10k 主题等插件。本文中作为演示的终端软件在 Linux 系统上为 [Hyper](https://github.com/vercel/hyper)，在 macOS 操作系统上为 [iTerm2](https://iterm2.com/)。

<!-- more -->

{% grid color:orange 提示 %}

在本文中，如果没有提到具体操作系统，则认为两个操作系统使用同一操作

{% endgrid %}

{% grid color:orange 提示 %}

本文使用的操作、功能、设置等可能具有时效性，仅供参考

{% endgrid %}

{% grid color:orange 提示 %}

本文中的 `Linux` 操作系统指代 Arch Linux 系操作系统（Arch、Artix、Manjaro 等），部分命令可能不适用于其他 Linux 发行。

{% endgrid %}

## 安装字体

Oh My Zsh 会在终端中显示一些徽标，而我们需要安装一个支持这些标志的字体。你可以前往 <https://www.nerdfonts.com/font-downloads> 寻找一个你想要的字体下载并安装。同时，你需要把你的终端软件字体设置为你安装的字体。

## 安裝 Oh My Zsh

首先，我们需要安装 [Oh My Zsh](https://ohmyz.sh/)，执行以下命令：

``` bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

或者

``` bash
sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

## 设置预设终端（可选）

如果再次启动你的终端软件后没有自动使用 zsh，执行以下命令设置：

``` bash
chsh -s $(which zsh)
```

并启动 zsh

``` bash
zsh
```

如果默认终端仍然没有改变，可以尝试重启计算机。

## 安装 Powerlevel10k 主题

然后我们需要安装 Powerlevel10k 主题，执行以下命令：

### Linux

``` bash
yay -S --noconfirm zsh-theme-powerlevel10k-git
echo 'source /usr/share/zsh-theme-powerlevel10k/powerlevel10k.zsh-theme' >> ~/.zshrc
```

### macOS

``` bash
brew install romkatv/powerlevel10k/powerlevel10k
echo "source $(brew --prefix)/opt/powerlevel10k/powerlevel10k.zsh-theme" >> ~/.zshrc
```

## 配置主题

如果你不在 zsh 中，使用 `zsh` 命令启动新的 zsh，否则重启一次终端，Powerlevel10k 主题配置将会显示，你可以通过提示配置你想要的主题。

如果配置页面没有自动弹出，执行 `p10k configure` 即可，如果想更改配置也可以通过此命令完成。

## 安装 [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) 插件

这个插件会在你输入指令时显示曾经使用过的命令，你可以通过按下 {% kbd ➔ %} 来自动补全命令。

执行以下命令安装：

``` bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

## 安装 [zsh-syntax-highlighting](https://github.com/zsh-users/) 插件

这个插件会为你输入的命令增加语法高亮。

执行以下命令安装：

``` bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

## 应用插件

接下来我们需要应用我们安装的主题与插件

### 打开配置文件

``` bash
vim ~/.zshrc
```

### 编辑配置

按 {% kbd I %} 进入编辑模式，增加以下内容：

``` plain
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```

### 保存并应用配置文件

使用以下方法保存文件

1. {% kbd Esc %}
2. {% kbd : %}
3. {% kbd w %}
4. {% kbd q %}
5. {% kbd 回车 %}

使用以下命令应用配置

``` bash
source ~/.zshrc
```

## 完成

Oh My Zsh 以及相关插件的配置就已经完成了！GL&HF!
