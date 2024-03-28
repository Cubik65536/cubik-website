---
title: 数据结构：栈与队列
topic: java-ds-oop
date: 2024-03-27 19:04:52
categories:
  - Java 数据结构与面向对象编程
tags:
  - Java
  - 数据结构
  - 面向对象编程
  - 栈
  - 队列
  - Stack
  - Queue
references:
  - "[1] \"Stack (abstract data type),\" *Wikipedia*. Accessed: Mar. 27, 2024.<br/>Available: https://en.wikipedia.org/wiki/Stack_(abstract_data_type)"
  - "[2] \"Queue (abstract data type),\" *Wikipedia*. Accessed: Mar. 27, 2024.<br/>Available: https://en.wikipedia.org/wiki/Queue_(abstract_data_type)"
  - "[3] GeekforGeeks, \"Introduction to Circular Queue,\" July 7, 2023. Accessed: Mar. 27, 2024.<br/>Available: https://www.geeksforgeeks.org/introduction-to-circular-queue/"
---

本文将简单介绍栈和队列的概念，给出它们的 Java 实现以及原理解析。

<!-- more -->

## 栈（Stack）

栈是一种线性数据结构，这意味着每个数据节点（元素）都只有一个前驱和一个后继。栈的特点是后进先出（LIFO，Last In First Out），即最后入栈的元素最先出栈。

后进先出：想象一下，你面前有一叠盘子，你每次只能将新的盘子放在最上面，而取盘子时也只能从最上面取你最后放的盘子。

栈被操作的元素不以下标（index）为代表，而是以栈顶（top）为代表。栈顶是栈中最后一个元素，也是唯一一个可以被操作的元素。

栈的基本操作包括：

- `push`：将元素压入栈顶
- `pop`：将栈顶元素弹出，同时允许获取该元素的值

{% image https://img.cubik65536.top/LIFO-Stack.svg "一个栈的 push 和 pop 操作示意图 [1]" fancybox:true %}

栈还可以有其他操作，如：

- `peek`：查看栈顶元素
- `show`：显示栈中所有元素
- `isEmpty`：判断栈是否为空
- `isFull`：判断栈是否已满
- `size`：获取栈的大小（元素个数）

栈可以使用数组以及其他的数据结构（如 ArrayList、链表等）实现。下面是一个使用数组实现的栈的 Java 代码以及对应的原理解析。

```java
// 本实现基于维基百科的实现 [1]，并做了适当的简化
// 叫做 MyStack，而不是 Stack，是为了避免和 Java Util 中的 Stack 类重名造成混淆
class MyStack {
    private int[] elements = new int[10];   // 栈的容量为 10，这个实现是一个静态（固定大小）的栈
    private int top = 0;                    // 栈顶元素的下标，用于指示栈顶元素在数组中的位置

    /**
     * 将元素压入栈顶（入栈）
     * @param element 要入栈的元素
     */
    public void push(int element) {
        if (!isFull()) {
            // 仅在栈未满时才能入栈
            // 将元素放入栈顶，然后栈顶下标加 1，这样下次入栈时就会放在新的栈顶位置
            elements[top] = element;
            top++;
            // 也可以被写作
            // elements[top++] = element;
        } else {
            System.out.println("The stack is full. Cannot push any more elements.");
        }
    }

    /**
     * 将栈顶元素弹出（出栈）
     * @return 被弹出的元素（栈顶元素）
     */
    public int pop() {
        if (!isEmpty()) {
            // 只有在栈非空时才能出栈
            // 栈顶下标减 1，下标现在指向的是有元素的位置，然后返回该位置的元素
            top--;
            return elements[top];
            // 也可以被写作
            // return elements[--top];
        } else {
            // 栈为空时无法出栈
            System.out.println("The stack is empty. Cannot pop any more elements.");
            // 其实**绝对不应该**返回 -1，因为 -1 也是一个合法的元素，但为了简单起见，这里返回 -1
            return -1;
        }
    }

    /**
     * 显示栈中所有元素
     */
    public void show() {
        if (isEmpty()) {
            // 栈为空时无法显示任何元素
            System.out.println("The stack is empty. Cannot show any elements.");
            return;
        }
        for (int i = 0; i < top; i++) {
            // 从栈底到栈顶依次显示元素
            System.out.print(elements[i] + " ");
        }
        System.out.println();
    }

    /**
     * 查看栈顶元素
     */
    public void peek() {
        if (isEmpty()) {
            // 栈为空时无法查看栈顶元素
            System.out.println("The stack is empty. Cannot peek any element.");
        }
        // 栈顶元素就是数组中下标为 top - 1 的元素
        System.out.println("The top element is " + elements[top - 1]);
    }

    /**
     * 判断栈是否为空
     * @return 如果栈为空，返回 true；否则返回 false
     */
    public boolean isEmpty() {
        // 如果栈顶下标为 0，说明栈为空
        return top == 0;
    }

    /**
     * 判断栈是否已满
     * @return 如果栈已满，返回 true；否则返回 false
     */
    public boolean isFull() {
        // 如果栈顶下标等于数组的长度，说明栈已满
        return top == elements.length;
    }

    /**
     * 获取栈的大小（元素个数）
     * @return 栈的大小
     */
    public int size() {
        // top 的值就是栈的大小
        return top;
    }
}

public class Main {
    public static void main(String[] args) {
        // 可以自行测试 MyStack 类的功能，看看各项操作的表现
    }
}
```

但是，在上述代码中，栈的容量是固定的，无法动态调整。但是我们通常希望栈的容量是动态的，即可以根据需要自动扩展或缩小。这时我们有两种方法：

1. 使用数组实现栈，当栈满时，创建一个新的更大的数组，将原数组中的元素复制到新数组中，然后将新数组作为栈的存储结构。
2. 使用 ArrayList 实现栈。ArrayList 是一个动态数组，可以根据需要自动扩展或缩小。

我们将会使用第二种方法（因为相对简单），下面是一个使用 ArrayList 实现的栈的 Java 代码。

```java
package org.qianq;

import java.util.ArrayList;

class MyStackArrayList {
    private ArrayList<Integer> elements = new ArrayList<>();    // 用于存放栈中元素的数组
    private int top = 0;                                        // 栈顶元素的下标，用于指示栈顶元素在数组中的位置

    /**
     * 将元素压入栈顶（入栈）
     * @param element 要入栈的元素
     */
    public void push(int element) {
        // 实现与普通数组没有太大区别，只不过不再需要检查栈是否已满
        // 将元素放入栈顶，然后栈顶下标加 1，虽然 ArrayList 会自动扩展，但这里还是加上这一步，这样访问栈顶元素时更方便
        elements.add(element);
        top++;
    }

    /**
     * 将栈顶元素弹出（出栈）
     * @return 被弹出的元素（栈顶元素）
     */
    public int pop() {
        if (!isEmpty()) {
            // 只有在栈非空时才能出栈
            // 栈顶下标减 1，下标现在指向的是有元素的位置，然后返回该位置的元素
            top--;
            return elements.remove(top);
        } else {
            // 栈为空时无法出栈
            System.out.println("The stack is empty. Cannot pop any more elements.");
            // 其实**绝对不应该**返回 -1，因为 -1 也是一个合法的元素，但为了简单起见，这里返回 -1
            return -1;
        }
    }

    /**
     * 显示栈中所有元素
     */
    public void show() {
        if (isEmpty()) {
            // 栈为空时无法显示任何元素
            System.out.println("The stack is empty. Cannot show any elements.");
            return;
        }
        for (int i = 0; i < top; i++) {
            // 从栈底到栈顶依次显示元素
            System.out.print(elements.get(i) + " ");
        }
        System.out.println();
    }

    /**
     * 查看栈顶元素
     */
    public void peek() {
        if (isEmpty()) {
            // 栈为空时无法查看栈顶元素
            System.out.println("The stack is empty. Cannot peek any element.");
        }
        // 栈顶元素就是数组中下标为 top - 1 的元素
        System.out.println("The top element is " + elements.get(top - 1));
    }

    /**
     * 判断栈是否为空
     * @return 如果栈为空，返回 true；否则返回 false
     */
    public boolean isEmpty() {
        // 如果栈顶下标为 0，说明栈为空
        return top == 0;
    }

    /**
     * 获取栈的大小（元素个数）
     * @return 栈的大小（元素个数）
     */
    public int size() {
        // top 的值就是栈的大小
        return top;
    }
}

public class Main {
    public static void main(String[] args) {
        // 可以自行测试 MyStackArrayList 类的功能，看看各项操作的表现
    }
}
```

你会发现其实两个版本的代码并没有太大区别。这些就是常用的栈的实现方法。

## 队列（Queue）

{% image https://img.cubik65536.top/DataQueue.png "一个 Queue 在被操作时的示意图 [2]" fancybox:true %}

队列同样是一种线性数据结构，但它的特点是先进先出（FIFO，First In First Out），即最先入队的元素最先出队（就跟生活中排队一样）。

与栈不同，由于一个队列需要同时操作队首和队尾，因此队列有两个元素表示：队首（front）和队尾（rear）。队首是队列中第一个元素（最早入队的元素），队尾是队列中最后一个元素（最晚入队的元素）。

队列的基本操作包括：

- `enqueue`：将元素入队
- `dequeue`：将队首元素出队，同时允许获取该元素的值

队列还可以有其他操作，如：

- `show`：显示队列中所有元素
- `isEmpty`：判断队列是否为空
- `isFull`：判断队列是否已满

我们仍然从一个使用数组实现的队列开始：

```java
class MyQueue {
    private final static int CAPACITY = 10;     // 队列的最大容量
    private int[] elements = new int[CAPACITY]; // 用于存放队列元素的数组
    int front = 0, rear = -1, current_size = 0; // 队列的头指针、尾指针、当前元素个数（我们需要当前元素个数来判断队列是否为空或者满）

    /**
     * 将元素入队
     * @param element 要入队的元素
     */
    public void enqueue(int element) {
        if (isFull()) {
            // 如果队列已满，就不能再入队了
            System.out.println("The queue is full. Cannot enqueue.");
            return;
        }
        rear++;  // 尾下标后移，这样就可以将元素放入正确的位置
        elements[rear] = element;  // 将元素放入目前队列的队尾
        current_size++; // 当前元素个数加一
    }

    /**
     * 将元素出队
     * @return 被出队的元素
     */
    public int dequeue() {
        if (isEmpty()) {
            // 如果队列为空，就不能再出队了
            System.out.println("The queue is empty. Cannot dequeue.");
            // 仍然为了简单起见，我们返回 -1 表示出队失败
            return -1;
        }
        front++;  // 头下标后移，这样就可以将元素出队
        current_size--; // 当前元素个数减一
        return elements[front - 1];  // 返回出队的元素
    }

    /**
     * 显示队列中的元素
     */
    public void show() {
        for (int i = front; i <= rear; i++) {
            System.out.print(elements[i] + " "); // 基本上就是从头走到尾，然后输出每个元素
        }
        System.out.println();
    }

    /**
     * 判断队列是否为空
     * @return 如果队列为空，返回 true；否则返回 false
     */
    public boolean isEmpty() {
        return current_size == 0;
    }

    /**
     * 判断队列是否已满
     * @return 如果队列已满，返回 true；否则返回 false
     */
    public boolean isFull() {
        return current_size == CAPACITY;
    }
}

public class Main {
    public static void main(String[] args) {
        // 可以自行测试 MyQueue 类的功能，看看各项操作的表现
    }
}
```

但是，同样的，这个队列的容量是固定的，无法动态调整。我们虽然一样可以使用 ArrayList 来调整队列的容量，但是这里我们可以实现一个环形队列（Circular Queue）。

环形队列可以被想象成一个首尾相连的环。当任何一个指针到达数组的末尾时，它会回到数组的开头。这样，我们就可以重复利用数组中已经被出队的元素的空间（否则出队的元素所占的空间就会被浪费掉）。

{% image https://img.cubik65536.top/CircularQueue.png "一个环形队列的示意图 [3]" fancybox:true %}

```java
class MyCircularQueue {
    private final static int CAPACITY = 10;     // 队列的最大容量
    private int[] elements = new int[CAPACITY]; // 用于存放队列元素的数组
    int front = 0, rear = -1, current_size = 0; // 队列的头指针、尾指针、当前元素个数（我们需要当前元素个数来判断队列是否为空或者满）

    /**
     * 将元素入队
     * @param element 要入队的元素
     */
    public void enqueue(int element) {
        if (isFull()) {
            // 如果队列已满，就不能再入队了
            // 此处我们仍然需要检查队列是否已满，因为我们的队列是循环队列，如果不检查，可能会导致未出队的元素被覆盖
            System.out.println("The queue is full. Cannot enqueue.");
            return;
        }
        rear = (rear + 1) % CAPACITY; // 尾下标后移，这样就可以将元素放入队尾
        // 这里使用了取模运算，是为了实现循环队列，如果下标超出了数组的范围，到达了数组长度为值的下标，实际上就是回到了数组的开头（0）
        // 然后如果继续往前走，实际上就是继续经过下标为 1, 2, 3, ... 的位置
        elements[rear] = element;  // 将元素放入目前队列的队尾
        current_size++; // 当前元素个数加一
    }

    /**
     * 将元素出队
     * @return 被出队的元素
     */
    public int dequeue() {
        if (isEmpty()) {
            // 如果队列为空，就不能再出队了
            System.out.println("The queue is empty. Cannot dequeue.");
            // 仍然为了简单起见，我们返回 -1 表示出队失败
            return -1;
        }
        front = (front + 1) % CAPACITY; // 头下标后移，这样就可以将元素从队头取出
        current_size--; // 当前元素个数减一
        return elements[front - 1];  // 返回出队的元素
    }

    /**
     * 显示队列中的元素
     */
    public void show() {
        for (int i = 0; i < current_size; i++) {
            System.out.print(elements[(front + i) % CAPACITY] + " ");
            // 这样可以保证从队头开始，依次输出队列中的元素，取模运算是为了实现循环队列中的元素输出
        }
        System.out.println();
    }

    /**
     * 判断队列是否为空
     * @return 如果队列为空，返回 true；否则返回 false
     */
    public boolean isEmpty() {
        return current_size == 0;
    }

    /**
     * 判断队列是否已满
     * @return 如果队列已满，返回 true；否则返回 false
     */
    public boolean isFull() {
        return current_size == CAPACITY;
    }
}

public class Main {
    public static void main(String[] args) {
        // 可以自行测试 MyCircularQueue 类的功能，看看各项操作的表现
    }
}
```

## 例题

Coming soon...
