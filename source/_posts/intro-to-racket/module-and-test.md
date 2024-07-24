---
title: Racket 入门 - 模块与测试
topic: intro-to-racket
date: 2024-07-23 23:02:35
categories:
  - Racket 入门
tags:
  - Racket
  - 函数式编程
  - Functional Programming
references:
  - "[1] P. Ragde, \"1 Basics,\" *Teach Yourself Racket*. Accessed: Jul. 23, 2024.<br/>Available: https://cs.uwaterloo.ca/~plragde/flaneries/TYR/Basics.html"
---

本文我将简单介绍一下 Racket 的模块以及如何进行测试。

<!-- more -->

## 模块

我们在开发程序的时候，通常会讲代码分成多个文件以更好的组织代码，提高代码的可维护性。与几乎所有其他语言一样，Racket 同样支持将代码分成多个文件。但对于熟悉 Java、Python 等语言的读者来说，Racket 的模块系统可能稍微有些不同：因为你不只需要进行“导入”，还需要进行“导出”。

我们暂且想象有一个文件 `utils.rkt`，其中定义了我们程序所需要的一些工具函数。例如：

``` scheme utils.rkt
#lang racket

(define hello "Hello, World!")
(define num1 3)
(define num2 4.2)

(define (cube x) (* x x x))

(define (sum-of-cube x y)
  (+ (cube x)
     (cube y)))
```

而我们希望在我们的主程序文件 `main.rkt` 中使用 `hello`, `num1`, `num2` 三个变量以 `sum-of-cube` 函数。我们首先需要从 `utils.rkt` 中“导出”这些变量和函数。

”导出“，或者说声明本文件提供哪些变量和函数，可以通过在文件的开头使用 `provide` 关键字来实现。在我们的示例中，`utils.rkt` 文件最终应该如下：

``` scheme utils.rkt
#lang racket

(provide hello num1 num2)
(provide sum-of-cube)

(define hello "Hello, World!")
(define num1 3)
(define num2 4.2)

(define (cube x) (* x x x))

(define (sum-of-cube x y)
  (+ (cube x)
     (cube y)))
```

其中的 `(provide hello num1 num2)` 和 `(provide sum-of-cube)` 分别声明了 `hello`, `num1`, `num2` 以及 `sum-of-cube` 在 `utils.rkt` 中是可以被其他文件访问的。当然，你其实可以将 `(provide hello num1 num2)` 和 `(provide sum-of-cube)` 合并成 `(provide hello num1 num2 sum-of-cube)`，或者将其分成四行：

``` scheme utils.rkt
(provide hello)
(provide num1)
(provide num2)
(provide sum-of-cube)
```

都是可以的。

最后，我们在 `main.rkt` 中使用 `require` 关键字来导入 `utils.rkt` 提供的变量和函数即可：

``` scheme main.rkt
#lang racket

(require "helper.rkt")

hello ;; "Hello, World!"
num1 ;; 3
num2 ;; 4.2

(define x (sum-of-cube num1 2))
x ;; 35
```

{% note color:orange 注意 "通常来讲，`provide` 和 `require` 语句应该放在文件的开头，`#lang` 声明之后。" %}

另外，除了使用 `(require "my-helpers.rkt")` 的方式，你还可以通过 `(require net/url)` 的方式来导入网络上的一些其他模块，或者一些安装 Racket 时附带的模块。例如提供测试功能的 `test-engine/racket-tests` 模块。

{% note color:cyan "另外，你可以访问 The Racket Reference 的 [`provide`](http://docs.racket-lang.org/reference/require.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._provide%29%29) 和 [`require`](https://docs.racket-lang.org/reference/require.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._require%29%29) 部分来了解这两个表达式其他的用法。你还可以访问 [Package Management in Racket](https://docs.racket-lang.org/pkg/index.html) 来了解 Racket 的包管理系统，Racket 默认安装的库，如何安装其他库等。我们之后也会再次提到这些内容。" %}

## 测试

有的时候，我们需要对我们的代码进行测试。Racket 提供了一个简单的测试框架，可以帮助我们进行测试。我们可以通过 `test-engine/racket-tests` 模块来使用这个测试框架。首先，我们需要导入这个模块：

``` scheme
#lang racket

(require test-engine/racket-tests)
```

然后，假如我们有一些函数需要测试，或者有一些变量值需要校验。例如：

``` scheme
(define (fact n)
  (cond
    [(= n 0) 1]
    [(= n 1) 1]
    [else (* n (fact (- n 1)))]))

(define x 16)
(define y 12)
```

我们分别需要测试斐波那契数列函数输出是否正确，以及 `x` 和 `y` 的和是否等于 `32`。我们可以通过 `check-expect` 表达式来建立测试用例：

``` scheme
(check-expect (fact 5) 120)
(define (fact n)
  (cond
    [(= n 0) 1]
    [(= n 1) 1]
    [else (* n (fact (- n 1)))]))

(check-expect (+ x y) 32)
(define x 16)
(define y 12)
```

其中，`(check-expect (fact 5) 120)` 会检查 `(fact 5)` 表达式的值是否等于 `120`，而 `(check-expect (+ x y) 32)` 会检查 `(x + y)` 的值是否等于 `32`。

注意，与其它一些语言的测试框架不同，我们可以把测试用例写在其相关的定义前面。这样我们其实可以通过文件中的空行等方式分隔不同的功能，并通过每个部分开头的测试用例来简单的描述这部分的功能。

目前，你的代码文件应该是这样的：

``` scheme
#lang racket

(require test-engine/racket-tests)

(check-expect (fact 5) 120)
(define (fact n)
  (cond
    [(= n 0) 1]
    [(= n 1) 1]
    [else (* n (fact (- n 1)))]))

(check-expect (+ x y) 32)
(define x 16)
(define y 12)
```

如果你运行这段代码，你会发现什么都没有发生。实际上，我们需要在代码的结尾加入 `(test)` 表达式来一次性运行所有的测试用例：

``` scheme
#lang racket

(require test-engine/racket-tests)

(check-expect (fact 5) 120)
(define (fact n)
  (cond
    [(= n 0) 1]
    [(= n 1) 1]
    [else (* n (fact (- n 1)))]))

(check-expect (+ x y) 32)
(define x 16)
(define y 12)

(test)
```

输出应该是这样的：

``` shell
Ran 2 tests.                                                        
1 of the 2 tests failed.                                            
Check failures:                                                     
                     ┌────┐              ┌────┐                     
        Actual value │ 28 │ differs from │ 32 │, the expected value.
                     └────┘              └────┘                     
in test.rkt, line 12, column 0   
```

其中不仅会告诉一共运行了多少个测试用例，还会告诉你有多少个测试用例失败了，以及失败的原因。这就是最简单的 Racket 测试。

> The testing modules also provide [`check-error`](http://docs.racket-lang.org/test-engine/index.html#%28form._%28%28lib._test-engine%2Fracket-tests..rkt%29._check-error%29%29), which checks if its single argument expression produces an error when evaluated; [`check-within`](http://docs.racket-lang.org/test-engine/index.html#%28form._%28%28lib._test-engine%2Fracket-tests..rkt%29._check-within%29%29), used to test whether an inexact computed value is within a certain tolerance of an expected value; and [`check-member-of`](http://docs.racket-lang.org/test-engine/index.html#%28form._%28%28lib._test-engine%2Fracket-tests..rkt%29._check-member-of%29%29), which permits several possible expected values.
>
> 这个测试框架还提供了 [`check-error`](http://docs.racket-lang.org/test-engine/index.html#%28form._%28%28lib._test-engine%2Fracket-tests..rkt%29._check-error%29%29)，用于检查其参数表达式在求值时是否产生错误；[`check-within`](http://docs.racket-lang.org/test-engine/index.html#%28form._%28%28lib._test-engine%2Fracket-tests..rkt%29._check-within%29%29)，用于测试计算出的非精确值是否在预期值的某个容差范围内；以及 [`check-member-of`](http://docs.racket-lang.org/test-engine/index.html#%28form._%28%28lib._test-engine%2Fracket-tests..rkt%29._check-member-of%29%29)，允许多个可能的预期值。
>
> Ragde [1]，Cubik65536 译

## 总结

本文简单讲述了如何通过 `provide` 和 `require` 表达式来在 Racket 中进行模块化开发，以及如何使用 Racket 自带的 `test-engine/racket-tests` 模块来进行简单的功能测试。

随着 *Teach Yourself Racket* 的 Basic 一章的结束，本文就是 Racket 入门系列的最后一篇文章了。虽然我们只是简单的介绍了一些 Racket 中最基础的内容，但是这些内容已经足够读者开始使用 Racket 来进行一些简单的编程了。

在即将开始的《Racket 学习笔记》系列中，我们将继续跟随 *Teach Yourself Racket*，了解 Racket 中的纯函数式编程（Lambda，高阶函数，本地变量），非纯函数式编程（I/O，可变性），以及 Racket 中的迭代和正则表达式。之后我们将跟随 Racket 官方的 [*The Racket Guide*](https://docs.racket-lang.org/guide/index.html) 来了解 *Teach Yourself Racket* 没有提到的内容，例如 Racket 的异常机制，宏，包管理等。

{% quot GL&HF! %}
