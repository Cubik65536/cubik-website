---
title: 创建使用 Windows 11 的（移除硬件限制的） Winddows To Go 系统
date: 2023-10-25 09:28:40
cover: https://www.microsoft.com/en-us/microsoft-365/blog/wp-content/uploads/sites/2/2021/06/WIN_CML_Start_Dark_16x9_en-US.png
categories:
  - 解决方案
  - 折腾
tags:
  - Windows
  - Windows To Go
  - Windows 11
  - 移动硬盘
  - 随身系统
---

前段时间收到了一个新的移动 SSD 并打算把一直在用但是系统很久没更新的 Windows To Go 替换掉，但是要在 Windows To Go 上使用 Windows 11 的话还需要不少特殊操作（尤其是需要用在我的 MacBook Pro 上），所以这里记录一下我是如何创建一个 Windows 11 系统的 Windows To Go 随身系统的

<!-- more -->

<br/>

{% note color:orange "注意" 本文适用于所有需要为 Windows To Go 移除 Windows 11 硬件限制的情况，部分步骤只打算制作使用其他 Windows 版本的 Windows To Go 也可参考 %}

## 准备工作

要制作 Windows To Go 系统，你需要准备以下东西：

- 一个移动硬盘（最好是 SSD，机械硬盘和 U 盘虽理论可行但并不推荐）- 视情况决定容量，我这里因为有游戏需求所以准备了 1TB 的 SSD
- 一个 Windows 机器（不确定是否可以使用虚拟机）- 用于制作 Windows To Go 盘，需要较新的 Windows 版本（10/11）
- [Rufus 软件](https://rufus.ie/zh/) - 用于制作 Windows To Go 盘，注意选择正确的 CPU 架构（x86/x64/ARM），可以酌情选择使用便携版
- Windows 镜像 - 一个你想要安装的 Windows 版本的 ISO 镜像文件，请注意甄别来源，不要使用来历不明的镜像文件。推荐使用 [Microsoft Software Download Listing](https://massgrave.dev/msdl/) 下载，并使用 [files.rg-adguard.net](https://files.rg-adguard.net/version/f0bd8307-d897-ef77-dbd6-216fefbe94c5) 验证文件哈希值以确保文件完整且未被恶意篡改

## 使用 Rufus 制作 Windows To Go

{% note color:orange "注意" 本文使用的 Rufus 软件显示语言为英语，可以将软件语言切换成英语以完全跟随本文指南，也可以参考本文中选项的含义使用其他语言的 Rufus %}

1. 将 ISO 镜像文件下载到 Windows 机器上并验证文件哈希值
2. 下载 Rufus 软件并安装（便携版无需安装）
3. 将移动硬盘连接到 Windows 机器上，确保移动硬盘已经被识别并且没有重要数据（制作过程会格式化移动硬盘）
4. 选择移动硬盘作为 Rufus 的目标设备

    - 点击 `Device` 选项下的选择列表，选择你的移动硬盘（确保名称，盘符和容量都正确）
    - 注意，Rufus 默认不会显示外置 USB 移动硬盘，如果你的移动硬盘没有显示，请点击 `Show advanced drive properties` 并勾选 `List USB Hard Drives` 选项

    ![Rufus-Select-Drive](https://img.cubik65536.top/Rufus-Select-Drive.PNG)

5. 打开 Rufus 软件，选择对应的 ISO 文件，并配置基础的可启动硬盘选项

    - 点击 `Boot selection` 选项下的选择列表，选择你下载好的 ISO 文件（在使用之前非常建议确保文件哈希值正确）
    - `Image option` 下的选择列表应选择 `Windows To Go` 选项
    - 判断你需要运行 Windows To Go 的机器的 BIOS 启动模式
        - 如果可以的话，在你需要运行 Windows To Go 的机器中使用 Windows 的 PowerShell 命令行运行以下命令：

            ``` powershell
            fltmc >$null;if($LASTEXITCODE -ne 0){echo "Not running as admin!"}else{$BootMode = If(bcdedit | Select-String "path.*efi"){"UEFI"}else{"legacy"};echo "Computer is running in $BootMode boot mode."}
            ```

        - 上述命令应输出 `UEFI` 或者 `Legacy`。`UEFI` 是一个较新的启动模式并支持 `GPT` 分区表，`Legacy` 是一个较旧的启动模式且仅支持 `MBR` 分区表
        - 如果你无法使用本命令或类似命令确认启动模式，一般来说，如果你的机器是较新的机器，那么它应该支持 `UEFI` 启动模式
    - 如果你的机器使用 `UEFI` 启动模式，`Partition scheme` 下的选择列表应选择 `GPT` 选项，并将 `Target system` 选项设置为 `UEFI (non CSM)`
    - 如果你的机器使用 `Legacy` 启动模式，`Partition scheme` 下的选择列表应选择 `MBR` 选项，并将 `Target system` 选项设置为 `BIOS (or UEFI-CSM)`
    - 不建议将 `UEFI` 与 `MBR` 或 `Legacy` 与 `GPT` 混合使用，这样做可能会导致无法启动
    - 点击 `Next` 按钮继续

    ![Rufus-Select-ISO](https://img.cubik65536.top/Rufus-Select-ISO.PNG)

6. 选择 Windows 版本

    - 在弹出的窗口中，选择你想要安装的 Windows 版本（不建议使用结尾带 `N` 的版本，推荐按需和你下载的 ISO 提供的版本选择 `Home`, `Pro` 或 `Pro for Workstations`）
    - 点击 `OK` 确认

    ![Rufus-Select-OS_Version](https://img.cubik65536.top/Rufus-Select-OS_Version.PNG)

7. 配置 Windows To Go 制作选项

    - 由于你在制作 Windows To Go 系统，Rufus 会自动移除 Windows 11 的硬件限制
    - 在弹出的窗口中，按需更改部分选项：
        - 如果你不希望让你的 Windows To Go 系统访问 Windows 机器上的硬盘（隐私以及安全考虑），请勾选 `Prevent Windows To Go from accessing internal hard disks` 选项
        - 如果你不希望在 Windows To Go 系统的账号中使用 Microsoft 账号登录，可以勾选 `Remove requirement for an online Microsoft account` 选项
        - 如果你希望 Rufus 提前为你设置好 Windows 账号，勾选 `Create a local account with username` 并在输入框中输入你想要的用户名
        - 如果你希望直接禁用 Windows 数据收集，勾选 `Disable data collection (Skip privacy questions)` 选项
    - 点击 `OK` 确认

    ![Rufus-WTG_Options](https://img.cubik65536.top/Rufus-WTG_Options.PNG)

8. 等待 Rufus 制作可引导移动硬盘
9. 将移动硬盘从 Windows 机器上拔出并连接到你想要运行 Windows To Go 的机器上
10. 进入 BIOS 或使用启动菜单选择从移动硬盘启动，如果一切正常，你应该可以看到 Windows 初次设置界面，按需设置即可
11. 大功告成，你现在可以使用 Windows To Go 了

## 更改 Windows To Go 硬盘的名称与图标

Rufus 会在其创建的 Windows To Go 硬盘中创建 `autorun.inf` 文件和 `autorun.ico` 图标文件。这些文件会限制你更改硬盘的名称和图标，如果你想要更改硬盘的名称和图标...

### 更改硬盘名称

打开 `autorun.inf` 文件，找到 `label` 字段，将其值更改为你想要的硬盘名称，保存并退出

### 更改硬盘图标

1. 将你想要的图标存储到硬盘的根目录下
2. 打开 `autorun.inf` 文件，找到 `icon` 字段，将其值更改为你想要的图标文件的名称，保存并退出

## 总结

本文简单介绍了如何使用 Rufus 制作 Windows To Go 系统，以及如何更改 Windows To Go 硬盘的名称和图标，希望能够帮助到你

{% quot GL & HF! %}
