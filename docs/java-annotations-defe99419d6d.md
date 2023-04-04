# Java 注释

> 原文：<https://blog.devgenius.io/java-annotations-defe99419d6d?source=collection_archive---------4----------------------->

Java 注释是一个表示元数据的标签，即附加到类、接口、方法或字段上，以指示 Java 编译器和 JVM 可以使用的一些附加信息。

Java 中的注释用于提供附加信息，因此它是 XML 和 Java 标记接口的替代选项。

在继续创建和使用定制注释之前，我们将从一些内置注释开始。

内置 Java 注释

Java 包括几个内置注释。一些注释应用于 Java 代码，而另一些应用于其他注释。

# Java 代码中使用的内置 Java 注释

*   @覆盖
*   @ SuppressWarnings
*   @已弃用

# 在其他注释中使用的内置 Java 注释

*   @目标
*   @保留
*   @继承
*   @已记录

# 了解内置注释

让我们从内置注释开始。

# @覆盖

注释@Override 确保子类方法覆盖父类方法。如果不是这样，就会出现编译时错误。

有时我们会犯一些愚蠢的错误，比如拼写错误。因此，最好使用@Override 注释来确保该方法被覆盖。

```
class Animal{
void eatSomething(){System.out.println("eating something");}
}class Dog extends Animal{
@Override
void eatsomething(){System.out.println("eating foods");}//should be eatSomething
}class TestAnnotation1{
public static void main(String args[]){
Animal a = new Dog();
a.eatSomething();
}}
```

输出:

```
Compile Time Error
```

**@SuppressWarnings**

@SuppressWarnings 批注用于取消编译器警告。

```
import java.util.*;
class TestAnnotation2{
@SuppressWarnings("unchecked")
public static void main(String args[]){
ArrayList list = new ArrayList();
list.add("sonoo");
list.add("vimal");
list.add("ratan");for(Object obj:list)
System.out.println(obj);}}
```

**输出:**

```
Now no warning at compile time.
```

因为我们使用的是非泛型集合，所以移除@SuppressWarnings("unchecked ")注释将导致编译时出现警告。

# @已弃用

@Deprecated 批注指示此方法已被否决，编译器会打印一条警告。它通知用户它可能会在未来的版本中被删除。结果，优选不采用这种方法。

```
class A{
void m(){System.out.println("hello m");}@Deprecated
void n(){System.out.println("hello n");}
}class TestAnnotation3{
public static void main(String args[]){A a = new A();
a.n();
}}
```

**编译时:**

```
Note: Test.java uses or overrides a deprecated API.

Note: Recompile with -Xlint:deprecation for details.
```

**运行时:**

```
hello n
```

# Java 自定义注释

Java 自定义注释，也称为 Java 用户定义的注释，创建和使用都很简单。注释是使用@interface 元素声明的。举个例子:

```
@interface MyAnnotations{}
```

在这种情况下，MyAnnotation 是自定义注释的名称。Java 自定义注释签名的注意事项

有几点是程序员应该记住的。

1.  该方法中不应有 throws 子句。

2.该方法应返回以下数据类型之一:基元、字符串、类、枚举或这些数据类型的数组。

3.方法中不应该有参数。

4.要定义注释，我们应该在 interface 关键字前面加上@。

5.它可以给这个方法一个默认值。