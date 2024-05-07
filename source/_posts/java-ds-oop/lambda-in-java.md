---
title: Java 中的 Lambda 表达式
topic: java-ds-oop
date: 2024-04-30 15:25:15
categories:
  - Java 数据结构与面向对象编程
tags:
  - Java
  - 数据结构
  - 面向对象编程
  - Lambda
  - 函数式编程
references:
---

在本文中，我们将简单了解 Java 中的 Lambda 表达式。

<!-- more -->

## 什么是 Lambda 表达式？

Lambda 表达式是 Java 8 中引入的一个新特性，它是一种匿名函数，可以用来简化代码，使代码更加简洁和易读。但是，在讲解 Lambda 表达式之前，我们先来了解一下函数式接口（Functional Interface）。

### 函数式接口

简单的复习：一个接口（Interface）是一个抽象类，它定义了一组方法，但没有实现这些方法。一些类可以实现这个接口，从而实现接口中定义的方法。

如果一个 Java 接口**有且仅有一个抽象方法**，那么这个接口就是一个函数式接口。这唯一的一个抽象方法会定义这个接口的全部行为和功能。

例如 `java.lang.Runnable` 和 `java.util.Comparator` 都是函数式接口，因为它们都各自只有一个抽象方法（分别是 `run()` 和 `compare()`）。

一个函数式接口的定义如下：

``` java
import java.lang.FunctionalInterface;

@FunctionalInterface
interface MyFunctionalInterface {
    double getValue();
}
```

在上述代码中，接口 `MyFunctionalInterface` 仅有一个抽象方法 `double getValue()`，因此它是一个函数式接口。`@FunctionalInterface` 注解是可选的，不过这个注解明确的表示了这个接口是一个函数式接口，因此 Java 编译器会在编译器时检查这个接口是否符合函数式接口的定义（是否只有一个抽象方法），增加这个注解可以避免不小心添加多个抽象方法的错误，也增加了代码的可读性。

有趣的是，如果一个接口只有一个方法，那么为这个方法命名就不再非常重要了，所以 Labmda 表达式可以用来简化这个过程，省去不必要的定义。

### Lambda 表达式

Lambda 表达式是一个匿名函数（anonymous function/unnamed function），其并不会直接执行，而是作为一个函数式接口中的抽象方法的实现使用。Lambda 表达式的语法如下：

``` java
(参数列表) -> 方法体
(参数列表) -> { 方法体 }
```

运算符 `->` 同时被叫做 Lambda 操作符（Lambda operator）或箭头操作符（Arrow operator），它将 Lambda 表达式的参数列表和方法体分开。

下面是一个简单的 Lambda 表达式的例子：

考虑以下代码：

``` java
double getValue() {
    return 3.1415926;
}
```

我们可以将这个传统的函数转换为 Lambda 表达式来简化这个方法的定义：

``` java
() -> 3.1415926
```

这个 Lambda 表达式的参数列表为空，方法体是 `3.1415926`。

## 两种 Lambda 表达式的代码体

Lambda 表达式的代码体可以是一个表达式或一个代码块。

``` java
// Lambda 表达式的代码体是一个表达式
() -> 3.1415926;
() -> System.out.println("Hello, Lambda!");
```

上述代码中，两个 Lambda 表达式的代码体都是“表达式”，也就是说，它们都是一个单独的语句。这些语句直接写在 Lambda 表达式的右侧，不需要使用大括号 `{}`。这些表达式可以是一个常量、一个变量、一个方法调用、一个对象的创建等。其返回值或值会被自动返回。

``` java
// Lambda 表达式的代码体是一个代码块
() -> {
    double pi = 3.1415926;
    return pi;
};
```

上述代码中，Lambda 表达式的代码体是一个代码块，代码块中包含了多个语句。在这种情况下，我们需要使用大括号 `{}` 来定义代码块，而且大括号结尾需要增加一个分号 `;`。如果使用代码块，我们需要使用 `return` 语句来返回值。

注意，我们并不需要定义返回值的类型，因为 Java 编译器可以根据上下文推断出返回值的类型，也就是说我们只需要在函数式接口中定义返回值的类型，Lambda 表达式就会自动返回这个类型的值。

由于 Lambda 表达式实际上是一个函数式接口的实现，因此我们需要提前定义这个函数式接口，然后将 Lambda 表达式作为这个接口的实现。

## Lambda 表达式的使用

``` java
import java.lang.FunctionalInterface;

// 定义一个函数式接口
@FunctionalInterface
interface MyFunctionalInterface {
    // 定义一个抽象方法
    double getValue();
}

public class LambdaExample {
    public static void main(String[] args) {
        // 使用 Lambda 表达式实现函数式接口（一个指向函数式接口的引用）
        MyFunctionalInterface myLambda1 = () -> 3.1415926;
        // 引用也可以分开写
        MyFunctionalInterface myLambda2;
        myLambda2 = () -> 2.7182818;
        // 调用 Lambda 表达式定义的方法（执行定义好的方法）
        System.out.println("Value of Pi: " + myLambda1.getValue());
        System.out.println("Value of e: " + myLambda2.getValue());
    }
}
```

输出结果：

``` plaintext
Value of Pi: 3.1415926
Value of e: 2.7182818
```

注意，上述的 `MyFunctionalInterface myLambda1 = () -> 3.1415926;` 并不是在创建一个**新**的 `MyFunctionalInterface` 对象（我们无法创建一个接口的对象），而是创建了一个指向 `MyFunctionalInterface` 的引用，这个引用指向了一个 Lambda 表达式的实现。这个引用可以调用 `MyFunctionalInterface` 中定义的抽象方法。

如果将上述的 Lambda 表达式改为传统的方法实现，代码如下：

``` java
import java.lang.FunctionalInterface;

@FunctionalInterface
interface MyFunctionalInterface {
    double getValue();
}

class ValueOfPi implements MyFunctionalInterface {
    @Override
    public double getValue() {
        return 3.1415926;
    }
}

class ValueOfE implements MyFunctionalInterface {
    @Override
    public double getValue() {
        return 2.7182818;
    }
}

public class LambdaExample {
    public static void main(String[] args) {
        ValueOfPi myLambda1 = new ValueOfPi();
        // 或者使用 MyFunctionalInterface myLambda1 = new ValueOfPi();
        MyFunctionalInterface myLambda2 = new ValueOfE();
        // 或者使用 ValueOfE myLambda2 = new ValueOfE();
        System.out.println("Value of Pi: " + myLambda1.getValue());
        System.out.println("Value of e: " + myLambda2.getValue());
    }
}
```

## 有参数的 Lambda 表达式

Lambda 表达式也可以有参数，参数列表写在 `()` 中，参数之间使用逗号 `,` 分隔。

``` java
(n) -> n % 2 == 0; // 判断 n 是否为偶数，注意，我们并不一定需要写参数的类型，因为函数式接口已经定义了参数的类型
(a, b) -> a + b; // 计算 a 和 b 的和
```

一个较为复杂的例子（返回一个反过来的字符串）：

``` java
import java.lang.FunctionalInterface;

@FunctionalInterface
interface ReverseString {
    String reverse(String str);
}

public class LambdaExample {
    public static void main(String[] args) {
        ReverseString reverseString = (str) -> {
            String result = "";
            for (int i = str.length() - 1; i >= 0; i--) {
                result += str.charAt(i);
            }
            return result;
        };
        String str = "Hello, Lambda!";
        System.out.println(str + " reversed is: " + reverseString.reverse(str));
    }
}
```

输出结果：

``` plaintext
Hello, Lambda! reversed is: !adbmaL ,olleH
```

另一个例子：

``` java
interface MyFunctionalInterface1 {
    void show();
}

interface MyFunctionalInterface2 {
    void show(int n);
}

interface MyFunctionalInterface3 {
    int show(int a, int b);
}

interface MyFunctionalInterface4 {
    String show(int n);
}

public class LambdaExample {
    public static void main(String[] args) {
        MyFunctionalInterface1 myLambda1 = () -> System.out.println("Hello, Lambda!"); // 对函数式接口的实现
        MyFunctionalInterface2 myLambda2 = (n) -> System.out.println("Number: " + n);
        MyFunctionalInterface3 myLambda3 = (a, b) -> a + b;
        MyFunctionalInterface4 myLambda4 = (int n) -> { // 如果你想的话，参数类型可以被明确指定
            if (n == 1 || n == 0) {
                return "Not a prime number";
            }
            for (int i = 2; i < n; i++) {
                if (n % i == 0) {
                    return "Not a prime number";
                }
            }
            return "Prime number";
        };

        // 调用 Lambda 表达式定义的方法，我们仍然需要使用函数式接口里定义过的方法名
        myLambda1.show();
        myLambda2.show(42);
        System.out.println("Sum of 3 and 5: " + myLambda3.show(3, 5));
        System.out.println(myLambda4.show(5));
    }
}
```

输出结果：

``` plaintext
Hello, Lambda!
Number: 42
Sum of 3 and 5: 8
Prime number
```

## 将 Lambda 表达式作为参数传递

``` java
@FunctionalInterface
interface MyFunctionalInterface {
    void calculate(int a, int b);
}

public class LambdaExample {
    private static void operate(int a, int b, MyFunctionalInterface myFunctionalInterface) {
        myFunctionalInterface.calculate(a, b);
    }

    public static void main(String[] args) {
        MyFunctionalInterface addition = (a, b) -> System.out.println(a + b);
        MyFunctionalInterface subtraction = (a, b) -> System.out.println(a - b);

        operate(5, 3, addition);
        operate(5, 3, subtraction);
    }
}
```

输出结果：

``` plaintext
8
2
```

在上述代码中，我们定义了一个 `operate` 方法，这个方法接受两个整数和一个函数式接口作为参数。我们可以将 Lambda 表达式的实现作为参数传递给 `operate` 方法，然后在 `operate` 方法中调用这个函数式接口的方法。

我们其实也可以直接将 Lambda 表达式作为参数传递给方法：

``` java
@FunctionalInterface
interface MyFunctionalInterface {
    void calculate(int a, int b);
}

public class LambdaExample {
    private static void operate(int a, int b, MyFunctionalInterface myFunctionalInterface) {
        myFunctionalInterface.calculate(a, b);
    }

    public static void main(String[] args) {
        operate(5, 3, (a, b) -> System.out.println(a + b));
        operate(5, 3, (a, b) -> System.out.println(a - b));
    }
}
```

这两个代码片段实际上是等价的。

## 例题1

使用函数式接口和 Lambda 表达式，实现一个程序，确认一个数字是否存在与一个数组中。

``` java
import java.util.ArrayList;

public class LambdaExample {
    @FunctionalInterface
    interface MyFunctionalInterface {
        boolean find(ArrayList<Integer> list, int n);
    }

    public static void main(String[] args) {
        MyFunctionalInterface lambda = (list, n) -> {
            int left = 0, right = list.size(), mid;

            while (left <= right) {
                mid = (left + right) / 2;
                if (mid >= list.size()) return false;
                if (list.get(mid) == n) {
                    return true;
                } else if (n < list.get(mid)) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }

            return false;
        };

        ArrayList<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(2);
        list.add(3);
        list.add(3);
        list.add(3);
        list.add(3);
        list.add(5);
        System.out.println(lambda.find(list, 3) ? "Found" : "Not Found");
    }
}
```

{% quot GL & HF! %}
