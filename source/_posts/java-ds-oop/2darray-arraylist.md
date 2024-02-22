---
title: 二维数组、类类型数组与 ArrayList
topic: java-ds-oop
date: 2024-02-21 22:20:28
categories:
  - Java 数据结构与面向对象编程
tags:
  - Java
  - 数据结构
  - 面向对象编程
  - 二维数组
  - ArrayList
  - 数组
---

本文将简单介绍 Java 中的二维数组、以类为元素类型的数组以及 ArrayList。

<!-- more -->

## 二维数组

在深入讨论二维数组之前，我们先来复习一下一维数组。在 Java 中，一维数组由相同类型的元素组成，一个一维数组的常用语法如下：

```java
// 创建一个长度为 5 的 int 类型数组
int[] arr = new int[5];
// 获取数组的长度
int len = arr.length;
// 获取数组的第一个元素
int first = arr[0];
// 修改数组的第一个元素
arr[0] = 1;

// 数组的元素类型可以是原始数据类型 (primitive data type) ，也可以是一个类 (例如 String 类)
String[] strArray = new String[5];
// 或者任何一个用户自定义的类
MyClass[] myClassArray = new MyClass[5];

// 注意，与原始数据类型数组不同，以类为元素的数组在创建时并不会自动初始化（不会给元素授予默认值，每个元素都是 null），需要手动初始化
// 该示例也同样展示了一维数组的遍历
for (int i = 0; i < myClassArray.length; i++) {
    myClassArray[i] = new MyClass();
}
```

现在发挥一下想象力，将一维数组想象成一排柜子，每个单元里都只能放同类的一样物品。但是，对一维数组来说，每排柜子都只有一个，二维数组则为这每排柜子增加了列的概念。

对 Java 来说，一个二维数组其实就是每个元素均为一个一维数组的数组。一个二维数组的常用语法如下：

```java
// 创建一个 3x3 的 int 类型二维数组
int[][] arr = new int[3][3];
// 获取数组中的第二个数组中的第一个元素
int num = arr[1][0];
// 修改数组中的第二个数组中的第一个元素
arr[1][0] = 1;
// 获取整体数组的长度
int len = arr.length;
// 获取每个元素数组的长度
int len1 = arr[0].length;

// 同理，二维数组的元素类型也可以是一个类
MyClass[][] myClassArray = new MyClass[3][3];
// 一个二维数组的简单遍历
for (int i = 0; i < myClassArray.length; i++) {
    for (int j = 0; j < myClassArray[i].length; j++) {
        myClassArray[i][j] = new MyClass();
    }
}
```

### 例题

1. 输入一个 4x4 的 int 类型二维数组，寻找其中的最大值并输出其坐标。

{% folding 解题方法 %}
```java
public class Main {
    public static void main(String[] args) {
        // 创建一个 4x4 的 int 类型二维数组
        int[][] arr = new int[][]{
                {182, 1723, 28, 12},
                {18, 238, 229, 128},
                {283, 2891, 38, 1},
                {0, -29, 382, 29}
        };

        // 创建一个变量用于存储最大值，其初始值为 int 类型的最小值，以确保数组中的任意值都会比其大
        int max = Integer.MIN_VALUE;
        // 创建两个变量用于存储最大值的坐标
        int x = 0, y = 0;

        // 遍历二维数组
        for (int i = 0; i < arr.length; i++) {
            for (int j = 0; j < arr[i].length; j++) {
                // 如果当前元素大于 max，则将 max 更新为当前元素，并记录其坐标
                if (arr[i][j] > max) {
                    max = arr[i][j];
                    x = i;
                    y = j;
                }
            }
        }

        // 输出最大值及其坐标
        System.out.println("max: " + max + ", x: " + x + ", y: " + y);
    }
}
```
{% endfolding %}

2. 输入一个 4x4 的 int 类型二维数组，使用冒泡排序 (bubble sort) 对其进行从大到小的排序。

{% folding 解题方法（较为简单） %}
```java
public class Main {
    public static void main(String[] args) {
        // 创建一个 4x4 的 int 类型二维数组
        int[][] arr = new int[][]{
                {182, 1723, 28, 12},
                {18, 238, 229, 128},
                {283, 2891, 38, 1},
                {0, -29, 382, 29}
        };

        // 使用冒泡排序对二维数组进行排序
        // 冒泡排序的算法本质决定了最多只需要 n 次遍历即可完成排序，其中 n 为数组的长度
        for (int c = 0; c < arr.length * arr[0].length; c++) { // 由于二维数组的长度为 4x4，所以这里的 n 为 16，是大数组的长度乘以小数组的长度
            // 冒泡排序核心算法：将每个元素与其后一个元素进行比较，如果前者小于后者，则交换两者的位置（我们需要从大到小排序）
            for (int i = 0; i < arr.length; i++) {
                int j;
                for (j = 0; j < arr[i].length - 1; j++) {
                    // 将每个元素与其后一个元素进行比较，如果前者小于后者，则交换两者的位置
                    // 注意：此处 j 只到每行的倒数第二个元素，因为如果 j 到最后一个元素，那么 j + 1 就会越界
                    if (arr[i][j] < arr[i][j + 1]) {
                        int temp = arr[i][j];
                        arr[i][j] = arr[i][j + 1];
                        arr[i][j + 1] = temp;
                    }
                }
                // 但是，上面只检查到每行的倒数第二个元素肯定是不够的，我们还需要将每行的最后一个元素和下一行的第一个元素进行比较并按需交换
                if (i < 3) { // 此处的 if 确保我们不去检查最后一行的最后一个元素，它后面没有元素了
                    if (arr[i][j] < arr[i + 1][0]) {
                        int temp = arr[i][j];
                        arr[i][j] = arr[i + 1][0];
                        arr[i + 1][0] = temp;
                    }
                }
            }
        }
    }
}
```
{% endfolding %}

{% folding 解题方法（优化版） %}
```java
public class Main {
    public static void main(String[] args) {
        // 创建一个 4x4 的 int 类型二维数组
        int[][] arr = new int[][]{
                {182, 1723, 28, 12},
                {18, 238, 229, 128},
                {283, 2891, 38, 1},
                {0, -29, 382, 29}
        };

        // 使用冒泡排序对二维数组进行排序
        // 冒泡排序的算法本质决定了最多只需要 n 次遍历即可完成排序，其中 n 为数组的长度
        bool swapped = true;
        while (swapped) { // 实际上，在某些情况下，我们不需要遍历 n 次就可以完成排序，所以我们可以通过判断上一次“排序”是否有交换来提前结束排序，如果上一次没有交换，那么说明数组已经排序完成
            swapped = false; // 每次遍历开始前，我们都假设没有交换
            // 其余算法与上面的解题方法相同
            // 冒泡排序核心算法：将每个元素与其后一个元素进行比较，如果前者小于后者，则交换两者的位置（我们需要从大到小排序）
            for (int i = 0; i < arr.length; i++) {
                int j;
                for (j = 0; j < arr[i].length - 1; j++) {
                    // 将每个元素与其后一个元素进行比较，如果前者小于后者，则交换两者的位置
                    // 注意：此处 j 只到每行的倒数第二个元素，因为如果 j 到最后一个元素，那么 j + 1 就会越界
                    if (arr[i][j] < arr[i][j + 1]) {
                        int temp = arr[i][j];
                        arr[i][j] = arr[i][j + 1];
                        arr[i][j + 1] = temp;
                        swapped = true;
                    }
                }
                // 但是，上面只检查到每行的倒数第二个元素肯定是不够的，我们还需要将每行的最后一个元素和下一行的第一个元素进行比较并按需交换
                if (i < 3) { // 此处的 if 确保我们不去检查最后一行的最后一个元素，它后面没有元素了
                    if (arr[i][j] < arr[i + 1][0]) {
                        int temp = arr[i][j];
                        arr[i][j] = arr[i + 1][0];
                        arr[i + 1][0] = temp;
                        swapped = true;
                    }
                }
            }
        }
    }
}
```
{% endfolding %}

## 类数组

正如上面提到过的，数组的元素可以不是 int, double, char 等原始数据类型，也可以是一个类。我们可以使用这个特性来完成一些比较复杂的操作。

### 例题

创建一个类 `Student`，包含两个属性 `name` 和 `score`，分别为学生的姓名和分数。然后，输入一个 `Student` 类型的数组，对其按照分数从大到小进行排序。最后以此输出学生的姓名和分数。

{% folding 解题方法 %}
```java
class Student {
    // 学生类，包含两个属性 name 和 score
    private String name;
    private int score;

    // 构造函数
    public Student(String name, int score) {
        this.name = name;
        this.score = score;
    }

    // 获取学生的成绩
    public int getScore() {
        return score;
    }
}

public class Main {
    public static void main(String[] args) {
        // 创建一个长度为 3 的 Student 类型数组
        Student[] students = new Student[3];

        for (int i = 0; i < students.length; i++) {
            // 初始化学生数组的每个元素
            // 此处我使用了 Student 加元素的索引来命名学生的姓名，以及一个随机数来模拟学生的分数
            // 实际上，这里的初始化过程可以是任何你想要的，可以是用户输入
            students[i] = new Student("Student" + i, (int)(Math.random() * 100));
        }

        // 使用冒泡排序对学生数组按成绩进行排序
        boolean swapped = true;
        while (swapped) {
            swapped = false;
            for (int i = 0; i < students.length - 1; i++) {
                if (students[i].getScore() < students[i + 1].getScore()) {
                    Student temp = students[i];
                    students[i] = students[i + 1];
                    students[i + 1] = temp;
                    swapped = true;
                }
            }
        }

        // 输出学生的姓名和分数
        for (Student student : students) {
            System.out.println(student.name + ": " + student.getScore());
        }
    }
}
```
{% endfolding %}

总而言之，使用类作为数组的元素类型可以让我们在处理一些复杂的数据时更加方便。以上述例题为例，如果我们分开使用两个数组来存储学生的姓名和分数，由于我们只在排序成绩，但同时需要保证姓名和分数的对应关系，相关操作会变的相当麻烦。但是，如果我们使用一个 `Student` 类型的数组，那么我们就可以保证姓名和分数的对应关系，从而更加方便地进行排序。

## ArrayList

ArrayList 是 Java 中的一个类，它仍然是一个 Array (数组)。但是，与数组不同的是，ArrayList 的长度是可以动态变化的。在 Java 中，ArrayList 是一个泛型类（目前不要花过多时间了解泛型类是个啥），我们可以使用它来存储任何类型的数据。

示例代码：

```java
import java.util.ArrayList; // 导入 ArrayList 类，你的 IDE 或许可以帮你把这件事做了。

public class Main {
    public static void main(String[] args) {
        ArrayList<String> strings = new ArrayList<>(); // 创建一个 String 类型的 ArrayList
        strings.add("Hello"); // 使用 add 方法向 ArrayList 中添加元素，你可以直接写入元素的值，该元素会被添加到 ArrayList 的末尾
        strings.add("Java");
        strings.add("Programming");
        strings.add("Language");
        strings.add(1, "World"); // 你也可以使用 add 方法的重载版本，指定元素的索引，该元素会被添加到指定索引的位置
        System.out.println(strings); // [Hello, World, Java, Programming, Language]
        strings.remove(2); // 使用 remove 方法移除指定索引的元素
        System.out.println(strings); // [Hello, World, Programming, Language]
        strings.remove("World"); // 使用 remove 方法移除指定元素
        System.out.println(strings); // [Hello, Programming, Language]

        ArrayList<Integer> integers = new ArrayList<>(); // 创建一个 Integer 类型的 ArrayList
        // 值得注意的是，ArrayList 的元素类型不可以是原始数据类型，所以我们需要使用包装类 (wrapper class) 来代替
        // 你可以将包装类理解为只有一个对应的原始数据类型的类，例如 Integer 对应 int
        integers.add(1); // 你仍然可以使用 add 方法向 ArrayList 中添加元素
        integers.add(2);
        integers.add(3);
        integers.add(4);
        integers.add(5);
        integers.add(1, 6); // 你也可以使用 add 方法的重载版本，指定元素的索引，该元素会被添加到指定索引的位置
        System.out.println(integers); // [1, 6, 2, 3, 4, 5]
        integers.remove(2); // 使用 remove 方法移除指定索引的元素
        System.out.println(integers); // [1, 6, 3, 4, 5]
        integers.remove(Integer.valueOf(6)); // 使用 remove 方法移除指定元素，此处值得注意的是，你不能直接写入 “6”，因为这样会被认为是索引，而不是元素，所以你需要使用包装类的 valueOf 方法
        System.out.println(integers); // [1, 3, 4, 5]
        // 你可以使用 size 方法获取 ArrayList 的长度
        System.out.println(integers.size()); // 4
        // 虽然 ArrayList 的长度是可以动态变化的，但是如果频繁更改 ArrayList 的长度，会对性能产生一定的影响，所以你可以使用 ensureCapacity 方法来提前设置 ArrayList 的容量
        // 你也可以在初始化 ArrayList 时指定其容量而不再需要单独使用 ensureCapacity 方法，例如：ArrayList<Integer> integers = new ArrayList<>(10);
        integers.ensureCapacity(10);
        // 但是 ensureCapacity 方法只是设置了 ArrayList 的容量，size 方法返回的长度仍然是实际元素的个数
        System.out.println(integers.size()); // 4
        integers.add(6);
        integers.add(7);
        // 当你确认 ArrayList 的长度不会再发生变化时，你可以使用 trimToSize 方法来释放多余的空间，此时 ArrayList 的容量会被设置为实际元素的个数
        integers.trimToSize();
        // 同样的，size 方法返回的长度仍然是实际元素的个数
        System.out.println(integers.size()); // 6

        integers.set(0, 10); // 你可以使用 set 方法来修改指定索引的元素，此处将索引为 0 的元素修改为 10
        System.out.println(integers); // [10, 3, 4, 5, 6, 7]
        // 你也可以使用 get 方法来获取指定索引的元素
        System.out.println(integers.get(0)); // 10
    }
}
```

### 将 ArrayList 转换为数组

有时候，我们需要将 ArrayList 转换为数组。我们有两种方法可以做到这一点：

第一种方法是创建一个相同长度的，新的数组，然后遍历 ArrayList，将其中的元素逐个添加到新数组中。示例代码如下：

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> strings = new ArrayList<>();
        strings.add("String1");
        strings.add("String2");
        strings.add("String3");
        strings.add("String4");

        // 创建一个相同长度的新数组
        String[] array = new String[strings.size()];

        // 遍历 ArrayList，将其中的元素逐个添加到新数组中
        for (int i = 0; i < strings.size(); i++) {
            array[i] = strings.get(i);
        }
    }
}
```

或者，我们可以使用 `toArray` 方法来完成这个任务。示例代码如下：

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> strings = new ArrayList<>();
        strings.add("String1");
        strings.add("String2");
        strings.add("String3");
        strings.add("String4");

        // 使用 toArray 方法将 ArrayList 转换为数组
        // 注意，传入给 toArray 方法的参数是一个新的数组，该数组的长度为 ArrayList 的长度
        String[] array = strings.toArray(new String[strings.size()]);
    }
}
```

## 例题

1. 写一个程序，创建一个长度为 10 的 String 类型的 ArrayList，然后向其中添加 10 个字符串。接着创建一个长度为 10 的 Integer 类型的 ArrayList，然后向其中添加 10 个整数。最后，将 String 类型的 ArrayList 中元素索引为偶数的元素删除掉，将 Integer 类型的 ArrayList 中元素索引为奇数的元素删除掉。

{% folding 解题方法 %}
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> strings = new ArrayList<>();
        strings.add("String0");
        strings.add("String1");
        strings.add("String2");
        strings.add("String3");
        strings.add("String4");
        strings.add("String5");
        strings.add("String6");
        strings.add("String7");
        strings.add("String8");
        strings.add("String9");

        ArrayList<Integer> integers = new ArrayList<>();
        integers.add(0);
        integers.add(1);
        integers.add(2);
        integers.add(3);
        integers.add(4);
        integers.add(5);
        integers.add(6);
        integers.add(7);
        integers.add(8);
        integers.add(9);

        // 创建新的 ArrayList 并不难，但是删除元素的时候需要注意，因为每次删除元素后，ArrayList 的长度会发生变化，需要删除的元素的索引也会发生变化，所以我们需要点小技巧
        // 接下来两个循环都使用了一些神奇的算法，如果你不太理解，可以尝试在草稿纸上画出数组，然后一步一步地模拟删除的过程，你就会发现其中的规律
        // 规律：为了删除索引为偶数的元素，我们其实正好只需要删除从 0 开始，每次 +1 的位置的元素，直至数组的长度的一半；对于奇数的情况，我们其实只需要删除从 1 开始，其他同理

        // 删除 String 类型的 ArrayList 中索引为偶数的元素
        int stringsSize = strings.size();
        for (int i = 0; i < stringsSize / 2; i++) {
            strings.remove(i);
        }

        // 删除 Integer 类型的 ArrayList 中索引为奇数的元素
        int integersSize = integers.size();
        for (int i = 1; i < integersSize / 2 + 1; i++) {
            integers.remove(i);
        }
    }
}
```
{% endfolding %}

2. 写一个程序，创建一个长度为 10 的 String 类型的 ArrayList，然后向其中添加 10 个字符串。最后删除掉所有重复的字符串。

{% folding 解题方法 %}
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> strings = new ArrayList<>();
        strings.add("String");
        strings.add("String1");
        strings.add("String");
        strings.add("String3");
        strings.add("String");
        strings.add("String5");
        strings.add("String");
        strings.add("String7");
        strings.add("String");
        strings.add("String9");

        // 实际上，我们只需要将每个元素和它后面的所有元素进行一下比较就可以了
        // 如果后面的元素和当前元素相同，那么我们就可以删除后面的元素
        for (int i = 0; i < strings.size(); i++) {
            for (int j = i + 1; j < strings.size(); j++) {
                if (strings.get(i).equals(strings.get(j))) {
                    strings.remove(j);
                    j--; // 此处要注意，删除元素后，后面的元素会向前移动，所以我们需要将 j 减一，以确保被向前移动的元素也会被检查
                }
            }
        }
    }
}
```
{% endfolding %}
