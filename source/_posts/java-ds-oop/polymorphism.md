---
title: Java 多态 (polymorphism) - 继承与抽象类/接口
topic: java-ds-oop
date: 2024-02-22 22:20:28
categories:
  - Java 数据结构与面向对象编程
tags:
  - Java
  - 数据结构
  - 面向对象编程
  - 继承
  - 接口
  - 多态
---

本文将简单介绍 Java 中的多态 (polymorphism) 特性，包括继承、抽象类和接口。

<!-- more -->

## 多态 (Polymorphism)

> 在编程语言和类型论中，多态（英语：polymorphism）指为不同数据类型的实体提供统一的接口，或使用一个单一的符号来表示多个不同的类型。
>
> 多态的最常见主要类别有：
>
> 特设多态：为个体的特定类型的任意集合定义一个共同接口。
> 参数多态：指定一个或多个类型不靠名字而是靠可以标识任何类型的抽象符号。
> 子类型（也叫做子类型多态或包含多态）：一个名字指称很多不同的类的实例，这些类有某个共同的超类。
>
> <sub>维基百科编者. c2024. 多态 (计算机科学). 来源: 维基百科, 自由的百科全书. [Internet]: 维基百科, 自由的百科全书; [cited 2024 February 10]. Available from: https://zh.wikipedia.org/w/index.php?title=%E5%A4%9A%E6%80%81_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)&oldid=80877272</sub>

简单来讲，多态允许我们在程序的不同部分使用一样的符号来表述不同的东西（例如，具有同样名称不同实现的方法）。

多态一般来讲有两种形式，动态多态 (dynamic polymorphism) 和静态多态 (static polymorphism)。动态多态是指在运行时根据对象的实际类型来调用方法，而静态多态是指在编译时根据对象的引用类型来调用方法。

## 函数重载 (Function Overloading)

我们先来看一下以前见到过的一中语言特性：函数重载。这是一种静态多态，允许我们在同一个类中定义多个同名方法，只要它们的参数类型和/或数量不同即可。

``` java
public class Main {
    private static boolean isEqual(int a, int b) {
        return a == b;
    }

    private static boolean isEqual(double a, double b) {
        return a == b;
    }

    private static boolean isEqual(String a, String b) {
        return a.equals(b);
    }

    public static void main(String[] args) {
        /* 代码 */
    }
}
```

上述代码展现了典型的函数重载，我们定义了三个同名方法 `isEqual`，分别接受两个 `int`、两个 `double` 和两个 `String` 类型的参数。这样，我们可以在不同的情况下使用同一个方法名来比较不同类型的数据。

## 继承 (Inheritance)

继承是面向对象编程中的一个重要特性，它允许我们定义一个子类，这个子类可以继承另一个类（父类）的属性和方法。这样，我们可以在不同但相关的类之间共享一些方法的实现。

注意，在 Java 中，一个子类只能继承一个父类。

``` java
class Parent {
    String name = "PARENT";

    public void show() {
        System.out.println("SHOW");
    }
}

class Child extends Parent {
    int justANumber = 42;

    public void display() {
        System.out.println("DISPLAY");
    }
}

public class Main {
    public static void main(String[] args) {
        Child c = new Child();
        c.show();
        c.display();
        System.out.println(c.name);
        System.out.println(c.justANumber);
    }
}
```

在上述代码中，我们定义了一个 `Parent` 类，它有一个 `show` 方法。然后我们定义了一个 `Child` 类，它通过 `extends` 关键字继承了 `Parent` 类。注意，就算我们并未在 `Child` 类中定义 `show` 方法，我们仍然可以在 `Child` 类的对象上调用 `show` 方法，因为 `Child` 类继承了 `Parent` 类的 `show` 方法。属性 `name` 同理，就算我们并未在 `Child` 类中定义 `name` 属性，我们仍然可以在 `Child` 类的对象上访问 `name` 属性，因为 `Child` 类继承了 `Parent` 类的 `name` 属性。

同时，我们也仍然可以为 `Child` 类定义自己的方法或者属性，例如 `display` 方法和 `justANumber` 属性。

但是，如果子类需要一个与父类有差别的方法，我们就需要进行方法重写 (method overriding)。

``` java
class Parent {
    public void show() {
        System.out.println("SHOW PARENT");
    }
}

class Child extends Parent {
    @Override
    public void show() {
        System.out.println("SHOW CHILD");
    }
}

public class Main {
    public static void main(String[] args) {
        Child c = new Child();
        c.show();
    }
}
```

在上面的代码中，最终的输出是 `SHOW CHILD`。这是因为 `Child` 类重写了 `Parent` 类的 `show` 方法。在调用 `c.show()` 时，子类单独的的 `show` 方法会被调用。

注意，在重写方法时，我们可以函数签名前加上 `@Override` 注解来明确表示我们是在重写父类的方法（该注解并非强制要求）。这样有两个好处：

1. 如果子类的方法签名与父类的方法签名不同，编译器会报错。
2. 增加了代码的可读性，让人一眼就能看出这是一个重写方法。

对于子类与父类的构造器，如果子类没有显式地调用父类的构造器，那么会默认调用父类的无参构造器。如果父类没有无参构造器，那么子类必须显式地调用父类的构造器。

``` java
class Parent {
    public void show() {
        System.out.println("SHOW PARENT");
    }
}

class Child extends Parent {
    int age;

    public Child(int age) {
        super(); // 调用父类的无参构造器，这一行可以省略
        this.age = age;
    }

    @Override
    public void show() {
        System.out.println("SHOW CHILD");
    }
}

public class Main {
    public static void main(String[] args) {
        Child c = new Child(10);
        c.show();
    }
}
```

``` java
class Parent {
    String name;

    public Parent(String name) {
        this.name = "PARENT";
    }

    public void show() {
        System.out.println("SHOW PARENT");
    }
}

class Child extends Parent {
    int age;

    public Child(String name, int age) {
        super(name); // 父类没有无参构造器，所以必须显式调用父类的构造器，而且该调用必须放在子类构造器的第一行
        this.age = age;
    }

    @Override
    public void show() {
        System.out.println("SHOW CHILD");
    }
}

public class Main {
    public static void main(String[] args) {
        Child c = new Child("Austin", 10);
        c.show();
    }
}
```

正如我们可以使用 `this` 关键字来明确的引用当前对象的属性和方法，我们也可以使用 `super` 关键字来引用父类的属性和方法。

``` java
class Parent {
    int age;

    public Parent(int age) {
        this.age = age;
    }

    public void show() {
        System.out.println("PARENT SHOW");
    }
}

class Child extends Parent {
    int age;

    public Child(int age) {
        super(age + 25);
        this.age = age;
    }

    public void show() {
        System.out.println("Parent age: " + super.age);
        System.out.println("Child age: " + age);
    }

    public void display() {
        super.show();
        show();
    }
}

public class Main {
    public static void main(String[] args) {
        Child c = new Child(10);
        c.display();
    }
}
```

在上面的代码中，最终的输出是：

``` plain
PARENT SHOW
Parent age: 35
Child age: 10
```

因为 `display` 方法首先调用了父类的 `show` 方法，显示了 "PARENT SHOW"，然后调用了子类的 `show` 方法，显示了父类和子类的 `age` 属性。

你甚至可以在子类的重写方法中调用父类中被重写的方法。这在需要在子类中扩展父类方法的功能时非常有用。例如：

``` java
class Cube {
    int length;
    int breadth;
    int height;

    public Cube(int length, int breadth, int height) {
        this.length = length;
        this.breadth = breadth;
        this.height = height;
    }

    public String getInfo() {
        return "Length: " + length + ", Breadth: " + breadth + ", Height: " + height;
    }
}

class Pool extends Cube {
    String location;

    public Pool(int length, int breadth, int height, String location) {
        super(length, breadth, height);
        this.location = location;
    }

    public String getInfo() {
        return super.getInfo() + ", Location: " + location;
    }
}

public class Main {
    public static void main(String[] args) {
        Pool pool = new Pool(10, 20, 30, "Backyard");
        System.out.println(pool.getInfo());
    }
}
```

在上面的代码中，我们在子类的 `getInfo` 方法中调用了父类的 `getInfo` 方法，利用了父类提供的信息，然后再加上了子类的信息。

最后，每个子类也都可以有自己的子类，这样就形成了一个继承的层级。而且就算每个子类只可以继承一个父类，但是一个父类可以有多个子类。

``` java
class Parent {
    public void show() {
        System.out.println("SHOW PARENT");
    }
}

class ChildA extends Parent {
    @Override
    public void show() {
        super.show();
        System.out.println("SHOW CHILDA");
    }
}

class ChildB extends Parent {
    @Override
    public void show() {
        super.show();
        System.out.println("SHOW CHILDB");
    }
}

class GrandChild extends ChildA {
    @Override
    public void show() {
        super.show();
        System.out.println("SHOW GRANDCHILD");
    }
}

public class Main {
    public static void main(String[] args) {
        GrandChild gc = new GrandChild();
        gc.show();
    }
}
```

### 例题

1. 创建一个 `Animal` 类，它有一个 `getLegNumber` 方法，返回动物的腿的数量，默认为 2。然后分别创建一个 `Human`，一个 `Kangaroo`，一个 `Horse` 和一个 `Snake` 类，它们都继承自 `Animal` 类，但是它们的腿的数量分别为 2，2，4 和 0。最后，输出它们的腿的数量。

    {% folding 解题方法 %}

    ``` java
    class Animal {
        String name;

        public Animal(String name) {
            this.name = name;
        }

        public int getLegNumber() {
            return 2;
        }
    }

    class Human extends Animal {
        public Human(String name) {
            super(name);
        }
    }

    class Kangaroo extends Animal {
        public Kangaroo(String name) {
            super(name);
        }
    }

    class Horse extends Animal {
        public Horse(String name) {
            super(name);
        }

        @Override
        public int getLegNumber() {
            return 4;
        }
    }

    class Snake extends Animal {
        public Snake(String name) {
            super(name);
        }

        @Override
        public int getLegNumber() {
            return 0;
        }
    }

    public class Main {
        public static void main(String[] args) {
            Human human = new Human("Human");
            System.out.printf("%s has %d legs\n", human.name, human.getLegNumber());

            Kangaroo kangaroo = new Kangaroo("Kangaroo");
            System.out.printf("%s has %d legs\n", kangaroo.name, kangaroo.getLegNumber());

            Horse horse = new Horse("Horse");
            System.out.printf("%s has %d legs\n", horse.name, horse.getLegNumber());

            Snake snake = new Snake("Snake");
            System.out.printf("%s has %d legs\n", snake.name, snake.getLegNumber());
        }
    }
    ```

    {% endfolding %}

### 冷知识 #1

你知道其实，就算你不写任何继承关系，所有的类也都是子类吗？

Java 中的所有类都继承自 `Object` 类。`Object` 类定义了一些通用的方法，例如 `toString`、`equals` 和 `hashCode` 等。这意味着，你可以在任何一个类的对象上调用 `toString` 方法，你每次在自己的类中实现一个 `toString` 方法时，你都是在重写 `Object` 类中的 `toString` 方法。

### 冷知识 #2

你知道如果你声明一个类型为父类的对象，你实际上可以传入一个子类的对象吗？

这甚至能被用在数组中。例如：

``` java
class Parent {
    public void show() {
        System.out.println("SHOW PARENT");
    }
}

class ChildA extends Parent {
    @Override
    public void show() {
        super.show();
        System.out.println("SHOW CHILDA");
    }
}

class ChildB extends Parent {
    @Override
    public void show() {
        super.show();
        System.out.println("SHOW CHILDB");
    }
}

class GrandChild extends ChildA {
    @Override
    public void show() {
        super.show();
        System.out.println("SHOW GRANDCHILD");
    }
}

public class Main {
    public static void main(String[] args) {
        Parent a = new ChildA(); // 这样是可以的

        Parent[] arr = new Parent[4]; // 这样也是可以的
        arr[0] = new Parent();
        arr[1] = new ChildA();
        arr[2] = new ChildB();
        arr[3] = new GrandChild();

        // ChildA b = new Parent(); // 这样是不可以的
    }
}
```

## 抽象类 (Abstract Class)

但是，有的时候我们并不希望父类提供一个默认的实现，而希望父类只给出一个对方法的定义 (抽象的含义就在此处)，然后要求子类去实现这个方法。

这种方式允许我们为一堆同类型但细节实现完全不相干的类定义一个共同的方法。

例如：定义一个类 `Shape`，而每个形状有一定有周长与面积，但是每个形状的周长与面积的计算方法都不一样。这时我们可以将 `Shape` 类定义为一个抽象类，然后定义一个抽象方法 `getPerimeter` 和一个抽象方法 `getArea`，然后让每个形状的类去继承 `Shape` 类，并实现这两个抽象方法。

``` java
abstract class Shape { // 在 class 关键字前加上 abstract 关键字以声明该类为抽象类
    abstract double getPerimeter(); // 在方法签名前加上 abstract 关键字以声明该方法为抽象方法，抽象方法没有方法体（具体实现），只有方法签名并以分号结束
    abstract double getArea();

    public void showInfo() { // 一个抽象类可以有非抽象方法（但只要有抽象方法的类一定是抽象类），这些方法可以有具体实现，只要不在签名开头加上 abstract 关键字的方法都是非抽象方法
        System.out.println("Perimeter: " + getPerimeter());
        System.out.println("Area: " + getArea());
    }
}

class Circle extends Shape { // 真实且有具体实现的类需要继承抽象类，并实现抽象方法
    private double radius;

    Circle(double radius) {
        this.radius = radius;
    }

    @Override // 实际上，对抽象方法的实现就是在重写父类的，没有任何具体实现的抽象方法
    // 如果子类没有实现父类的抽象方法且子类不是抽象类，那么编译器会报错
    double getPerimeter() {
        return 2 * Math.PI * radius;
    }

    @Override
    double getArea() {
        return Math.PI * radius * radius;
    }
}

class Rectangle extends Shape {
    private double width;
    private double height;

    Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    double getPerimeter() {
        return 2 * (width + height);
    }

    @Override
    double getArea() {
        return width * height;
    }
}

public class Main {
    public static void main(String[] args) {
        Circle circle = new Circle(5);
        Rectangle rectangle = new Rectangle(3, 4);

        System.out.println("Circle perimeter: " + circle.getPerimeter());
        System.out.println("Circle area: " + circle.getArea());
        System.out.println("Rectangle perimeter: " + rectangle.getPerimeter());
        System.out.println("Rectangle area: " + rectangle.getArea());
    }
}
```

## 接口 (Interface)

但实际上，我们并不常使用抽象类，而是使用接口。接口是一种特殊的抽象类，它不需要你在抽象方法前加上 `abstract` 关键字，而是需要在非抽象方法前加上 `default` 关键字。在抽象类/接口的日常使用情况中，大部分需要被定义的都是抽象方法，所以省去 `abstract` 关键字会使代码更加简洁。

值得注意的是，接口中仍然可以定义属性，这些属性默认是公开 (public) 静态 (static) 常量 (final) 。

同时，与继承不同，一个类可以实现多个接口。

``` java
interface FirstInterface {
    int A = 10; // public static final 三个关键字由于在接口中定义属性时是默认的，所以可以省略，所以该定义等效于 public static final int A = 10;
                // 由于这是一个常量，所以使用 SCREAMING_SNAKE_CASE 命名规则
    void show();
}

interface SecondInterface {
    int B = 20;
    void display();

    static void print() { // 接口中可以定义静态方法，这些方法只能通过接口名调用
        System.out.println("Static method of SecondInterface");
    }
    
    default void printDefault() { // 接口中可以定义默认方法，这些方法可以通过类的对象调用，而且实现该接口的类可以选择是否重写该方法
        System.out.println("Default method of SecondInterface");
    }
}

class MyClass implements FirstInterface, SecondInterface { // 一个类可以实现多个接口
    public void show() { // 对接口中抽象方法的实现，实际上这仍然是在重写接口中的抽象方法，但是不同的是在这种情况下一般不加 @Override 注解
        System.out.println("Show method of FirstInterface");
    }

    public void display() {
        System.out.println("Display method of SecondInterface");
    }
}

public class Main {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        obj.show();
        obj.display();
        System.out.println("Value of a: " + obj.a); // 接口中的静态常量属性可以直接通过类的对象访问
        System.out.println("Value of b: " + obj.b);
        System.out.println("Value of a: " + FirstInterface.a); // 也可以通过接口名访问
        System.out.println("Value of b: " + SecondInterface.b);
        System.out.println("Value of b: " + MyClass.b); // 也可以通过类名访问
        SecondInterface.print();
        obj.printDefault();
    }
}
```

### 例题

1. 创建一个正多边形 (`RegularPolygon`) 接口，它有四个抽象方法 `getSideNum`、`getSideLength`、`showPerimeter` 和 `showInteriorAngle`。然后分别创建一个 `EquilateralTriangle` 和一个 `Square` 类，它们都实现了 `RegularPolygon` 接口。最后，输出他们的周长、面积以及内角角度。

   - `getSideNum` 方法返回正多边形的边数。
   - `getSideLength` 方法返回正多边形的边长。
   - `showPerimeter` 方法显示正多边形的周长。
   - `showInteriorAngle` 方法显示正多边形的内角角度。
     - 角度制中，内角的角度为 $\frac{180 (n - 2)}{n} $，其中 $n$ 为正多边形的边数。
     - 弧度制中，内角的角度为 $\frac{\pi (n - 2)}{n} $，其中 $n$ 为正多边形的边数。

   {% folding 解题方法 %}

   ``` java
   interface RegularPolygon {
       double getSideNum();
       double getSideLength();

       default void showPerimeter() {
           double perimeter = getSideNum() * getSideLength(); // 是的，接口中的默认方法可以调用接口中的抽象方法，具体使用哪里的实现取决于你在哪个实现类的对象上调用该方法
           System.out.println("Perimeter: " + perimeter);
       }
       default void showInteriorAngle() {
           double interiorAngleDeg = (getSideNum() - 2) * 180 / getSideNum();
           double interiorAngleRad = (getSideNum() - 2) * Math.PI / getSideNum();
           System.out.println("Interior angle: " + interiorAngleDeg + "° or " + interiorAngleRad + " rad");
       }
   }

   class EquilateralTriangle implements RegularPolygon {
       private double sideNum = 3;
       private double sideLength;

       public EquilateralTriangle(double sideLength) {
           this.sideLength = sideLength;
       }

       public double getSideNum() {
           return sideNum;
       }

       public double getSideLength() {
           return sideLength;
       }
   }

   class Square implements RegularPolygon {
       private double sideNum = 4;
       private double sideLength;

       public Square(double sideLength) {
           this.sideLength = sideLength;
       }

       public double getSideNum() {
           return sideNum;
       }

       public double getSideLength() {
           return sideLength;
       }
   }

   public class Main {
       public static void main(String[] args) {
           EquilateralTriangle equilateralTriangle = new EquilateralTriangle(3);
           equilateralTriangle.showPerimeter();
           equilateralTriangle.showInteriorAngle();

           Square square = new Square(3);
           square.showPerimeter();
           square.showInteriorAngle();
       }
   }
   ```

   {% endfolding %}

2. 创建一个 `Employee` 类，它需要：

   - 实现三个接口
     - `PersonalInfo` 接口，它有一个抽象方法 `getPersonalInfo`，以 String 数组返回个人信息 (姓名、年龄、地址)。
     - `EducationInfo` 接口，它有一个抽象方法 `getEducationInfo`，以 String 数组返回教育信息 (学校、专业、入学年份)。
     - `WorkInfo` 接口，它有一个抽象方法 `getWorkInfo`，以 String 数组返回工作信息 (职位、薪资)。
   - 继承 `Family` 类，它有两个属性，一个是家庭人数，一个是本家庭是否为低收入家庭。它还有一个 `show` 方法显示相关信息。
   - `Employee` 类需要有自己的构造器以及一个特殊方法 `isEligibleForPromotion`，它返回一个布尔值，表示该员工是否有资格晋升。
     - 晋升条件：
       - 年龄大于 45，家庭人数大于等于 2，职位是经理 (`manager`)，或者
       - 年龄大于 50，学位是博士 (`Ph.D.`)，薪资小于 5000

   {% folding 解题方法 %}

   ``` java
   interface PersonalInfo {
       String[] getPersonalInfo(String name, int age, String location);
   }

   interface EducationInfo {
       String[] getEducationInfo(String school, String fieldOfStudy, int graduationYear);
   }

   interface WorkInfo {
       String[] getWorkInfo(String position, int salary);
   }

   class Family {
       int familyMemberNumber;
       boolean isLowIncome;

       public Family(int familyMemberNumber, boolean isLowIncome) {
           this.familyMemberNumber = familyMemberNumber;
           this.isLowIncome = isLowIncome;
       }

       public void show() {
           System.out.println("Family member number: " + familyMemberNumber);
           System.out.println("Is low income: " + isLowIncome);
       }
   }

   class Employee extends Family implements PersonalInfo, EducationInfo, WorkInfo {
       String[] personalInfo;
       String[] educationInfo;
       String[] workInfo;

       public String[] getPersonalInfo(String name, int age, String location) {
           return new String[]{name, String.valueOf(age), location};
       }

       public String[] getEducationInfo(String school, String fieldOfStudy, int graduationYear) {
           return new String[]{school, fieldOfStudy, String.valueOf(graduationYear)};
       }

       public String[] getWorkInfo(String position, int salary) {
           return new String[]{position, String.valueOf(salary)};
       }

       public Employee(
               String name, int age, String location,
               String school, String fieldOfStudy, int graduationYear,
               String position, int salary,
               int familyMemberNumber, boolean isLowIncome
       ) {
           super(familyMemberNumber, isLowIncome);
           personalInfo = getPersonalInfo(name, age, location);
           educationInfo = getEducationInfo(school, fieldOfStudy, graduationYear);
           workInfo = getWorkInfo(position, salary);
       }

       public boolean isEligibleForPromotion() {
           return (
                   Integer.parseInt(personalInfo[1]) > 45 && familyMemberNumber >= 2 && workInfo[0].equalsIgnoreCase("manager")
           ) || (
                   Integer.parseInt(personalInfo[1]) > 50 && educationInfo[1].equals("Ph.D.") && Integer.parseInt(workInfo[1]) < 5000
           );
       }
   }

   public class Main {
       public static void main(String[] args) {
           Employee employee1 = new Employee(
                   "John", 46, "New York",
                   "Harvard", "Ph.D.", 2010,
                   "Manager", 56000,
                   3, false
           );
           System.out.println(employee1.isEligibleForPromotion());

           Employee employee2 = new Employee(
                   "Alice", 52, "New York",
                   "Harvard", "Ph.D.", 2010,
                   "Manager", 4000,
                   3, true
           );
           System.out.println(employee2.isEligibleForPromotion());

           Employee employee3 = new Employee(
                   "Bob", 46, "New York",
                   "Harvard", "Ph.D.", 2010,
                   "Manager", 56000,
                   1, false
           );
           System.out.println(employee3.isEligibleForPromotion());
       }
   }
   ```

   上述代码中最特殊的一点是：没错，一个类可以同时继承另一个类并实现一个或多个接口。具体语法是：

   ``` java
   class MyClass extends ParentClass implements Interface1, Interface2, Interface3
   // class 类名 extends 父类名 implements 接口名1, 接口名2, 接口名3
   ```

   {% endfolding %}
