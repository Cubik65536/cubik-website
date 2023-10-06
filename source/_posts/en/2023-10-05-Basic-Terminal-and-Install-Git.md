---
title: Basic Terminal Usage & How to Install Git
date: 2023-10-05 08:27:42
cover:
categories:
  - Solutions
tags:
  - Terminal
  - git
  - macOS
  - Linux
  - Windows
---

In this article, I will briefly cover some basic terminal usage, *some recommendation on terminal emulators*, and how to install **git** on different platforms.

{% note color:yellow "Note" Pay attention to your operating system. You have to follow the specific instructions for your os. %}

## Recommended Terminal Emulators

Usually, the default terminal emulator is good enough for basic usages, but they don't really provide the best experience (except those coming with Linux distributions). Here are some recommendations:

- [Tabby](https://tabby.sh) (for Windows, macOS & Linux) - FOSS
  One of the best cross-platform terminal emulators, works well with zsh (for macOS/Linux) and powershell (Windows). Not native, so not really recommended to macOS users (while there's terminals emulators like iTerm2 written in native codes).
- [iTerm2](https://iterm2.com) (for macOS) - FOSS
  Personally the terminal emulator to recommend for macOS users. Written in Objective-C and Swift, it's feature complete and supports lot of features.
- [Warp](https://warp.dev) (for macOS (for now)) - Non-FOSS
  Yes, Rust. Yes, AI-Powered. I love the strong command completion feature, and it's really fast. But, a bad experience: it requires to sign in (and the promise of not using t later was removed), not really a fan of having to sign in to use a terminal emulator and execute LOCAL commands.
- [Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701) (for Windows) - FOSS
  NOT the terminal that comes with Windows. But having a good support for PowerShell and WSL (Windows Subsystem for Linux), still a good choice for Windows users.

## Homebrew

Homebrew is a package manager for macOS. It's a good way to install softwares and libraries and keep them up-to-date. To use it, you'll have to install it first. Open your terminal and execute the following command:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## zsh/oh-my-zsh and powershell/oh-my-posh

You might noticed that I mentionned zsh and powershell in the previous section. They are shells, or let's put it simple: the real terminal programm that reads your commands and execute them. They are more powerful than the default shells (bash for macOS/Linux, cmd for Windows), and they are highly customizable. You can install plugins, themes, and customize the prompt. Do you want to have a prompt like this?

![zsh prompt](https://img.cubik65536.top/SCR-20231005-loec.png)

### zsh

zsh is the recommended for macOS and Linux users. It is even the default shell for some recent versions of macOS and some Linux distributions. Execute the command {% copy "echo $SHELL" %} in your terminal to check your current shell. If it shows `/bin/zsh` or a similar path (like `/usr/bin/zsh` or other paths ending with `zsh`), you are using zsh. Otherwise, follow [this guide](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH) to install it.

{% note color:yellow "Note" If you are using macOS, you'll have to install zsh using Homebrew. You can use the package manager that comes with your distribution (apt, pacman, etc.) to install zsh on Linux. %}

If you followed the guide above, restart your terminal, and type {% copy "echo $SHELL" %} again, you should see the correct output, but, yes I know, it doesn't looks as same as the screenshot above.

#### oh-my-zsh and plugins

To get highligting and other features, you'll have to install oh-my-zsh. Follow the [guide here](https://github.com/ohmyzsh/ohmyzsh/wiki#welcome-to-oh-my-zsh).

After that, we need to install:

- Powerlevel10k: Theme for zsh, to install:
  1. [Install the recommended font.](https://github.com/romkatv/powerlevel10k#meslo-nerd-font-patched-for-powerlevel10k) Optional but highly recommended. (You'll also have to install the font, and set the font of your terminal to the font you just installed.)
  2. Install Powerlevel10k itself.
  3. Restart your terminal.
  4. Execute `p10k configure` command if the configuration wizard doesn't start automatically.
  5. Configure the theme as you like.
- Install [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) plugin.
  It will suggest commands based on your history. And you can use {% kbd Tab %} to autocomplete the command.
  To install it, execute the following command:

  ``` bash
  git clone https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions
  source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh
  ```

- Install [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) plugin.
  It will highlight the command you are typing (if the command is valid, string highlights, etc.).
  To install it, execute the following command:

  ``` bash
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
  echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
  source ./zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
  ```

And then you're good to go!

### PowerShell

PowerShell is the recommended shell for Windows users. It's developed by Microsoft, ships with Windows, targeting replacing cmd. If you're using Windows, you can just use it with Tabby or Windows Terminal (you don't have to install it).

#### oh-my-posh

Oh-My-Posh is the oh-my-zsh for PowerShell. And it also provides some fonctionnalities that are provided by plugins in oh-my-zsh. To install it, follow [this guide](https://ohmyposh.dev/docs/installation/windows). You can use winget (the package manager that comes with Windows) to install it.

It is also recommended to follow [this guide](https://ohmyposh.dev/docs/installation/fonts) to install suggested fonts.

And then you're good to go!

## Basic Terminal Usage

If you really want to learn step by step how to use the terminal, you can follow [Terminal Basics Series](https://itsfoss.com/change-directories/). Otherwise, here are some basic commands:

- `cd`: Change directory. You can use `cd <path>` to change directory to `<path>`. You can use `cd ..` to go to the parent (last) directory.
- `ls`: List files and directories in the current directory.
  - `ls -a`: List all files and directories in the current directory, including hidden files and directories.
  - `ls -l`: List files and directories in the current directory, with more details.
  - `ls -la`: List all files and directories in the current directory, including hidden files and directories, with more details. (You can also use `ls -al`.)
- `mkdir`: Make (create) a directory. You can use `mkdir <path>` to create a directory at `<path>`.
- `touch`: Create a file. You can use `touch <path>` to create a file at `<path>`.
- `rm`: Remove a file or directory. You can use `rm <path>` to remove a file or directory at `<path>`.
  - `rm -r`: Remove a directory and all files and directories in it. You can use `rm -r <path>` to remove a directory at `<path>`.
  - `rm -rf`: Remove a directory and all files and directories in it, without asking for confirmation. You can use `rm -rf <path>` to remove a directory at `<path>`. (Don't use it if you don't know what you are doing.)
- `mv`: Move a file or directory. You can use `mv <path> <new path>` to move a file or directory at `<path>` to `<new path>`.
- `cp`: Copy a file or directory. You can use `cp <path> <new path>` to copy a file or directory at `<path>` to `<new path>`.

## Install Git

Git is a version control system. It's used to manage source code and track changes. It's also used to collaborate with others.

Sometimes, git comes with your operating system. You can check if it's installed by executing `git --version` in your terminal. If it shows something similar to `git version 2.39.3 (Apple Git-145)`, it's installed. Otherwise, you'll have to install it.

### macOS/Linux

Install git on macOS and Linux is easy. You can use the package manager that comes with your distribution (apt, pacman, etc.) to install it.

To install git on macOS, make sure you have Homebrew installed, and execute the following command:

```bash
brew install git
```

To install git on Linux, follow the distribution-specific instructions.

### Windows

You can download the installer from [here](https://git-scm.com/download/win) (make sure to download the **Standalone Installer**). Follow the instructions of the installer to install git.

### Verify Installation

After installing git, you can execute `git --version` in your terminal to verify the installation. If it shows something similar to `git version 2.33.0`, it's installed and you're good to go!

We'll talk about configuring git and using git next time. GL&HF!
