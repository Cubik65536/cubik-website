---
title: 异常处理 (Exception Handling) - 第二部分
topic: java-ds-oop
date: 2024-03-01 20:46:29
categories:
  - Java 数据结构与面向对象编程
tags:
  - Java
  - 异常处理
---

在上一篇文章中，我们简单的讲述了异常处理的基本概念以及 `try-catch`, `throw`, `throws` 等关键机制。在本篇文章中，我们将继续讨论异常处理的两个高级特性：自定义异常类以及 `catch` 多个异常。

<!-- more -->

## 自定义异常类

正如我们在第一部分中提到的，有些时候我们需要在 Java 不认为有异常情况的时候依照程序设计主动抛出异常。虽然我们有的时候是可以直接使用已有的异常类的，但是由于我们通常希望异常类本身的名字就能够为异常提供一定的描述，因此我们通常会自定义异常类。

```java
public class Main {
    private static void checkGrade(int grade) {
        if (grade < 60) {
            throw new ArithmeticException("This grade is below passing grade!");
        }
    }

    public static void main(String[] args) {
        int grade = 50;
        try {
            checkGrade(grade);
        } catch (ArithmeticException arithmeticException) {
            System.out.println(arithmeticException);
        }
    }
}
```

这是上次我们使用过的示例，我们可以看到，在检查成绩的时候，我们直接使用了 `ArithmeticException` 这个异常类。但是数学错误其实并非我们真正想要表达的意思，我们更希望能够使用一个更加贴切的异常类名字。因此，我们可以自定义一个异常类：

```java
class GradeException extends Exception {
    public GradeException(String message) {
        super(message);
    }
}

public class Main {
    private static void checkGrade(int grade) throws GradeException {
        if (grade < 60) {
            throw new GradeException("This grade is below passing grade!");
        }
    }

    public static void main(String[] args) {
        int grade = 50;
        try {
            checkGrade(grade);
        } catch (GradeException gradeException) {
            System.out.println(gradeException);
        }
    }
}
```

这样，`checkGrade` 函数就不再会抛出 `ArithmeticException`，而是抛出我们自定义的 `GradeException`。这样，我们的异常处理就变得更加清晰了。你如果运行这段代码，你会发现输出的异常信息中包含的异常名也变成了 `GradeException`。

对于自定义异常类，其实它们就是继承自 `Exception` 类或者 `RuntimeException` 类的类。一般我们会选择继承 `Exception` 类使得我们的自定义异常成为一个检查异常 (因为我们通常希望会在某处处理我们自定义的异常)。

一个基础的自定义异常长这样：

```java
class CustomException extends Exception {
}
```

抛出这个自定义异常只需要：

```java
throw new CustomException();
```

注意，该函数中没有任何的带参构造函数，所以此处隐性的使用了默认的无参构造函数。这样的错误在被打印出来的时候只会显示异常名字，而不会显示任何的异常信息。它的输出长这样：

```shell
CustomException

Process finished with exit code 0
```

你会发现输出里只有异常类名，任何关于异常的细节都没被提供。如果我们希望在抛出异常的时候能够提供一些额外的信息，我们可以使用 `toString` 方法：

```java
class CustomException extends Exception {
    public String toString() {
        return "错误信息";
    }
}
```

（**不要这么做！！！不要这么做！！！不要这么做！！！**）

虽然任何显示错误信息的时候其实都是在调用 `Exception` 类的 `toString` 方法，而且重写 `toString` 方法可以使代码正常工作，但是这样做是不标准的。你应该在上面的代码中发现了，`ArithmeticException` 之类的异常类可以接受一个 String 类型的参数。没错，异常类有数个带参构造函数，这里我们只讨论最重要的一个，带有参数 `String message` 参数的构造函数。正如参数名所示，这个参数就是用来传递异常信息的。使用这个构造函数在打印异常信息的时候会显示这个参数的值。因此，我们应该使用这个构造函数来传递异常信息：

```java
class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
}
```

然后在抛出异常的时候使用：

```java
throw new CustomException("错误信息");
```

输出就会成为：

```shell
CustomException: 错误信息

Process finished with exit code 0
```

如果你希望为自定义异常类添加一个默认的错误，你可以自定义无参构造函数，然后让它去调用父类 `Exception` 的带参构造函数：

```java
class CustomException extends Exception {
    public CustomException() {
        super("默认错误信息");
    }
}
```

这样就算你在抛出异常的时候没有提供任何的参数，异常信息也会显示为 "默认错误信息"。

这个默认信息的示例代码如下：

```java
class CustomException extends Exception {
    public CustomException() {
        super("默认错误信息");
    }
}

public class Main {
    private static void throwException() throws CustomException {
        throw new CustomException();
    }

    public static void main(String[] args) {
        try {
            throwException();
        } catch (CustomException customException) {
            System.out.println(customException);
        }
    }
}
```

输出为：

```shell
CustomException: 默认错误信息

Process finished with exit code 0
```

## `catch` 多个异常

有的时候我们可能希望一个 `catch` 可以捕获多个异常，然后对这些不同的异常作出相同的处理，这个时候我们只需要在 `catch` 的括号中使用 `|` 分隔不同的异常类名即可：

```java
public class Main {
    public static void main(String[] args) {
        try {
            int[] arr = new int[5];
            int element = arr[5];
            int result = 5 / 0;
        } catch (ArrayIndexOutOfBoundsException | ArithmeticException e) {
            System.out.println(e);
        }
    }
}
```

在上述的代码中，同一个 `catch` 在数组下标越界异常和除零异常发生的时候都会被执行，且对这两个异常的处理都是一样的。这样的写法可以使得代码更加简洁，而且仍然允许我们不去使用 `Exception` 类的父类来捕获所有的异常。
