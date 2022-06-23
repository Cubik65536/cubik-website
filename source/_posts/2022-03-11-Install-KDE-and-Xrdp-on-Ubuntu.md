---
title: 在 Ubuntu 20.04 上安装 KDE desktop 并使用 Xrdp 远程连接
date: 2022-03-11 20:29:18
cover: https://upload.wikimedia.org/wikipedia/commons/b/b6/Kubuntu_21.10_Desktop.png
categories:
  - 解决方案
tags:
  - Linux
  - 折腾
---

在本文中，我将简单的带大家了解如何在 Ubuntu 20.04.3 LTS 上安装 KDE desktop 桌面环境并使用 Xrdp 进行远程桌面连接

<!-- more -->

## 1. 安装 KDE desktop 桌面环境

在命令行中执行以下命令：

```bash
sudo apt upgrade
sudo apt update
cat <<EOF | sudo tee /etc/X11/default-display-manager
    /usr/bin/sddm
    EOF
sudo DEBIAN_FRONTEND=noninteractive apt install -y kubuntu-desktop
```

KDE desktop 就安装完成啦！接下来重启：

``` bash
sudo reboot
```

## 2, 安装 Xrdp

在命令行中执行以下命令：

``` bash
sudo apt install -y xrdp
sudo sed -e 's/^new_cursors=true/new_cursors=false/g' \
     -i /etc/xrdp/xrdp.ini
sudo systemctl enable xrdp
sudo systemctl restart xrdp

echo "startplasma-x11" > ~/.xsession
D=/usr/share/plasma:/usr/local/share:/usr/share:/var/lib/snapd/desktop
C=/etc/xdg/xdg-plasma:/etc/xdg
C=${C}:/usr/share/kubuntu-default-settings/kf5-settings

cat <<EOF > ~/.xsessionrc
    export XDG_SESSION_DESKTOP=KDE
    export XDG_DATA_DIRS=${D}
    export XDG_CONFIG_DIRS=${C}
    EOF

cat <<EOF | \
  sudo tee /etc/polkit-1/localauthority/50-local.d/xrdp-NetworkManager.pkla
    [xrdp-Netowrkmanager]
    Identity=unix-group:sudo
    Action=org.freedesktop.NetworkManager.network-control
    ResultAny=no
    ResultInactive=yes
    ResultActive=yes
    EOF

cat <<EOF | \
  sudo tee /etc/polkit-1/localauthority/50-local.d/xrdp-packagekit.pkla
    [xrdp-packagekit]
    Identity=unix-group:sudo
    Action=org.freedesktop.packagekit.system-sources-refresh
    ResultAny=no
    ResultInactive=yes
    ResultActive=yes
    EOF

sudo systemctl restart polkit
```

然后前往你的另外一台电脑，输入你要远程连接的电脑的 ip 就可以连接啦！

{% note color:yellow 注意！ 请确保你在防火墙中放行了 RDP 服务所需的端口 **3389**！%}

{% note color:orange Tips: 如果你希望从外网远程到一个内网的机器上，你可以选择进行穿透，在连接远程桌面时使用外网机器ip + 穿透端口即可。%}

------

GL&HF!
