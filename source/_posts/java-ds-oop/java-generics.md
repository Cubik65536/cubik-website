---
title: Java 中的泛型
topic: java-ds-oop
date: 2024-04-02 15:06:43
categories:
  - Java 数据结构与面向对象编程
tags:
  - Java
  - 数据结构
  - 面向对象编程
  - 泛型
  - Generics
references:
---

在前两章中，我们提到了 Java 中的栈、队列和链表的实现，而一个问题随之而来 - 在我们的实现中，每个数据结构都只能存储一种类型的数据。但是，如果我们有多种数据都需要存储在同一个数据结构中，该怎么办呢？为每个类型都实现一遍每个数据结构吗？这显然是不现实的。这就是 Java 中泛型的作用。

泛型（Generics）是 Java 中的一个重要特性，自 Java 5.0 版本开始引入。它允许我们在定义类、接口和方法时使用类型参数。这样，我们就可以在使用这些类、接口和方法时指定具体的类型，从而实现代码的复用。

一个最简单的泛型类的定义如下：

```java
class Box<T> {
    private T data;

    public Box(T data) {
        this.data = data;
    }

    public void setData(T data) {
        this.data = data;
    }

    public T getData() {
        return data;
    }
}
```

在这个例子中，`Box` 是一个泛型类，`T` 是类型参数。在创建 `Box` 对象时，我们可以指定 `T` 的具体类型，而这个被指定的类型将会成为 `Box` 类中的使用 `T` 作为类型的地方的具体类型。例如：

```java
Box<Integer> intBox = new Box<>(42);
intBox.setData(43);
System.out.println(intBox.getData()); // 43

Box<String> strBox = new Box<>("Hello, World!");
strBox.setData("Hello, Java!");
System.out.println(strBox.getData()); // Hello, Java!
```

在上述代码中，`intBox` 中 `data` 的类型是 `Integer`，而 `strBox` 中 `data` 的类型是 `String`。

{% note color:orange 注意！ 泛型类中的类型参数不能是基本数据类型，只能是“类”类型。也就是说，我们不能创建一个 `Box<int>` 对象，而应该使用 `Box<Integer>`。但是，我们可以随便使用其他类作为类型参数，例如 `Box<Toy>`。 %}

在 Java 中，泛型在集合（Collection）类中得到了广泛的应用。例如，`ArrayList`、`LinkedList`、`HashMap` 等都是泛型类。在使用这些类时，我们可以指定集合中元素的类型，从而避免了类型转换的麻烦。同时，泛型的使用提高了代码的复用性以及代码的安全性（由于类型已经在编写时确定，编译器可以在编译时检查类型是否匹配）。

## 泛型的类型

Java 中，泛型可以在多种地方使用，包括类、接口、方法等，包括：

- **泛型类**
- **泛型接口**
- **泛型方法**
- **泛型通配符**

### 泛型类

泛型类是一个拥有一个或者多个类型参数的类，这个类型参数可以在类中的成员变量、方法参数、方法返回值等地方使用。

要定义泛型类，需要在类名后面加上尖括号（`<>`），然后在尖括号中定义类型参数（通常使用大写字母例如 `T`、`K`、`V` 等）。

上述的 `Box` 类就是一个泛型类的例子。我们可以定义一个泛型类，然后在创建对象时指定具体的类型。例如：

```java
class Box<T> {
    private T data; // 类型参数被作为成员变量的类型使用

    public Box(T data) { // 类型参数被作为构造函数的参数的类型使用
        this.data = data;
    }

    public void setData(T data) { // 类型参数被作为类中一个方法的参数的类型使用
        this.data = data;
    }

    public T getData() { // 类型参数被作为类中一个方法的返回值的类型使用
        return data;
    }
}
```

一个类可以有多个类型参数，只需要在类名后面的尖括号中定义多个类型参数即可。例如：

```java
class Pair<K, V> {
    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() {
        return key;
    }

    public V getValue() {
        return value;
    }
}
```

在创建对象时，我们需要指定对应的类型参数。例如：

```java
Box<Integer> intBox = new Box<>(42);
Pair<String, Integer> pair = new Pair<>("age", 18);
```

### 泛型接口

泛型接口和泛型类类似，只是泛型接口定义的是一个接口，而不是一个类。

```java
interface List<T> {
    void add(T element);
    T get(int index);
}
```

例如，上述代码就定义了一个列表接口，这个列表接口可以存储任意类型的元素。这个接口有两个方法，一个是 `add` 方法，用于向列表中添加元素，使用类型参数 `T` 作为参数类型；另一个是 `get` 方法，用于获取列表中指定位置的元素，返回类型为 `T`。

### 泛型方法

泛型方法是一个拥有一个或者多个类型参数的方法，这个类型参数可以在方法的参数、返回值等地方使用。

要定义泛型方法，需要在方法的返回值前面加上尖括号（`<>`），然后在尖括号中定义类型参数。

```java
public static <T> T getFirstElement(T[] array) {
    return array[0];
}
```

在上述示例中，这个方法有一个类型参数 `T`，这个类型参数可以在方法的参数、返回值等地方使用。例如，这个方法接受一个由类型 `T` 组成的数组，然后返回这个数组的第一个元素。

### 泛型通配符

有的时候，我们不关心具体的类型，那么我们可以使用泛型通配符 `?`。泛型通配符 `?` 表示任意类型，可以用在泛型类、泛型接口、泛型方法等地方。

```java
public static void printList(List<?> list) {
    for (Object element : list) {
        System.out.println(element);
    }
}
```

在上述示例中，`printList` 方法接受一个 `List` 类型的参数，这个 `List` 类型的元素类型是未知的，而且我们也不关心它的具体类型。因此，我们使用了泛型通配符 `?`。这时，这个 `list` 参数的类型可以是任何元素组成的列表。

有的时候我们希望泛型参数是某个类型的子类，那么我们可以使用泛型通配符 `extends`。例如：

```java
public static void printList(List<? extends Number> list) {
    for (Number element : list) {
        System.out.println(element);
    }
}
```

此时，`list` 参数这个列表的元素的类型就只能是 `Number` 类型或者 `Number` 的子类类型了。

同时，我们也可以使用泛型通配符 `super`，表示泛型参数是某个类型的父类。例如：

```java
public static void addNumbers(List<? super Integer> list) {
    list.add(42);
}
```

在上述示例中，`addNumbers` 方法接受一个 `List` 类型的参数，这个 `List` 类型的元素类型只能是 `Integer` 或者 `Integer` 的父类类型。

{% note color:red 注意！ 泛型通配符 `?` 只能用于声明泛型类型，不能用于创建对象。例如，`List<?> list = new ArrayList<>()` 是错误的。 %}

## 例题

Coming soon...

## 挑战

使用泛型实现 [数据结构：栈与队列](/java-ds-oop/stack-and-queue) 中的栈和队列以及 [数据结构：链表](/java-ds-oop/linked-list) 中的链表。

## 总结

泛型是 Java 中的一个重要特性，它允许我们在定义类、接口和方法时使用类型参数。泛型的使用提高了代码的复用性（可以不用为每个类型都实现一遍数据结构）和代码的安全性（编译器可以在编译时检查类型是否匹配）。

{% quot GL & HF! %}
