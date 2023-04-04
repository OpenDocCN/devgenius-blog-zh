# 如果你不知道 Java 多态性，没关系？

> 原文：<https://blog.devgenius.io/its-okay-if-you-dont-know-java-polymorphism-31530df3c4e0?source=collection_archive---------6----------------------->

## 软件工程之旅

## 为了学习如何以不同的方式执行某些任务，软件工程师应该熟悉多态性的概念。

![](img/715030ea00640b71821f1b27fefb9137.png)

照片由来自 pianalytix 的 pianalytix mlops 拍摄

**简介**

让我们首先用一种非常通俗的语言来理解 Java 多态性到底是什么，这是一个我们可以用多种方式执行一项任务的概念。

单词 Polymorphism 由两个希腊单词组成，即 poly 和 morphs，分别表示许多和形式。

java 中有两种类型的多态性

1.  编译时多态性
2.  运行时多态性

# 编译时多态性

如果在 Java 中重载静态方法，这就是编译时多态性的一个例子。

但是在我们学习编译时多态性之前，我们应该了解“方法重载”，因为这是编译时多态性的原因。

它也被称为静态多态性。

# 方法重载:

当一个类有多个同名但参数不同的方法时，这就是所谓的方法重载。

它增加了程序的可读性。

假设您必须执行给定数字的加法，但是可以有任意数量的参数，如果您编写的方法(如 a(int，int)用于两个参数，b(int，int，int)用于三个参数)可能会让您和其他程序员很难理解该方法的行为，因为它的名称不同。

因此，我们执行方法重载来快速找出程序。

我们可以通过以下方式重载该方法

1.  通过改变参数的数量
2.  通过更改数据类型

**示例 1:** 通过改变参数的数量来重载方法。

```
class Adder{
static int add(int a,int b){return a+b;}
static int add(int a,int b,int c){return a+b+c;}
}

class TestOverloading1{
public static void main(String[] args){
System.out.println(Adder.add(11,11));
System.out.println(Adder.add(11,11,11));
} }
```

**输出:**

```
22
33
```

**例 2:** 通过改变数据类型实现方法重载。

```
class Adder{
static int add(int a, int b){return a+b;}
static double add(double a, double b){return a+b;}
}

class TestOverloading2{
public static void main(String[] args){
System.out.println(Adder.add(11,11));
System.out.println(Adder.add(12.3,12.6));
} }
```

**输出:**

```
22
24.9
```

**注意:重载时需要记住的几点:**

*   我们不能改变这个方法的返回类型，因为这会引起歧义
*   我们也可以重载 main 方法，一个类中可以有任意数量的 main 方法。但是 JVM 调用 main()方法，该方法只接收字符串数组作为参数。

**示例:**

```
class TestOverloading4{
public static void main(String[] args){System.out.println("main with String[]");}
public static void main(String args){System.out.println("main with String");}
public static void main(){System.out.println("main without args");}
}
```

**输出:**

```
main with String[]
```

# 运行时多态性

运行时多态也称为**动态方法分派**是一个在运行时而不是编译时解析对被覆盖方法的调用的过程。

在这个过程中，通过超类的引用变量调用被覆盖的方法。要调用的方法的确定基于引用变量所引用的对象。

**上抛**如果父类的引用变量引用了子类的对象。

**例子:**

```
class Bike{
void run(){System.out.println("running");}
}
class Splendour extends Bike{
void run(){System.out.println("running safely with 60km");}
public static void main(String args[]){
Bike b = new Splendor();//upcasting
b.run();
}
}
```

**输出:**

```
running safely with 60km.
```

# 方法覆盖

如果一个子类拥有与父类相同的方法，这在 Java 中被称为方法覆盖。方法重写用于运行时多态性。

Java 方法覆盖的规则

1.  该方法必须与父类中的名称相同
2.  该方法必须具有与父类中相同的参数。
3.  必须有一个 IS-A 关系(继承)

**举例:**

```
class Vehicle{
//defining a method
void run(){System.out.println("Vehicle is running");}
}
//Creating a child class
class Bike extends Vehicle{
//defining the same method as in the parent class
void run(){System.out.println("Bike is running safely");}
public static void main(String args[]){
Bike obj = new Bike();//creating object
obj.run();//calling method
}
}
```

**输出:**

```
Bike is running safely
```

# 使用数据成员的运行时多态性

方法可以被覆盖，但数据成员不能被覆盖，这意味着数据成员不能实现运行时多态性。

**举例:**

```
class Bike{
int speedlimit=90;
}
Class bike1 extends Bike{
int speedlimit=150;
public static void main(String args[]){
Bike obj=new bike1();
System.out.println(obj.speedlimit);//90
}
```

**输出:**

```
90
```

# 具有多级继承的 Java 运行时多态性

**举例:**

```
class Animal{
void eat(){System.out.println("eating");}
}
class Dog extends Animal{
void eat(){System.out.println("eating fruits");}
}
class labrador extends Dog{
void eat(){System.out.println("drinking milk");}
public static void main(String args[]){
Animal a1,a2,a3;
a1=new Animal();
a2=new Dog();
a3=new labrador();
a1.eat();
a2.eat();
a3.eat();
}
}
```

**输出:**

```
eating
eating fruits
drinking Milk
```

具有或不具有多态性的 Java 代码:

**无多态性**

```
class Rectangle{
int length;
int width;
void insert(int l, int w){
length=l;
width=w;
}
void calculateArea(){System.out.println(length*width);}
}
class TestRectangle1{
public static void main(String args[]){
Rectangle r1=new Rectangle();
Rectangle r2=new Rectangle();
r1.insert(11,5);
r2.insert(3,15);
r1.calculateArea();
r2.calculateArea();
}
}
```

**输出:**

```
55
45
```

**具有多态性**

```
class Rectangle {
// Overloaded Area() function to
// calculate the area of the rectangle
// It takes two double parameters
void Area(double S, double T)
{
System.out.println("Area of the rectangle: "
+ S * T);
}
// Overloaded Area() function to
// calculate the area of the rectangle.
// It takes two float parameters
void Area(int S, int T)
{
System.out.println("Area of the rectangle: "
+ S * T);
}
}
class ABC{
// Driver code
public static void main(String[] args)
{
// Creating object of Rectangle class
Rectangle obj = new Rectangle();
// Calling function
obj.Area(20, 10);
obj.Area(10.5, 5.5);
}
}
```

**输出:**

```
Area of the rectangle: 200
Area of the rectangle: 57.75
```

时间复杂度:O(1)

**多态性的优势**

*   它帮助程序员在编写、测试和实现之后重用代码和类。它们可以以多种方式重用。
*   单个变量名可用于存储多种数据类型的变量(Float、double、Long、Int 等)。
*   多态性有助于减少不同功能之间的耦合。

**多态性的缺点**

*   多态性的缺点之一是开发人员发现很难在代码中实现多态性。
*   运行时多态性会导致性能问题，因为机器需要决定调用哪个方法或变量，所以它基本上会降低性能，因为决策是在运行时做出的。
*   多态性降低了程序的可读性。人们需要识别程序的运行时行为来识别实际的执行时间。

## 结论

最后，我想对阅读我文章的读者说声谢谢。我希望它能帮助你增加关于多态性的知识。感谢阅读！