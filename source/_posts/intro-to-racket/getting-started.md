---
title: Racket 入门 - 配置与基础
topic: intro-to-racket
date: 2024-07-02 16:20:30
categories:
  - Racket 入门
tags:
  - Racket
  - 函数式编程
  - Functional Programming
references:
  - "[1] \"Racket,\" *Wikipedia*. Accessed: Jul. 2, 2024.<br/>Available: https://zh.wikipedia.org/zh-cn/Racket"
  - "[2] P. Ragde, \"1 Basics,\" *Teach Yourself Racket*. Accessed: Jul. 2, 2024.<br/>Available: https://cs.uwaterloo.ca/~plragde/flaneries/TYR/"
---

本文是 Racket 入门系列的第一篇，主要介绍 Racket 的配置与基础知识。

<!-- more -->

## 简介

Racket 是一门现代、通用、多范型的函数式编程语言，Lisp 与 Scheme 的一种方言。

> 它的设计目之一是为了提供一种用于创造设计与实现其它编程语言的平台，Racket被用于脚本程序设计、通用程序设计、计算机科学教育和学术研究等不同领域。
>
> Wikipedia [1]

本系列文章将基于 [滑铁卢大学计算机科学系 Prabhakar Ragde 教授的 *Teach Yourself Racket*](https://cs.uwaterloo.ca/~plragde/flaneries/TYR/)，Racket 官方的 [*The Racket Guide*](https://docs.racket-lang.org/guide/index.html) 以及我个人的一些理解，旨在为有命令式编程语言（如 Python、Java、C++ 等）基础的读者介绍 Racket 的基础知识，帮助读者快速上手 Racket 编程。

考虑到这个目标，我可能会去使用一些 Java 中的概念来解释一些 Racket 的概念，以帮助读者更好地理解 Racket，所以可能会出现一些解释或用词不太准确的地方。我仍然推荐读者在阅读本系列文章后去查看 *Teach Yourself Racket* 和 *The Racket Guide* 来更好地理解 Racket。

## 安装

可以直接从 [Racket 官网](https://download.racket-lang.org/) 下载安装程序进行安装。

对于 macOS 用户，可以使用 Homebrew 安装：

```bash
brew install --cask racket # 推荐安装这个版本，因为它包含了最新版的 Racket 以及 DrRacket（Racket 的 IDE）
# 或
brew install racket
```

如果你是 Linux 用户，你可能可以使用你的包管理器安装 Racket。

安装完成后，你可以通过在命令行中输入 `racket --version` 来检查 Racket 是否安装成功。如果有类似于 `Welcome to Racket v8.13 [cs].` 的输出，则代表你的 Racket 安装成功了。

或者，你可以尝试启动 DrRacket，如果 DrRacket 能够正常启动，那么你的 Racket 就是正常的。DrRacket 是 Racket 的集成开发环境（IDE），你可以在其中编写、运行 Racket 程序，界面类似于下图：

{% image https://img.cubik65536.top/DrRacket.png "DrRacket" %}

## 编辑器

*Teach Yourself Racket* 实际上推荐使用 DrRacket 作为 Racket 的 IDE。根据我本人的体验，DrRacket 实际上相当优秀，但是仍然缺少一些我认为对初学者很重要的功能，比如函数提示等。而且由于本系列文章针对的是有命令式编程语言基础的读者，而大部分读者可能已经有了自己的编辑器偏好，所以此处我仍然会简单的介绍一下如何在其它编辑器中编写 Racket。

### 安装 Racket Langserver

Racket Langserver 是一个 Racket 的 Language Server Protocol（LSP）实现，它可以为 DrRacket 以外的编辑器提供 Racket 的语法高亮、自动补全等功能。不管你使用什么编辑器，都需要安装 Racket Langserver 以获得 Racket 的语法高亮、函数提示等功能。

你可以通过以下命令安装 Racket Langserver：

```bash
raco pkg install racket-langserver
```

`raco` 是 Racket 的包管理器，在你安装 Racket 时也会一并安装。

### Visual Studio Code

如果你使用 Visual Studio Code，你可以安装 Magic Racket 插件（插件 ID `evzen-wybitul.magic-racket`）来获得 Racket 的语法高亮等功能。

### JetBrains 系列 IDE

如果你使用 JetBrains 系列的 IDE，你可以安装 Racket 插件来获得 Racket 的语法高亮等功能。但相对于 Visual Studio Code，我更不推荐各位使用 JetBrains 系列 IDE 来编写 Racket，因为 JetBrains 系列的 Racket 插件功能相对没那么完善。

### NeoVim

Neovim LSP 客户端支持 Racket Langserver，可以参考 [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md#racket_langserver) 来配置。

### Sublime Text

Sublime Text 也有 Racket 的语法高亮插件，但是似乎已经很久没有更新了，所以我不推荐使用 Sublime Text 来编写 Racket。

## Hello, World!

首先，新建一个 Racket 代码文件（后缀名为 `.rkt`），可以命名为 `hello.rkt`，然后打开文件，输入第一行代码：

``` scheme hello.rkt
#lang racket
```

这行代码会告诉 Racket 解释器，这个文件是一个 Racket 源代码文件。然后在下一行输入：

``` scheme hello.rkt
"Hello, World!"
```

然后保存文件，运行这个文件，你会看到输出 `Hello, World!`。

完整的 `hello.rkt` 文件如下：

``` scheme hello.rkt
#lang racket

"Hello, World!"
```

## Racket 表达式与值

如果你习惯了使用 Java、Python 等语言，你可能会发现很奇怪的一件事情：我们并没有使用任何打印函数来输出 `Hello, World!`，这是因为在 Racket 中，表达式的值会被自动输出到控制台。

> In imperative programming languages, a program typically computes by repeatedly modifying or mutating the values of variables. In contrast, functional programming languages focus more on the application of functions (also called procedures) to already-computed values in order to create new values. Mutation is present in Racket but is de-emphasized; we will discuss it and other side effects much later in this document, to show what can be accomplished in their absence.
>
> 在命令式编程语言中，程序通常通过反复修改或变化变量的值来进行计算。相比之下，函数式编程语言更注重将函数（也称为过程）应用于已计算的值，以创建新值。Racket 中存在可变性，但是它被弱化了；我们将在本文档的后面讨论它和其他副作用，以展示在它们的缺失下可以实现什么。
>
> A Racket program is a sequence of definitions and expressions. When one runs a program, each of the expressions is evaluated in order, and the resulting values are printed in the Interactions window, one to a line. (All Racket values have a printed representation, though, as we will see, in some cases only a limited amount of information is printed.) The Interactions window then repeatedly offers an interactive prompt (>) which lets one enter more expressions and have them evaluated. This read-evaluate-print loop (REPL) is a common and useful feature of interpreters for functional programming languages.
>
> 一个 Racket 程序是由一系列的定义与表达式组成的。当运行一个程序时，每个表达式按顺序被求值，结果值被打印在交互窗口中，每行一个。（所有 Racket 值都有一个打印表示，尽管，正如我们将看到的，有些情况下只有有限的信息被打印。）交互窗口随后会提供一个交互式提示符（>），让你输入更多的表达式并对其求值。这种读取-求值-打印循环（REPL）是函数式编程语言解释器的一个常见且有用的特性。
>
> Ragde [2]，Cubik65536 译

由于我们暂时并不会去使用 Racket 交互式命令行，所以我们暂且先不去深究交互窗口的内容。

在 Racket 中，每一个表达式都会产生一个值作为结果，而这个结果要不会被直接输出到控制台，要不会被用在其他表达式中（例如一个定义中）。这也是为什么我们在 `hello.rkt` 中不使用任何打印函数也可以输出 `Hello, World!` 的原因。

在 Racket 中，最简单的表达式就是一个值，表现为一个直接定义在程序中的常量：

``` scheme
42 ;; 整数
3.14 ;; 浮点数
-392 ;; 负数
32/4 ;; 分数，在输出时会被化简为，这里会输出 8
;; 32+8 ;; 这是不可以的
"Hello, World!" ;; 字符串
```

注意，上述的 `32+8` 是不合法的，因为 Racket 实际上不使用中缀表示法来表示运算，而是使用前缀表示法，即运算符在操作数之前，例如 `(+ 32 8)`。所以 Racket 中的四则运算应该是这样的：

``` scheme
(+ 32 8) ;; 40
(+ 10 (/ 8 2) (* 3 6)) ;; 10 + 8 / 2 + 3 * 6
```

请注意第二行的 `(+ 10 (/ 8 2) (* 3 6))`，此处的括号 `()` 是用来表示一个表达式的范围的，也就是说，这个加法计算的是表达式 `10`、表达式 `/ 8 2` 和表达式 `* 3 6` 的值的和。所以括号在 Racket 的运算中并不起数学或者其他一些语言中决定运算顺序的作用，不要混淆。

第三种表达式即为函数调用，其语法为 `(function-name arg1 arg2 ...)`，其中 `function-name` 为函数名，`arg1`、`arg2` 等为函数的参数。例如找出数个数字中的最大值：

``` scheme
(max 3 15 7 -24) ;; 15
```

建议读者自行尝试这些表达式的使用，以加深对 Racket 表达式的理解。

## 定义

在 Racket 中，我们可以使用 `define` 来定义一个变量或者一个函数。`define` 的语法为 `(define name value)`，其中 `name` 表达式为变量名或者函数签名，`value` 表达式为变量的值或者函数的定义。

定义一个变量：

``` scheme
(define x 42) ;; 定义一个变量 x，值为 42
x ;; 42

(define y (+ 32 8)) ;; 你同样可以在定义中使用表达式来讲一个值赋给一个变量
y ;; 40

(define z (+ x y)) ;; 你也可以在定义中使用已经定义的变量
z ;; 82

;; (define x 3.14) ;; 但是，要注意，你不能重新定义一个已经定义的变量，这会导致错误
```

定义一个函数：

``` scheme
(define (square x) (* x x)) ;; 定义一个函数 square，参数为 x，返回值为 x * x
(square 5) ;; 25

;; 你也可以在定义中使用已经定义的函数
(define (square-sum x y)
  (+ (square x)
     (square y)))
(square-sum 3 4) ;; 25
```

对于习惯了命令式编程的读者，你可以将 `name` 理解为函数名和参数名列表，`value` 理解为函数体。你可以在 `value` 表达式中使用 `name` 中声明的参数名来引用参数。在这时，`name` 不会使用其他位置定义的同名值，而会使用函数调用时传入的参数。

``` scheme definitions.rkt
#lang racket

(define x 3)
 
(define y (+ x 1))
 
(define (sum-of-squares x y)
  (+ (square x)
     (square y)))
 
(define (square x) (* x x)) ;; 方法定义的顺序并不重要，只要一个方法在脚本的某处被定义了就可以在脚本的任何地方被调用

(sum-of-squares x y) ;; 25
```

## 结语

本文主要介绍了 Racket 的配置与基础知识，包括 Racket 的安装、编辑器的选择、Hello, World! 程序的编写、Racket 表达式与值、定义等内容。下一章我们将会涉及到 Racket 的条件表达式、递归、复合数据等内容。

{% quot GL&HF! %}
