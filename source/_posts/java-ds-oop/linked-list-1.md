---
title: 数据结构：链表（LinkedList）- 上
topic: java-ds-oop
date: 2024-03-28 12:59:46
categories:
  - Java 数据结构与面向对象编程
tags:
  - Java
  - 数据结构
  - 面向对象编程
  - 链表
  - LinkedList
references:
  - "[1] T. H. Cormen, C. E. Leiserson, R. L. Rivest, and C. Stein, \"Elementary Data Structures - 10.2 Linked lists\", *Introduction to Algorithms.* Cambridge, Massachusetts, USA: The MIT Press, 2022."
  - "[2] GeeksforGeeks, \"Linked List Data Structure,\" February 22, 2024. Accessed: Mar. 28, 2024.<br/>Available: https://www.geeksforgeeks.org/data-structures/linked-list/"
  - "[3] GeekforGeeks, \"Linked List vs Array,\" July 10, 2023. Accessed: Mar. 28, 2024.<br/>Available: https://www.geeksforgeeks.org/linked-list-vs-array/"
  - "[4] GeekforGeeks, \"Types of Linked List,\" Januray 29, 2024. Accessed: Mar. 28, 2024.<br/>Available: https://www.geeksforgeeks.org/types-of-linked-list/"
  - "[5] GeekforGeeks, \"Insertion in Linked List,\" February 22, 2024. Accessed: Mar. 28, 2024.<br/>Available: https://www.geeksforgeeks.org/insertion-in-linked-list/"
---

本文将简单介绍链表的概念，给出它们的 Java 实现以及原理解析。

<!-- more -->

## 对象的引用（指针）

在开始讲链表之前，我们先来看复习一下 Java 中的对象引用（Reference）。

考虑以下代码：

```java
class MyClass {
    public int value;

    public MyClass(int value) {
        this.value = value;
    }
}

public class Main {
    private static void changeValue(MyClass myClass) {
        myClass.value = 20;
    }

    public static void main(String[] args) {
        MyClass myClass = new MyClass(10);
        System.out.println(myClass.value);
        changeValue(myClass);
        System.out.println(myClass.value);
    }
}
```

第一次调用 `System.out.println(myClass.value);` 时，输出的是 `10`，但是第二次调用 `System.out.println(myClass.value);` 应该输出什么呢？

思考一下，两个可能的答案 `10` 和 `20`，哪个是正确的？

{% folding 答案 %}
正确答案是 `20`。
{% endfolding %}

虽然 Java 中没有明确定义指针（Pointer）这个概念，但是在 Java 中，对象引用可以看作是指向对象的指针。

一个非常简化的解释：

简单来讲，Java 中的每个对象都需要使用一整块内存存储，但是我们并不能永远找到这个对象从内存的哪个存储位置（地址）开始。所以，我们还会在内存中存储一个指向这个对象的指针（这个对象存储块开始位置的地址）。我们每次在代码中使用对象时，实际上是在使用这个指针。（这也是为什么没有 `toString` 方法的对象在输出时会显示一个十六进制的地址。）

在上述代码中，我们实际上并没有将对象 `myClass` 传递给 `changeValue` 方法，而是将指向对象 `myClass` 的指针传递给了 `changeValue` 方法。所以，当我们在 `changeValue` 方法中修改了对象的值时，实际上仍然在修改这个指针指向的对象的值，所以当我们回到 `main` 方法时，对象的值仍然是修改后的值。

理解这个概念对于理解链表的实现非常重要。

## 链表是什么？

链表，与数组、栈以及队列一样，是一种线性数据结构。

> 与数组不同的是，链表中的元素顺序并不以数组的下标为准，而以每个元素中包含的一个指向下一个元素的指针为准。这样的话，链表可以作为一个简单且灵活的数据结构，并支持搜索、插入、删除等操作（虽然链表的实现并不一定是最高效的）。
>
> Coremen, et al. (2022, p. 258) [1]

在链表中，每个元素都是一个节点（Node）。每个节点都包含两个数据：一个是该节点存储的数据 `data` ，另一个是指向下一个节点的指针 `next`。这样的设计允许我们以比数组高效的多的方式插入和删除元素。[2]

{% image https://img.cubik65536.top/ArrayRepresentation.png 一个简单的数组示例 [3] %}

{% image https://img.cubik65536.top/LinkedList.png 一个简单的链表示例 [3] %}

这种每个节点都只有一个指向下一个节点的指针的链表称为**单链表**（Singly Linked List）。

{% image https://img.cubik65536.top/SinglyLinkedList.png 一个单链表示例 [4] %}

（这个单链表示例与上面的链表示例其实一样，只不过这个链表没有把指向下一个节点的指针画到上一个节点里，所以与实际上的实现有些不同。不过由于这个图更加简洁，而且后面关于操作的示例也是基于这个图的，所以我还是在这里展示一下。）

链表也有其他类型，包括**双向链表**（Doubly Linked List，意味着每个节点有两个指针，一个指向前一个节点，一个指向后一个节点）、**循环链表**（Circular Linked List，意味着链表的最后一个节点的“下一个”指针指向第一个节点）以及**双向循环链表**（Doubly Circular Linked List，意味着链表的最后一个节点的“下一个”指针指向第一个节点，而第一个节点的“上一个”指针指向最后一个节点）。

{% image https://img.cubik65536.top/DoublyLinkedList.png 一个双向链表示例 [4] %}

{% image https://img.cubik65536.top/CircularLinkedList.png 一个循环链表示例 [4] %}

{% image https://img.cubik65536.top/DoublyCircularLinkedList.png 一个双向循环链表示例 [4] %}

为了简单起见：本文将主要讨论单链表的操作原理和实现。

## 链表的操作

链表主要支持以下几种操作：

- 遍历（Traverse）：遍历链表中的所有节点。
- 插入（Insert）：在链表中插入一个新节点。
- 删除（Delete）：在链表中删除一个节点。
- 搜索（Search）：在链表中搜索一个节点。
- 更新（Update）：更新链表中的一个节点。
- 排序（Sort）：对链表中的节点进行排序。

本文将仅讨论遍历、插入和删除操作。

### 遍历

遍历其实非常简单，只需要从链表的第一个节点开始，每次访问完一个节点，将目前正在操作的节点指针指向换成当前节点的“下一个”指针指向的节点即可。

```java
Node current = head; // 从链表的第一个节点（head）开始
while(current.next != null) { // 当前节点的“下一个”指针不为空时（即当前节点不是最后一个节点）
    System.out.println(current.data); // 获取当前节点的数据（如果需要的话）
    current = current.next; // 将当前节点指针指向下一个节点
}
System.out.println(current.data); // 输出最后一个节点的数据（如果需要的话）
```

### 插入

插入操作分为三种情况：

1. 在链表的头部插入一个新节点。
2. 在链表的尾部插入一个新节点。
3. 在链表的中间插入一个新节点。

#### 头部插入

头部插入仍然相对简单，只需要创建一个新节点，将新节点的“下一个”指针指向原来的头节点，然后将新节点设置为头节点即可。

```java
// 头部插入方法需要一个参数 data 作为新节点的数据
Node newNode = new Node(); // 创建一个新节点
newNode.data = data; // 设置新节点的数据
newNode.next = head; // 将新节点的“下一个”指针指向原来的头节点
head = newNode; // 将新节点设置为头节点
```

{% image https://img.cubik65536.top/Insertion-at-the-Beginning-of-Singly-Linked-List.png 在链表头部插入一个新节点 [5] %}

#### 尾部插入

尾部插入相对复杂一些，因为我们需要遍历整个链表，找到最后一个节点，然后将最后一个节点的“下一个”指针指向新节点。

```java
// 尾部插入方法也需要一个参数 data 作为新节点的数据
Node newNode = new Node(); // 创建一个新节点
newNode.data = 10; // 设置新节点的数据
newNode.next = null; // 将新节点的“下一个”指针设置为 null
if (head == null) { // 如果链表为空
    head = newNode; // 将新节点设置为头节点
} else {
    Node current = head; // 从头节点开始遍历
    while (current.next != null) { // 当前节点的“下一个”指针不为空时（即当前节点不是最后一个节点）
        current = current.next; // 将当前节点指针指向下一个节点
    }
    current.next = newNode; // 将最后一个节点的“下一个”指针指向新节点
}
```

{% image https://img.cubik65536.top/Insertion-at-the-End-of-Singly-Linked-List.png 在链表尾部插入一个新节点 [5] %}

#### 中间插入

中间插入操作与尾部插入操作类似，只不过我们需要在找到要插入的位置后停下来，将新节点的“下一个”指针指向当前节点的“下一个”指针指向的节点，然后将当前节点的“下一个”指针指向新节点。

```java
// 中间插入方法需要两个参数 data 和 index，分别表示新节点的数据和要插入的位置（从 0 开始）
if (index == 0) {
    // 实际上就是头部插入
    insertAtBeginning(data); // 由于已经有了头部插入方法，所以可以直接调用头部插入方法
} else {
    Node newNode = new Node(); // 创建一个新节点
    newNode.data = data; // 设置新节点的数据
    Node current = head; // 从头节点开始遍历
    for (int i = 1; i < index; i++) { // 遍历到要插入的位置的前一个位置
        current = current.next; // 如果还没到要插入的位置，就将当前节点指针指向下一个节点
    }
    // 到了要插入的位置后（此时 current 指向要插入的位置的前一个位置）
    newNode.next = current.next; // 将新节点的“下一个”指针指向当前节点的“下一个”指针指向的节点
    current.next = newNode; // 将当前节点的“下一个”指针指向新节点
}

```

{% image https://img.cubik65536.top/Insertion-at-a-Specific-Position-of-the-Singly-Linked-List.png 在链表中间插入一个新节点 [5] %}

### 删除

删除操作基本只有两种情况：

1. 删除头节点。
2. 删除中间或尾部节点。

删除头节点相当简单，只要将 `head` 指针指向头节点的“下一个”指针指向的节点即可。

而删除中间节点则需要遍历链表，找到要删除的节点的前一个节点，然后将前一个节点的“下一个”指针指向要删除节点的“下一个”指针指向的节点。

如果我们要删除的节点是最后一个节点，那么其实就是将前一个节点的“下一个”指针指向 `null`，不需要特殊处理。

```java
// 删除方法需要一个参数 index，表示要删除的节点的位置（从 0 开始）
if (index == 0) {
    // 实际上就是删除头节点
    Node temp = head; // 保存头节点
    head = head.next; // 将头节点指针指向头节点的“下一个”指针指向的节点
    temp = null; // 删除头节点，这一步实际上不是必要的
} else {
    Node current = head; // 从头节点开始遍历
    for (int i = 1; i < index; i++) { // 遍历到要删除的位置的前一个位置
        current = current.next; // 如果还没到要删除的位置，就将当前节点指针指向下一个节点
    }
    // 到了要删除的位置的前一个位置后（此时 current 指向要删除的位置的前一个位置）
    Node temp = current.next; // 保存要删除的节点
    current.next = temp.next; // 将前一个节点的“下一个”指针指向要删除节点的“下一个”指针指向的节点
    temp = null; // 删除要删除的节点
}
```

{% image https://img.cubik65536.top/Deletion-at-a-Specific-Position-of-Singly-Linked-List.png 在链表中间删除一个节点 [5] %}

## 例题

Coming soon...

## 预告

你有没有发现，我们上面的代码（以及之前关于栈和队列的文章中的代码）中都只提到了如何存储 int 类型的数据？那么，我们如果需要存储其他类型的数据怎么办呢？为每个类型都写一个实现显然是不现实的，所以我们需要使用泛型（Generic）。

在下一篇文章中，我们将讨论 Java 中的泛型，以及如何实现对应的栈、队列和链表。

再之后，我们会讨论链表的其他操作。

{% quot GL & HF! %}
