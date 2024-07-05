---
title: Racket 入门 - 条件表达式、递归与复合数据
topic: intro-to-racket
date: 2024-07-05 22:20:30
categories:
  - Racket 入门
tags:
  - Racket
  - 函数式编程
  - Functional Programming
references:
  - "[1] P. Ragde, \"1 Basics,\" *Teach Yourself Racket*. Accessed: Jul. 5, 2024.<br/>Available: https://cs.uwaterloo.ca/~plragde/flaneries/TYR/Basics.html"
  - "[2] A. Sikora, \"My other car is a cdr,\", *Medium*, Jan. 23, 2018. Accessed: Jul. 5, 2024.<br/>Available: https://medium.com/@aleksandrasays/my-other-car-is-a-cdr-3058e6743c15"
  - "[3] L. Jennings, \"IBM 704 Scientific Computer,\", Oct. 13, 2012. Accessed: Jul. 5, 2024.<br/>Available: https://www.zl2al.com/1531/ibm-704-scientific-computer/"
---

本文将会介绍 Racket 的条件表达式、递归与复合数据。

<!-- more -->

## Racket 的代码风格

在深入介绍 Racket 的条件表达式、递归与复合数据之前，我们先来了解一下 Racket 的代码风格。这里我直接引用 [*Teach Yourself Racket*](https://cs.uwaterloo.ca/~plragde/flaneries/TYR/Basics.html#%28part._.Some_notes_on_style%29) 中对此的介绍。

> The uniform syntax of Racket has computational advantages which we shall soon see. But it takes some adjustment. The beginnings of stylistic convention are already evident in our examples. Racket identifiers are case-sensitive, but capital letters are rarely used. Hyphens (`-`) are preferred over underscores (`_`) in identifiers. Arguments are either placed on the same line or start in the same column. Multiple close parentheses are typically placed on the same line.
>
> Racket 的统一语法具有计算优势，而且我们很快就能看到这种优势。但是，这需要一些调整。在我们的示例中，代码风格已经开始显现出来。Racket 的标识符区分大小写，但很少使用大写字母。标识符中通常使用连字符 (`-`) 而不是下划线 (`_`)。参数要么放在同一行，要么从同一列开始。多个右括号通常放在同一行。
>
> DrRacket will position the cursor at the right level of indentation when the Enter key is pressed. This positioning can be overridden with Space and Delete keys, and restored for a single line using the Tab key. There are menu items to reindent the current text selection and the entire program. If the cursor is before an open parenthesis or after a close parenthesis, the entire sub-expression will be highlighted, which avoids the need to count parentheses. There are many useful keyboard shortcuts for editing which you can explore (and alter) by using the Keybindings entry in the Edit menu.
>
> DrRacket（以及大部分经过语法分析器配置的代码编辑器，译者注）会自动在按下 Enter 键时将光标定位到正确的缩进级别。使用 Space 和 Delete 键可以覆盖此定位，并使用 Tab 键恢复单行的自动缩进。\[DrRacket 中\]有菜单项可以重新缩进当前文本选择和整个程序。如果光标位于开括号之前或闭括号之后，则整个子表达式将被高亮显示，这样就不需要计算括号的数量了。有许多有用的编辑键盘快捷键，您可以通过“编辑”菜单中的“键绑定”选项进行来探索（和更改）可用的快捷键。（非 DrRacket 的编辑器可能在细节上一些差别，或者一些功能可能不存在，译者注）
>
> A semicolon (`;`) signals the start of a comment, which persists for the rest of the line. Nestable multi-line comments start with `#|` and end with `|#`. Racket programs tend to contain many small functions which don’t require in-line comments. A short header describing purpose and arguments can be useful.
>
> 分号 (`;`) 表示注释的开始，注释将持续到行尾。可嵌套的多行注释以 `#|` 开始，以 `|#` 结束。Racket 程序通常包含许多不需要行内注释的小函数。但一个简短的，描述函数的目的和参数的注释可能会很有用。
>
> Ragde [1]，Cubik65536 译

## 条件表达式

在 Racket 中，布尔值分别使用 `#t` 和 `#f` 表示。`#t` 表示 `true`，`#f` 表示 `false`。

如果要比较两个数字是否相等，可以使用 `=` 函数，并将数字作为参数传递给 `=` 函数。例如：

```scheme
(= 2 2) ; #t
(= 2 3) ; #f
```

表达式也可以作为参数传递给函数。例如：

```scheme
(define x 24)
(define y 42)
(= x y) ; #f

(= (sqrt 4) 2) ; #t
```

你甚至可以一次比较多个数字。只要所有数字都相等，调用 `=` 函数的表达式的值即为 `#t`；否则，值为 `#f`。例如：

```scheme
(= 2 2 2) ; #t
(= 2 2 3) ; #f
```

但是，如果你想要比较两个字符串是否相等，就不能使用 `=` 函数了。以下代码会报错：

```scheme
(= "hello" "hello")
#|
=: contract violation
  expected: number?
  given: "hello"
  context...:
   body of "/Users/cubik65536/Software Development/RacketPlayground/conditional.rkt"
|#
```

一个更好的方法是使用 `string=?` 函数。例如：

```scheme
(string=? "hello" "hello") ; #t
(string=? "hello" "world") ; #f
```

两个数的不等关系可以使用 `>`、`<`、`>=` 和 `<=` 函数来比较。例如：

```scheme
(< 2 3) ; #t
(> 2 3) ; #f
(<= 2 3) ; #t
(>= 2 3) ; #f
```

同时，你应该发现了，我们在比较字符串时使用的函数命名方式就是在运算符前加上 `string` 并在后面加上 `?`。所以，你同样可以使用 `string>`、`string<`、`string>=` 和 `string<=` 函数来比较字符串。例如：

```scheme
(string<=? "hawk" "handsaw") ; #f
```

字符串的比较逻辑是按照字典顺序进行的，如果在一本按照字母顺序排列的字典中，第一个字符串出现在第二个字符串之前，则第一个字符串小于第二个字符串。

除此之外，Racket 还有一些特殊的条件表达式函数，例如：

```scheme
(even? 42) ; #t，判断一个数是否为偶数
(exact? (sqrt 4)) ; #t，判断一个数是否可以被精确表示
(exact? (sqrt 5)) ; #f
(boolean? (= 3 5)) ; #t，判断一个值是否为布尔值
(number? (+ 3 5)) ; #t，判断一个值是否为数字
(string? "hello") ; #t，判断一个值是否为字符串
```

另外，Racket 中的 `equal?` 函数可以比较任意类型的值是否相等。例如：

```scheme
(equal? 3 3) ; #t
(equal? "hello" "hello") ; #t
(equal? "hello" 3) ; #f
```

最后，在其他语言中的 `!` 运算符（取反运算符）在 Racket 中是 `not` 函数。例如：

```scheme
(not #t) ; #f
(not #f) ; #t
```

这个函数会将任何 `#t` 值转换为 `#f`，任何 `#f` 值转换为 `#t`。

### if 表达式

在 Racket 中，`if` 表达式包含三个子表达式：一个条件表达式、一个为条件为真时执行的表达式和一个为条件为假时执行的表达式。在 `if` 表达式被求值时，首先求值条件表达式。如果条件表达式的值为 `#t`，则求值并返回第二个表达式的值；否则，求值并返回第三个表达式的值。

例如：

```scheme
(define z -2)
(if (> z (sqrt 5)) "greater" "not greater") ; "not greater"
(if (positive? z) z (- z)) ; 2
```

需要注意的是，`if` 的条件表达式不必是布尔值。例如：

```scheme
(if "some random thing" "yes" "no") ; "yes"
```

在这种情况下，条件表达式永远被视为 `#t`。

另外，值得注意的是：

> A Racket `if` expression differs in several ways from an **if** statement in an imperative language: an `if` expression has a value, both the second and third subexpressions must be present, and `if` expressions are typically not nested, because the more general and more readable `cond` expression can be used instead.
>
> Racket 的 `if` 表达式与命令式语言中的 **if** 语句有几点不同：`if` 表达式有一个值，`if` 表达式的第二个和第三个子表达式都必须存在（意味着，与其他语言不一样，在 Racket 中实现类似于 **else** 功能的表达式不可省略，译者注），而且 `if` 表达式通常不会互相嵌套（意味着 `if` 表达式中通常不会出现另一个 `if` 表达式，译者注），因为更通用且更易读的 `cond` 表达式在这种情况下可以代替 `if` 表达式。
>
> Ragde [1]，Cubik65536 译

### cond 表达式

`cond` 表达式在 Racket 中更有 `if-elseif-else` 语句的效果（虽然它实际上拥有类似于 `switch-case` 的结构和功能）。`cond` 表达式包含一系列的 "问题"-“答案” 表达式对，每个表达式对都被包含在一对方括号 `[]` 中。在执行 `cond` 表达式时，Racket 会逐个检查每个问题表达式，直到找到一个值为 `#t` 的问题表达式。然后，Racket 对对应的答案表达式求值，并将这个值作为 `cond` 表达式的值。最后一个问题表达式可以是 `else`，如果所有其他问题表达式的值都为 `#f`，则执行最后一个问题表达式对应的答案表达式。

例如：

```scheme
(define (sign-or-zero x)
    (cond
        [(> x 0) 1]
        [(< x 0) -1]
        [else    0]))
(sign-or-zero 3) ; 1
(sign-or-zero -3) ; -1
(sign-or-zero 0) ; 0
```

> The use of square brackets here is purely for readability. A matched pair of parentheses in a Racket program can be replaced by square brackets without changing the program, and vice-versa. We will see a few other places where square brackets are used by convention.
>
> 这里使用方括号纯粹是为了可读性。在 Racket 程序中，成对的圆括号可以被方括号替换而程序不会因此被改变，反之亦然。我们以后将看到其他一些使用方括号的代码风格约定。
>
> Ragde [1]，Cubik65536 译

更复杂的条件可以通过使用 `and` 和 `or` 函数来实现。

一个 `and` 表达式会逐个求值每个子表达式，直到找到一个值为 `#f` 的子表达式，然后返回 `#f`；如果所有子表达式的值都为 `#t`，则返回最后一个子表达式的值。例如：

```scheme
(define (f x) (if (positive? x) x #f))
(and (> 3 4) (< 3 4)) ; #f，因为第一个子表达式的值为 #f
(and (f 3) (f 4)) ; 4，因为没有子表达式的值为 #f，而最后一个子表达式 (f 4) 的值为 4
(and (f 3) (f -4)) ; #f，因为第二个子表达式的值为 #f
```

一个 `or` 表达式以类似的方式工作。当 `or` 表达式被求值时，它会逐个求值每个子表达式，直到找到一个值为 `#t` 的子表达式，然后返回这个子表达式的值；如果所有子表达式的值都为 `#f`，则返回最后一个子表达式的值。例如：

```scheme
(or (f -2) (f 3) (f 4)) ; 3，因为第二个子表达式的值为 3
(or (f -2) (f -3) (f -4)) ; #f，因为所有子表达式的值都为 #f
```

### 示例代码

完整的条件表达式示例代码如下：

```scheme conditional.rkt
#lang racket

; Comparing numbers
(= 2 2) ; #t
(= 2 3) ; #f

; Comparing numbers when nested statements are used
(define x 24)
(define y 42)
(= x y) ; #f
(= (sqrt 4) 2) ; #t

; Comparing more than two numbers
(= 2 2 2) ; #t
(= 2 2 3) ; #f

; Comparing strings
; (= "hello" "hello")
#|
=: contract violation
  expected: number?
  given: "hello"
  context...:
   body of "/Users/cubik65536/Software Development/RacketPlayground/conditional.rkt"
|#
(string=? "hello" "hello") ; #t
(string=? "hello" "world") ; #f

; Comparing other relations
(< 2 3) ; #t
(> 2 3) ; #f
(<= 2 3) ; #t
(>= 2 3) ; #f
(string<=? "hawk" "handsaw") ; #f

(even? 42) ; #t，if the number is even
(exact? (sqrt 4)) ; #t, if the number can be represented exactly
(exact? (sqrt 5)) ; #f
(boolean? (= 3 5)) ; #t, if the value is a boolean
(number? (+ 3 5)) ; #t, if the value is a number
(string? "hello") ; #t, if the value is a string

; equal? to compare values of any type
(equal? 3 3) ; #t
(equal? "hello" "hello") ; #t
(equal? "hello" 3) ; #f

; if expression
(define z -2)
(if (> z (sqrt 5)) "greater" "not greater") ; "not greater"
(if (positive? z) z (- z)) ; 2
; Note that the test subexpression need not produce a Boolean value
(if null "yes" "no") ; "yes"

; cond expression
(define (sign-or-zero x)
    (cond
        [(> x 0) 1]
        [(< x 0) -1]
        [else    0]))
(sign-or-zero 3) ; 1
(sign-or-zero -3) ; -1
(sign-or-zero 0) ; 0

; and
(define (f x) (if (positive? x) x #f))
(and (> 3 4) (< 3 4)) ; #f, as the first subexpression is #f
(and (f 3) (f 4)) ; 4, as no subexpression is #f, and the last subexpression has a value of 4
(and (f 3) (f -4)) ; #f, as the second subexpression is #f

; or
(or (f -2) (f 3) (f 4)) ; 3, as the second subexpression has a value of 3
(or (f -2) (f -3) (f -4)) ; #f, as no subexpression has a value other than #f
```

## 递归

在 Racket 中，一切“循环”都需要利用递归来实现。和指令式编程语言一样，递归的仍然是在函数内部调用函数自身，并通过条件检查来终止递归。例如，计算斐波那契数列的第 $n$ 项：

$$
f(n) = \begin{cases}
    0, & \text{if } n = 0 \newline
    1, & \text{if } n = 1 \newline
    f(n-1) + f(n-2), & \text{if } n > 1
\end{cases}
\text{where } n \in \mathbb{N}_0
$$

```scheme
(define (fib n)
  (cond
    [(= n 0) 0]
    [(= n 1) 1]
    [else 
        (+ (fib (- n 1))
           (fib (- n 2)))
    ]))

(fib 5) ; 5
(fib 6) ; 8
```

或者计算一个数的阶乘：

$$
f(n) = n! = n \times (n-1) \times (n-2) \times \cdots \times 1 = n \times (n-1)!
$$

```scheme
(define (fact n)
  (cond
    [(= n 0) 1]
    [(= n 1) 1]
    [else (* n (fact (- n 1)))]))

(fact 5) ; 120
(fact 0) ; 1
```

> The evaluation of `(fact n)` for large $n$ requires space (memory) roughly proportional to $n$, because the computation of `(fact n)` requires work to be done after the recursive application `(fact (- n 1))` (namely the multiplication by $n$). We can avoid this space overhead by using tail recursion (intuitively, ensuring that the recursive application is the "last thing done").
>
> 对于一个比较大的数 $n$，计算 `(fact n)` 需要的空间（内存）大致与 $n$ 成正比，因为计算 `(fact n)` 需要在 `(fact (- n 1))` 被执行完毕之后才可以进行运算（即乘以 $n$）。我们可以通过使用尾递归（直观地说，确保递归应用是“最后一件事”）来避免这种空间开销。
>
> Ragde [1]，Cubik65536 译

```scheme
(define (fact-helper n acc)
  (cond
    [(= n 0) acc]
    [else (fact-helper (- n 1) (* n acc))]))
(define (fact n) (fact-helper n 1))

(fact 10) ; 3628800
(fact 0) ; 1
```

但是，值得注意的是，阶乘计算可以通过尾递归来实现是因为其定义中本身有一个不依赖于递归计算的值。如果一个函数的定义中没有这样的值（例如完全依赖于递归计算的斐波那契数列），那么这个算法则没有办法通过尾递归来实现。

另外，由于 Racket 中并没有类似于 Java 的函数重载的功能，所以在定义尾递归函数时的辅助函数时不能使用相同的函数名，而我们通常会在函数名后加上 `-helper` 来表示这是一个辅助函数。

## 复合数据

一个复合数据由多个数据组成，这些数据可以是不同类型的数据。

在 Racket 中，`cons` 函数可以用来创建一个最简单的复合数据。`cons` 是 *construct* 的缩写，意为使用多个值**构造**一个复合数据。

这个复合数据是包含 0 个、1 个或 2 个元素的结构。`cons` 函数接受两个参数并将其构造成一个复合数据。例如：

```scheme
(cons 3 4) ; '(3 . 4)
(cons #t "true") ; '(#t . "true")
```

你同样可以定义一个全空的结构，只需使用 `empty` 函数即可。例如：

```scheme
(empty) ; '()
```

或者，你可以将 `cons` 结构的其中一个元素设置为 `empty`，这样就可以创建一个只有一个元素的结构。例如：

```scheme
(cons 3 (empty)) ; '(3)
```

注意，正如我们在第一章中所提到的，Racket 中的打印输出内容会根据值而变化，在上面的例子中，我们看到 `empty` 函数返回的是 `'()`（一个空结构），而一个 `cons` 结构中的 `empty` 不会被打印出来。我们稍后还会看到类似的例子。

同样的，我们可以将 `cons` 表达式内的任何一个参数都替换为另一个 `cons` 表达式，这样就可以创建一个包含多个元素的列表。例如：

```scheme
(cons #t (cons "true" empty)) ; '(#t "true")
(cons (cons 1 (cons 2 empty)) (cons 3 (cons 4 empty))) ; '((1 2) 3 4)
(cons (cons 1 (cons 2 3)) (cons 4 (cons 5 empty))) ; '((1 2 . 3) 4 5)
```

要从这个有至多两个元素的结构中中提取元素，可以使用 `car` 和 `cdr` 函数。

这两个函数的命名可以追溯到 IBM 704 计算机，也就是 Lisp 语言最早被实现的计算机上。IBM 704 的内存上有两个部分，分别叫做 *decrement* 和 *address*，而这两个部分被允许直接访问，`car` 和 `cdr` 的名字就来源于此。`car` 是 *Contents of the Address part of the Register* 的缩写，而 `cdr` 是 *Contents of the Decrement part of the Register* 的缩写。这两个函数的作用是分别返回一个复合数据的第一个元素和剩余元素（在一个只有两个元素的复合数据结构中，剩余元素也就是第二个元素）。例如：

```scheme
(car (cons 3 4)) ; 3
(cdr (cons 3 4)) ; 4
```

但是，如果一个符合数据结构是一个列表，那么你还可以使用 `first` 和 `rest` 函数来获取第一个元素和剩余元素。例如：

```scheme
(first (cons #t (cons "true" empty))) ; #t
(rest (cons (cons 1 (cons 2 3)) (cons 4 (cons 5 empty)))) ; '(4 5)
```

但是下方这种情况就不行了：

```scheme
(rest (cons 3 4)) ; 4
```

因为 `(cons 3 4)` 并没有构建一个列表。

你在上述输出中看到的 `'()` 结构被称之为 *quote notation*（引号表示法）。这种表示法是 Racket 用来表示一个列表的一种方式，这个列表的值被写在括号中，括号前面加上一个引号 `'`。也就是说，你可以用以下方法更直观的构建一个列表（而不需要嵌套使用 `cons` 函数）：

```scheme
'(1 2 3) ; '(1 2 3)
```

当然，还有一个更直观的函数 `list` 来做到一样的效果：

```scheme
(list 1 2 3) ; '(1 2 3)
```

一个列表还可以有一些其他的函数用来对其进行操作：

函数 `first` 到 `tenth` 分别可以返回列表的第一个到第十个元素。

```scheme
(define numbers '(1 2 3 4 5 6 7 8 9 10))
(first numbers) ; 1
(second numbers) ; 2
(third numbers) ; 3
(tenth numbers) ; 10
```

函数 `append` 可以将两个或多个列表连接在一起。形成一个新的列表。

```scheme
(append '(1 2 3) '(4 5 6)) ; '(1 2 3 4 5 6)
(append '(1 2 3) '(4 5 6) '(7 8 9)) ; '(1 2 3 4 5 6 7 8 9)
```

函数 `list-ref` 可以返回列表中的第 $n$ 个元素。（注意，与其他编程语言一样，Racket 中的索引从 0 开始，所以第 0 个元素实际上是列表的第一个元素。）

```scheme
(list-ref '(1 2 3 4 5) 2) ; 3
```

函数 `length` 可以返回列表中的元素个数。

```scheme
(length '(1 2 3 4 5)) ; 5
```

函数 `reverse` 可以返回一个列表的逆序列表。

```scheme
(reverse '(1 2 3 4 5)) ; '(5 4 3 2 1)
```

函数 `take` 可以返回一个列表的前 $n$ 个元素。

```scheme
(take '(1 2 3 4 5) 3) ; '(1 2 3)
```

函数 `drop` 可以删除一个列表的前 $n$ 个元素（返回这个列表的剩余部分）。

```scheme
(drop '(1 2 3 4 5) 3) ; '(4 5)
```

同样的，`car` 和 `cdr` 函数也会返回列表的第一个和除了第一个元素之外的剩余部分。

```scheme
(car '(1 2 3 4 5)) ; 1
(cdr '(1 2 3 4 5)) ; '(2 3 4 5)
```

### 引号表示法... 们

由于我实在不想翻译各种表示法，所以这部分我将会直接使用各种表示法原本的英文称呼。

先来一段非常理论的内容：

> The expression `'(1 2)` is actually shorthand for `(quote (1 2))`. When the quote is applied to a single identifier, the result is a symbol, a form of atomic data with a constant-time equality test. Examples of symbols include `'nabokov` and `'lowry`. Symbols are useful when human-readable distinct values are needed, for example as tags. To a first approximation, the result of quoting an expression that starts with an open parenthesis is that all subexpressions until the corresponding close parenthesis are quoted and the results put into a list. Numbers, strings, and values specified with a hash-sign (such as `#t` and `#f`) quote to themselves. This is an oversimplification, but will do for the purposes of learning. (For the full gory details, see [The Reader](http://docs.racket-lang.org/reference/reader.html) in the Racket Reference.)
>
> 表达式 `'(1 2)` 实际上是 `(quote (1 2))` 的简写。当引号应用于单个标识符时，其结果就是一个符号，这是一种具有常数时间相等性测试的原子数据形式。符号的示例包括 `'nabokov` 和 `'lowry`。当需要人类可读的不同值时，符号是很有用的，例如作为标签。第一次近似地说，引用以开括号开头的表达式的结果是，直到相应的闭括号的所有子表达式都被引用，并将结果放入列表中。数字、字符串和使用井号（例如 `#t` 和 `#f`）指定的值引用为它们自己。这是一个过于简化的说法，但对于学习目的来说足够了。（有关完整的细节，请参见 Racket 参考手册中的 [The Reader](http://docs.racket-lang.org/reference/reader.html)。）
>
> Ragde [1]，Cubik65536 译

如果对上面的内容无法理解，目前只需要知道，`'()` 实际上是 `quote` 函数的一种简写，这意味着 `'(1 2 3)` 实际上和 `(quote (1 2 3))` 是等价的。

但是，假如说我们需要一个列表的元素是从另一个表达式中计算出来的，那么我们就不能使用引号表示法了，例如：

```scheme
'(1 2 (length '(1 2 3)) 4 5)
```

的期望结果是 `'(1 2 3 4 5)`，但是实际上这个表达式的结果是 `'(1 2 (length '(1 2 3)) 4 5)`。这是因为在引号表示法中，所有的子表达式都会被引用，而不会被求值。

这时候我们就需要使用 `quasiquote`（使用 <code>\`</code> 简写）和 `unquote`（使用 `,` 简写）来实现这个目的。`quasiquote` 函数和 `quote` 函数类似，但是所有 `quasiquote` 中被使用 `unquote` 包含的表达式都会被求值，然后这个值会被放入 `quasiquote` 构建的列表中，例如：

```scheme
`(1 2 ,(length '(1 2 3)) 4 5) ; '(1 2 3 4 5)
```

### 复合数据中递归的应用

由于列表等复合数据在本质上仍然是 `cons` 结构套 `cons` 结构，也就是说，它们基本上就是 `cons` 的递归调用，所以我们可以很自然的对复合数据进行递归操作。

例如，我们可以使用递归函数来计算一个列表中所有元素的和：

```scheme
(define (sum-list lst)
  (cond
    [(empty? lst) 0]
    [else (+ (first lst) (sum-list (rest lst)))]))
(sum-list '(1 2 3 4 5)) ; 15
```

或者计算每个元素的平方，并给出一个新的列表：

```scheme
(define (square-list lst)
  (cond
    [(empty? lst) empty]
    [else (cons (sqr (first lst)) (square-list (rest lst)))]))
(square-list '(1 2 3 4 5)) ; '(1 4 9 16 25)
```

当然，尾递归也可以被使用（这种情况下实际上更像 `while` 循环的逻辑）：

```scheme
(define (sum-list-helper lst acc)
  (cond
    [(empty? lst) acc]
    [else (sum-list-helper (rest lst) (+ acc (first lst)))]))
(define (sum-list-tail lst) (sum-list-helper lst 0))
(sum-list-tail '(1 2 3 4 5)) ; 15
```

### String

字符串实际上在 Racket 中也是一个复合数据，它是一个字符的列表。

首先，你可以使用 `substring` 来获取一个字符串的子串。`substring` 函数接受三个参数，分别是原字符串、子串的起始位置和子串的结束位置。例如：

```scheme
(substring "weather" 1 4) ; "eat"
```

你还可以使用 `string-append` 函数来连接两个或者多个字符串，并形成一个新的字符串。例如：

```scheme
(string-append "hello" ", " "world") ; "hello, world"
```

你甚至可以将一个字符串转换为一个字符的列表，然后（如果需要的话）对这个列表进行操作并将其转换回字符串。例如：

```scheme
(string->list "hello") ; '(#\h #\e #\l #\l #\o)
; 翻转字符串
(list->string (reverse (string->list "hello"))) ; "olleh"
```

### 树

由于列表中可以出现列表，所以我们实际上可以使用列表来表示树（第一个数据是根节点，后面的数据是子树）。但是，我们可以使用 `struct` 来定义一个有命名的数据结构，来增强树相关代码的可读性。这里是一个简单的二叉树的例子：

```scheme
(struct bt (label left right) #:transparent)
(define tree (bt "root"
                 (bt "left child" "hello" empty)
                 (bt "right child" empty "world")))

(bt? tree) ; #t, "tree" 是一个 bt 结构吗？
(bt-label tree) ; "root"
(bt-left tree) ; (bt "left child" "hello" '())
(bt-label (bt-left tree)) ; "left child"
(bt-left (bt-left tree)) ; "hello"
```

## 总结

本文介绍了 Racket 中的条件表达式（和条件控制结构），递归和复合数据。下一章我们将简单了解一下 Racket 中的模块以及单元测试。

{% quot GL&HF! %}
