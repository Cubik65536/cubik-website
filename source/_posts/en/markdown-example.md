---
title: "Markdown Example"
date: 0000-00-00 00:00:00
categories: markdown
mathjax: true
---

The source file of this post is a Markdown file to test the compatibility of some Markdown features.

<!-- more -->

![](https://pandao.github.io/editor.md/images/logos/editormd-logo-180x180.png)

**Table of Contents**

[TOCM]

[TOC]

# Heading 1

## Heading 2

### Heading 3

#### Heading 4

##### Heading 5

###### Heading 6

#### Heading (underline)

This is an H1
=============

This is an H2
-------------

### Text effects and horizontal lines

----

~~delete~~ <s>delete</s>
*Italic*      _Italic_
**Bold**      __Bold__
***Italic Bold*** ___Italic Bold___

Superscript：X<sub>2</sub>，Subscript：O<sup>2</sup>

### Abbreviations (HTML abbr tag)

> The abbrivation of a long word or phrase, which is enabled by default when recognizing HTML tags.

The <abbr title="Hyper Text Markup Language">HTML</abbr> specification is maintained by the <abbr title="World Wide Web Consortium">W3C</abbr>.

### Blockquotes

> Blockquotes

Blockquotes

> Quote: If you want to insert a blank line break (that is, the `<br/>` tag), add two or more spaces at the insertion point and then press Enter. [Normal Link](http://localhost/).

### Links

[Normal Link](http://localhost/)

[Normal Link with Title](http://localhost/ "普通链接带标题")

Direct Link: <https://github.com>

[Anchor Link][anchor-id]

[anchor-id]: http://www.this-anchor-link.com/

[mailto:test.test@gmail.com](mailto:test.test@gmail.com)

GFM a-tail link @pandao  AUTO LINK FOR EMAIL test.test@gmail.com  www@vip.qq.com

> @pandao

### Codes

#### Inline code

Execute command: `npm install marked`

#### Indented code

Indent with for spaces, also for implement Preformatted Text function similar to `<pre>`.

    <?php
        echo "Hello world!";
    ?>

Preformatted Text

    | First Header  | Second Header |
    | ------------- | ------------- |
    | Content Cell  | Content Cell  |
    | Content Cell  | Content Cell  |

#### Multi-line codeblock

``` cpp
#include <bits/stdc++.h>

using namespace std;
const int MAX_N = 100;
const int MAX_SUM = 1e5;
bool dp[MAX_N + 1][MAX_SUM + 1];
int main() {
    int n;
    cin >> n;
    vector<int> coins_values(n);
    for (int i = 0; i < n; i++) {
        cin >> coins_values[i];
    }
    dp[0][0] = true;
    for (int i = 1; i <= n; i++) {
        for (int current_sum = 0; current_sum <= MAX_SUM; current_sum++) {
            dp[i][current_sum] = dp[i - 1][current_sum];
            int prev_sum = current_sum - coins_values[i - 1];
            if (prev_sum >= 0 && dp[i - 1][prev_sum]) {
                dp[i][current_sum] = true;
            }
        }
    }
    vector<int> possible;
    for (int sum = 1; sum <= MAX_SUM; sum++) {
        if (dp[n][sum]) {
            possible.push_back(sum);
        }
    }
    cout << (int)(possible.size()) << endl;
    for (int sum : possible) {
        cout << sum << " ";
    }
    cout << endl;
}
```

``` java Kattio Class in Java
import java.io.*;
import java.util.*;

class Kattio extends PrintWriter {
    private BufferedReader r;
    private StringTokenizer st;

    // standard input
    public Kattio() {
        this(System.in, System.out);
    }

    public Kattio(InputStream i, OutputStream o) {
        super(o);
        r = new BufferedReader(new InputStreamReader(i));
    }

    // USACO-style file input
    public Kattio(String problemName) throws IOException {
        super(new FileWriter(problemName + ".out"));
        r = new BufferedReader(new FileReader(problemName + ".in"));
    }

    // read next line
    public String readLine() {
        try {
            return r.readLine();
        } catch (IOException e) {
        }
        return null;
    }

    // returns null if no more input
    public String next() {
        try {
            while (st == null || !st.hasMoreTokens())
                st = new StringTokenizer(r.readLine());
            return st.nextToken();
        } catch (Exception e) {
        }
        return null;
    }

    public int nextInt() {
        return Integer.parseInt(next());
    }

    public double nextDouble() {
        return Double.parseDouble(next());
    }

    public long nextLong() {
        return Long.parseLong(next());
    }
}
```

### Images

Image:

![Image](https://pandao.github.io/editor.md/examples/images/4.jpg)

> Follow your heart.

![Follow your heart](https://pandao.github.io/editor.md/examples/images/8.jpg)

> 图为：厦门白城沙滩

Image + Link:

[![李健首张专辑《似水流年》封面](https://pandao.github.io/editor.md/examples/images/7.jpg)](https://pandao.github.io/editor.md/images/7.jpg)

> 图为：李健首张专辑《似水流年》封面

----

### 列表 Lists

#### 无序列表（减号）Unordered Lists (-)

- 列表一
- 列表二
- 列表三

#### 无序列表（星号）Unordered Lists (*)

- 列表一
- 列表二
- 列表三

#### 无序列表（加号和嵌套）Unordered Lists (+)

- 列表一
- 列表二
  - 列表二 -1
  - 列表二 -2
  - 列表二 -3
- 列表三
  - 列表一
  - 列表二
  - 列表三

#### 有序列表 Ordered Lists (-)

1. 第一行
2. 第二行
3. 第三行

#### GFM task list

- [x] GFM task list 1
- [x] GFM task list 2
- [ ] GFM task list 3
  - [ ] GFM task list 3-1
  - [ ] GFM task list 3-2
  - [ ] GFM task list 3-3
- [ ] GFM task list 4
  - [ ] GFM task list 4-1
  - [ ] GFM task list 4-2

----

### 绘制表格 Tables

| 项目        | 价格   |  数量  |
| --------   | -----:  | :----:  |
| 计算机      | $1600   |   5     |
| 手机        |   $12   |   12   |
| 管线        |    $1    |  234  |

First Header  | Second Header
------------- | -------------
Content Cell  | Content Cell
Content Cell  | Content Cell

| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |

| Function name | Description                    |
| ------------- | ------------------------------ |
| `help()`      | Display the help window.       |
| `destroy()`   | **Destroy your computer!**     |

| Left-Aligned  | Center Aligned  | Right Aligned |
| :------------ |:---------------:| -----:|
| col 3 is      | some wordy text | $1600 |
| col 2 is      | centered        |   $12 |
| zebra stripes | are neat        |    $1 |

| Item      | Value |
| --------- | -----:|
| Computer  | $1600 |
| Phone     |   $12 |
| Pipe      |    $1 |

----

#### 特殊符号 HTML Entities Codes

&copy; &  &uml; &trade; &iexcl; &pound;
&amp; &lt; &gt; &yen; &euro; &reg; &plusmn; &para; &sect; &brvbar; &macr; &laquo; &middot;

X&sup2; Y&sup3; &frac34; &frac14;  &times;  &divide;   &raquo;

18&ordm;C  &quot;  &apos;

### Emoji 表情 :smiley:

> Blockquotes :star:

#### GFM task lists & Emoji & fontAwesome icon emoji & editormd logo emoji :editormd-logo-5x

- [x] :smiley: @mentions, :smiley: #refs, [links](), **formatting**, and <del>tags</del> supported :editormd-logo:;
- [x] list syntax required (any unordered or ordered list supported) :editormd-logo-3x:;
- [x] [ ] :smiley: this is a complete item :smiley:;
- [ ] []this is an incomplete item [test link](#) :fa-star: @pandao;
- [ ] [ ]this is an incomplete item :fa-star: :fa-gear:;
  - [ ] :smiley: this is an incomplete item [test link](#) :fa-star: :fa-gear:;
  - [ ] :smiley: this is  :fa-star: :fa-gear: an incomplete item [test link](#);

#### 反斜杠 Escape

\*literal asterisks\*

### 科学公式 TeX (KaTeX)

$E=mc^2$

行内的公式 $E=mc^2$

行内的 $E=mc^2$ 公式。

$$x > y$$

$$(\sqrt{3x-1}+(1+x)^2)$$

$$\sin(\alpha)^{\theta}=\sum_{i=0}^{n}(x^i + \cos(f))$$

多行公式：

$$
\displaystyle
    \frac{1}{
        \Bigl(\sqrt{\phi \sqrt{5}}-\phi\Bigr) e^{
        \frac25 \pi}} = 1+\frac{e^{-2\pi}} {1+\frac{e^{-4\pi}} {
        1+\frac{e^{-6\pi}}
        {1+\frac{e^{-8\pi}}
         {1+\cdots} }
        } 
    }
$$

### 绘制流程图 Flowchart

``` flow
st=>start: 用户登陆
op=>operation: 登陆操作
cond=>condition: 登陆成功 Yes or No?
e=>end: 进入后台

st->op->cond
cond(yes)->e
cond(no)->op
```

### 绘制序列图 Sequence Diagram

``` sequence
Andrew->China: Says Hello 
Note right of China: China thinks\nabout it 
China-->Andrew: How are you? 
Andrew->>China: I am good thanks!
```

### End
