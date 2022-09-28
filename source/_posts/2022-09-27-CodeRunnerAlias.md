---
title: 使用终端命令别名快速编译运行代码
date: 2022-09-27 20:49:10
cover:
categories:
  - 解决方案
tags:
  - 终端
  - macOS
  - 代码运行
  - C++
  - Java
  - OI
---

由于最近在尝试一些新的编辑器来进行单个代码文件的开发（例如竞赛开发），而这些编辑器需要使用命令行才可以编译运行代码。但是，每次运行代码都需要重复输入一长串的命令，这样就会很繁琐，因此我就想到了使用终端命令别名来简化这个过程。

<!-- more -->

{% note color:yellow 注意！ 此处的一些功能可能仅适用于 macOS 操作系统。Linux 上的操作应该是类似的，而 Windows 用户可能需要使用 WSL 实现此功能。%}

## 代码运行

我们先来看看常见情况下编译并运行代码的命令

### C++

```bash
g++ -std=c++17 -O2 -o Main.out Main.cpp -Wall && ./main
```

注意，此处的命令使用了以下参数：

* `-O2`：让 `g++` 编译器通过增加编译时间换取更快的运行速度。（参考[本文](https://www.rapidtables.com/code/linux/gcc/gcc-o.html)）
* `-std=c++17`：指定 `g++` 编译器使用 C++17 标准编译代码。假如你希望使用其他标准，可以参考[此处](https://www.cubik65536.top/2022-06-22-ChangeCheckAndExplainCppStandards/#切换-C-编译器标准)的标准列表。
* `-Wall`：检查一些常见错误

### Java

```bash
javac Main.java && java Main
```

## 设置终端命令别名

我们可以通过设置别名来简化上述两个命令为 `runc`（运行 C++ 代码）和 `runj`（运行 Java 代码），具体操作如下：

1. 打开终端，输入 `vim ~/.bashrc`，进入编辑模式
2. 增加以下内容

```bash
# Compile & Run C++

coc() { g++ -std=c++17 -O2 -o "${1%.*}.out" $1 -Wall; }
runc() { coc $1 && ./${1%.*}.out & fg; }

# Compile & Run Java

runj() { javac $1 && java ${1%.*}; }
```

3. 保存退出，输入 `source ~/.bashrc` 使设置生效

{% note color:orange "`& fg` 是做什么用的？" "`& fg` 会将正在运行的任务的控制信息显示到前台<br/>一些错误信息（例如 C++ 的 `segmentation fault`）需要用此方法才可以显示出来。"%}

## 使用

现在我们就可以使用 `runc` 和 `runj` 来编译运行代码了。假如我们有一个名为 `main.cpp` 的 C++ 代码文件，我们可以使用 `runc main.cpp` 来编译然后运行它，而不需要输入那么长的命令。对于 Java 代码，我们可以使用 `runj main.java` 来编译然后运行它。

## 参考

- [C++ With the Command Line - USACO Guide](https://usaco.guide/general/cpp-command?lang=cpp)
