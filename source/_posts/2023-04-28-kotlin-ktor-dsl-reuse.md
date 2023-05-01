---
title: Kotlin HTML DSL 代码复用
date: 2023-04-30 21:30:15
cover:
categories:
  - 解决方案
  - 折腾
tags:
  - Kotlin
  - Ktor
  - Ktor HTML DSL
---

我在开发 [RemoteMC-Core](https://github.com/iXORTech/RemoteMC-Core) 的时候，大量的使用了 Ktor 来完成各项工作。其中就包含使用 Ktor 的
HTML DSL 功能来生成 HTML 页面。但是，我发现我会需要在不同的页面中大量的重复使用一些代码（例如导航栏代码），这样会导致代码不易维护等问题，所以我想着能不能重复使用一段生成代码，在经过一番搜索之后，我发现是可以的！来一起看看怎么做吧！

<!-- more -->

> 本文部分内容参考自 [Static Web with Kotlin DSLs](https://medium.com/kotlin-thursdays/static-web-with-kotlin-dsls-9ff53a604bd2)

首先，我们要确定我们要复用的代码是什么。在这里，我要复用的代码是页脚的代码，它长这样：

``` kt
hr {}
a(href = "https://github.com/iXORTech/RemoteMC-Core/issues") {
  +I18N.reportBug()
}
hr {}
a(href = "https://github.com/iXORTech") {
  i { attributes["data-feather"] = "github" }
}
+" | ${I18N.poweredBy} "
a(href = "https://ixor.tech") { +"iXOR Technology" }
+" ${I18N.withLove}"
br {}
+I18N.htmlThemeDesigned0()
a(href = "https://github.com/athul/archie") { +"Archie Theme" }
+I18N.htmlThemeDesigned1()
a(href = "https://github.com/KevinZonda") { +"@KevinZonda" }
+I18N.htmlThemeDesigned2()
```

而且，要注意的是，它永远会被包含在一个 `DIV` 对象中。

这时，我们要做的就是创建一个新文件用于放置复用的代码，然后写上这段代码：

``` kt
fun DIV.htmlPageFooter() = footer {
  hr {}
  a(href = "https://github.com/iXORTech/RemoteMC-Core/issues") {
    +I18N.reportBug()
  }
  hr {}
  a(href = "https://github.com/iXORTech") {
    i { attributes["data-feather"] = "github" }
  }
  +" | ${I18N.poweredBy} "
  a(href = "https://ixor.tech") { +"iXOR Technology" }
  +" ${I18N.withLove}"
  br {}
  +I18N.htmlThemeDesigned0()
  a(href = "https://github.com/athul/archie") { +"Archie Theme" }
  +I18N.htmlThemeDesigned1()
  a(href = "https://github.com/KevinZonda") { +"@KevinZonda" }
  +I18N.htmlThemeDesigned2()
}
```

这基本上就是为 `DIV` 增加了一个扩展函数 `htmlPageFooter`，这样的话，只要我们在一个 `DIV` 块中，就可以直接使用 `htmlPageFooter` 函数来生成页脚了：

``` kt
div {
  htmlPageFooter()
}
```

如果你不想在 `DIV` 对象中调用这个函数，你则需要将扩展函数前的 `DIV` 换成你想要的对象，例如 `BODY`，然后照常调用就可以了。

``` kt
body {
  htmlPageFooter()
}
```

另外，如果我们需要向复用块中添加其他内容，我们可以就像其他函数一样设置参数：

``` kt
fun BODY.pageWrapper(
  title: String,
  content: DIV.() -> Unit
) = div {
  h1 { +title }
  content()
  htmlPageFooter()
}
```

在这个函数里，`title` 参数就是一个普通的字符串，用来填写 `h1` 中的标题内容。而 `content` 实际上是一个 DIV 对象，我们在调用 `pageWrapper` 函数的时候填写的 `content`内容会被包装在一个 DIV 对象中，放置在 `content()` 被调用的位置。而调用的时候，我们并不需要将 `content` 参数填写在括号中，而是直接在函数调用括号的外面再添加一个大括号，然后在大括号中填写 `content` 的内容。就像这样：

``` kt
pageWrapper("Title") {
  p { +"Hello World!" }
}
```

在这种情况下，我们的代码与以下写法是等价的：

``` kt
body {
  h1 { +"Title" }
  div {
    p { +"Hello World!" }
  }
}
```

那么，Kotlin Ktor HTML DSL 的代码复用就介绍到这里了，希望对你有所帮助！GL & HF！
