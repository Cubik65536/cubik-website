---
title: 异常处理 (Exception Handling) - 第一部分
topic: java-ds-oop
date: 2024-02-27 17:02:40
categories:
  - Java 数据结构与面向对象编程
tags:
  - Java
  - 异常处理
---

在程序执行的过程中，可能会出现一些无法被预见到的错误 (例如：用户输入错误等) ，这些错误被称为异常。在这种情况下，如果我们不去处理这些问题，程序就会终止 (崩溃) 。但是，我们通常不希望程序在出现异常时直接终止，而是希望能够尝试处理这些问题 (例如：显示错误信息等) ，而 Java 提供了一些机制来实现这一点。

<!-- more -->

## 异常的分类

在 Java 中，我们一般将异常分为两类：**检查异常** (Checked Exception) 和**非检查异常** (Unchecked Exception)。

检查异常，或者编译时异常 (compile-time exception)，例如 `IOException`，通常是由于外部因素 (例如：文件不存在) 引起的。这类异常会在编译时被检查。这并不意味着编译器会在编译时检查需要的文件是否存在，但是编译器会检查这类异常是否已经被处理 (例如被 `try-catch` 代码块捕获，或者在函数签名中被使用 `throws` 关键字声明)，如果没有任何已有的处理措施，JDK 会拒绝编译。

非检查异常，或者运行时异常 (runtime exception)，通常由代码中的逻辑错误或者用户的错误引起 (例如访问越界数组元素)。 JDK 不会对这类问题进行检查，如果对应的异常处理没有被提供，JDK 也不会拒绝编译，但是当这类问题在运行时出现时，程序会直接终止。

{% note color:red "有时 IDE 甚至会提示已经一些可以被检测到的运行时异常！ (而编译器不会去进行检测) 请充分利用 IDE 提供的功能" %}

## 异常处理 - `try-catch` 语句

常见的异常处理关键字：`try`、`catch`、`finally`、`throw`、`throws`。

在 Java 中，常见的运行时异常包括 `ArithmeticException`、`ArrayIndexOutOfBoundsException`、`NullPointerException`、`NumberFormatException` 等。这些异常都是 `RuntimeException` 的子类 (`RuntimeException` 是 `Exception` 的子类，而 `Exception` 是 `Throwable` 的子类) 。

在 Java 中，当一个语句或者函数/方法出现异常时，会抛出 (throw) 一个异常对象 (一个继承自 `Throwable` 的对象) 。这个异常对象会被传递给调用者，直到被捕获 (catch) 或者导致程序终止。为了捕获异常，我们可以使用 `try-catch` 语句。

```java
public class Main {
    public static void main(String[] args) {
        int[] array = new int[12];
        array[16] = 1;
    }
}
```

在上述代码中，我们创建了一个长度为 12 的数组，然后尝试将第 16 个元素赋值为 1。这会导致 `ArrayIndexOutOfBoundsException` 异常。目前这个异常没有被处理，所以程序会直接终止。

```java
public class Main {
    public static void main(String[] args) {
        int[] array = new int[12];
        try {
            array[16] = 1;
        } catch (ArrayIndexOutOfBoundsException arrayIndexOutOfBoundsException) {
            System.out.println(arrayIndexOutOfBoundsException); // 我们可以通过直接打印异常对象来查看异常信息
            System.out.println("Trying to access out-of-bound array element."); // 也可以输出自定义的错误信息
        }
    }
}
```

在上述代码中，`ArrayIndexOutOfBoundsException` 异常被捕获并处理了。我们通过将可能出现的异常代码放在 `try` 代码块中，然后在 `catch` 代码块中处理异常。`catch` 代码块中的参数是异常对象。如果 `try` 代码块中的代码抛出了对应的异常，那么有对应异常对象的 `catch` 代码块中的代码就会被执行。

{% note color:red "我们可以使用 <code>catch (Exception exception)</code> 来捕获所有可能的异常，但是并不建议这么做<br/>有些 IDE 可以通过查看函数文档看到该函数会抛出哪些异常，你可以通过该功能查看到你可能需要处理哪些异常" %}

### 一些其他的情况

```java
public class Main {
    public static void main(String[] args) {
        int[] array = new int[12];
        try {
            array[16] = 1;
            String str = null;
            System.out.println(str.length());
        } catch (ArrayIndexOutOfBoundsException arrayIndexOutOfBoundsException) {
            System.out.println(arrayIndexOutOfBoundsException);
            System.out.println("Trying to access out-of-bound array element.");
        }
    }
}
```

在上述代码中，我们尝试在访问越界数组元素后检查一个空字符串的长度，但这个代码**不会**导致 `NullPointerException` 异常。这是因为在尝试访问空字符串的长度之前，错误的数组访问已经导致了异常，所以程序不会继续执行后面的代码而会直接跳转到 `catch` 代码块。

```java
public class Main {
    public static void main(String[] args) {
        int[] array = new int[12];
        try {
            array[6] = 1;
            String str = null;
            System.out.println(str.length());
        } catch (ArrayIndexOutOfBoundsException arrayIndexOutOfBoundsException) {
            System.out.println(arrayIndexOutOfBoundsException);
            System.out.println("Trying to access out-of-bound array element.");
        }
    }
}
```

在上述代码中，数组访问不再会导致 `ArrayIndexOutOfBoundsException` 异常，所以程序会继续执行后面的代码。这会导致 `NullPointerException` 异常，但是我们没有对这个异常进行处理，所以程序会直接终止。

```java
public class Main {
    public static void main(String[] args) {
        int[] array = new int[12];
        try {
            array[6] = 1;
            String str = null;
            System.out.println(str.length());
        } catch (ArrayIndexOutOfBoundsException arrayIndexOutOfBoundsException) {
            System.out.println(arrayIndexOutOfBoundsException);
            System.out.println("Trying to access out-of-bound array element.");
        } catch (NullPointerException nullPointerException) {
            System.out.println(nullPointerException);
            System.out.println("Trying to access a null string.");
        }
    }
}
```

但是，我们可以通过再添加一个 `catch` 代码块来处理 `NullPointerException` 异常，此时程序不会再崩溃。

```java
public class Main {
    public static void main(String[] args) {
        int[] array = new int[12];
        try {
            array[16] = 1;
            String str = null;
            System.out.println(str.length());
        } catch (ArrayIndexOutOfBoundsException arrayIndexOutOfBoundsException) {
            System.out.println(arrayIndexOutOfBoundsException);
            System.out.println("Trying to access out-of-bound array element.");
        } catch (NullPointerException nullPointerException) {
            System.out.println(nullPointerException);
            System.out.println("Trying to access a null string.");
        }
    }
}
```

在上述代码中，就算两个异常都可以被处理，但是由于在访问数组时已经抛出了异常，所以程序不会继续执行后面的代码，所以本程序只会输出 `ArrayIndexOutOfBoundsException` 异常信息。

如果你希望分开输出两个异常信息，可以将两个可能出现异常的代码分开放在两个 `try` 代码块中。

```java
public class Main {
    public static void main(String[] args) {
        int[] array = new int[12];
        try {
            array[16] = 1;
        } catch (ArrayIndexOutOfBoundsException arrayIndexOutOfBoundsException) {
            System.out.println(arrayIndexOutOfBoundsException);
            System.out.println("Trying to access out-of-bound array element.");
        }
        
        try {
            String str = null;
            System.out.println(str.length());
        } catch (NullPointerException nullPointerException) {
            System.out.println(nullPointerException);
            System.out.println("Trying to access a null string.");
        }
    }
}
```

此时，程序会同时输出 `ArrayIndexOutOfBoundsException` 和 `NullPointerException` 异常信息。

## throw 和 throws

有的时候，我们希望在我们的逻辑代码中主动抛出一个异常 (因为 Java 可能不认为此处有错误，但是我们根据实际程序的需求认为某种情况应该是错误的) 。这时，我们可以使用 `throw` 关键字。

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

在上述代码中，Java 并不认为 `grade` 不可以小于 60，但是，假如本程序的使用场景是检查学生成绩，那么 60 以下的成绩就是错误的。所以我们可以使用 `throw` 关键字来主动抛出一个 `ArithmeticException` 异常。此时如果我们不对这个异常进行处理，程序会仍然会崩溃。

异常抛出语法：

```java
throw new ExceptionType("Exception Message");
```

{% note color:yellow "实际上，在这种情况下，我们可能会创建一个自定义异常 (例如 <code>FailedGradeExpection</code>，我们希望异常类名与异常信息都可以对情况进行简单描述) ，这点之后再讲" %}

异常可以被向调用者传递，例如：

```java
public class Main {
    private static void method1() {
        System.out.println("method1");
        try {
            method2();
        } catch (ArrayIndexOutOfBoundsException arrayIndexOutOfBoundsException) {
            System.out.println(arrayIndexOutOfBoundsException);
            System.out.println("Trying to access out-of-bound array element.");
        }
    }

    private static void method2() {
        System.out.println("method2");
        method3();
    }

    private static void method3() {
        System.out.println("method3");
        exceptionThrower();
    }

    private static void exceptionThrower() {
        int[] array = new int[12];
        array[16] = 1;
    }

    public static void main(String[] args) {
        method1();
    }
}
```

在上述代码中，`main` 函数通过另外数个函数执行了可能会抛出异常的函数 `exceptionThrower`，如果我们不对此异常进行处理，那么程序会直接崩溃。但是，如果我们需要进行处理，处理逻辑可以被放置在任何调用 `exceptionThrower` 的函数中，例如 `method1`。

同时，由于异常可以被向上传递，有的时候异常处理逻辑可能需要由其他使用该函数的人来进行处理，而我们可以使用 `throws` 关键字来声明一个函数可能会抛出的异常。

```java
public class Main {
    private static void method1() {
        System.out.println("method1");
        try {
            method2();
        } catch (ArrayIndexOutOfBoundsException arrayIndexOutOfBoundsException) {
            System.out.println(arrayIndexOutOfBoundsException);
            System.out.println("Trying to access out-of-bound array element.");
        } finally {
            System.out.println("Finally block.");
        }
    }

    private static void method2() {
        System.out.println("method2");
        method3();
    }

    private static void method3() {
        System.out.println("method3");
        exceptionThrower();
    }

    private static void exceptionThrower() throws ArrayIndexOutOfBoundsException {
        int[] array = new int[12];
        array[16] = 1;
    }

    public static void main(String[] args) {
        method1();
    }
}
```

在上述代码中，我们使用 `throws` 关键字来声明 `exceptionThrower` 可能会抛出 `ArrayIndexOutOfBoundsException` 异常。这样，函数文档中就会显示出该函数可能会抛出的异常，这样使用该函数的开发者就可以针对 `ArrayIndexOutOfBoundsException` 异常进行处理。

{% note color:yellow "实际上，在上述情况下 (如果我们认为 <code>method1</code> 是另一位开发者写的函数，而 <code>method2</code> 和 <code>method3</code> 都是我们写的函数)，我们一般会将 <code>throws ArrayIndexOutOfBoundsException</code> 声明到 <code>method2</code> (也就是最上层的函数) 中，否则 <code>method2</code> 的使用者是看不到异常声明的" %}

对于检查异常，我们**必须**在可能会抛出异常的函数以及所有会调用该函数但没有进行任何处理的函数的签名中使用 `throws` 声明该函数可能抛出异常。

同时，上面代码中出现了一个新的关键字：`finally`。`finally` 代码块中的代码会在 `try-catch` 代码块中的代码执行完毕后执行，无论是否出现异常。实际上，就算 `try` 代码块中的代码出现了异常，`catch` 代码块并未处理被抛出的异常，`finally` 代码块中的代码也会被执行，然后异常会继续向上传递（或者导致程序终止）。

## 例题

### 例题 1

要求用户输入两个整数 a 与 b，然后处处 `a/b`。使用异常处理机制，如果 b 为 0，输出一个消息要求用户重新输入两个整数。

除非用户输入两个可以被计算的数值，否则程序会一直要求用户重新输入。

{% folding 解题方法 %}

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("a/b calculator");

        boolean running = true;

        while (running) {
            int a, b;
            try {
                System.out.print("Enter number 'a': ");
                a = scanner.nextInt();
                System.out.print("Enter number 'b': ");
                b = scanner.nextInt();

                int result = a / b;

                System.out.printf("%d / %d = %d", a, b, result);

                running = false;
            } catch (ArithmeticException arithmeticException) {
                System.out.println(arithmeticException);
                System.out.println("Please try with two other numbers.");
            }
        }
    }
}
```

{% endfolding %}

### 例题 2

输入 10 个内容为整数的字符串，将它们存储在一个数组中。将每个字符串转换为整数并打印，如果字符串不能转换为整数，打印一个错误信息然后继续输出剩下的字符串 (中的整数) 。

{% folding 解题方法 %}

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        String[] strings = new String[10];

        System.out.println("Enter 10 strings:");

        for (int i = 0; i < strings.length; i++) {
            System.out.printf("Enter string #%d: ", i + 1);
            strings[i] = scanner.nextLine();
        }

        for (String string : strings) {
            try {
                System.out.printf("Number: %d\n", Integer.parseInt(string));
            } catch (NumberFormatException numberFormatException) {
                System.out.printf("Cannot convert %s to number, continuing...\n", string);
            }
        }
    }
}
```

{% endfolding %}
