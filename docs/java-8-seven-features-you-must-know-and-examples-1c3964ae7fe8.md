# 你必须知道的 Java 8: 7 特性和例子

> 原文：<https://blog.devgenius.io/java-8-seven-features-you-must-know-and-examples-1c3964ae7fe8?source=collection_archive---------4----------------------->

![](img/2a10f950e736a30f34c1f5a2681433e2.png)

[来自 number8.com 的图片](https://number8.com/software-developer-salary-trends-2022/)

# JAVA 8 概述

2014 年 3 月 18 日，甲骨文发布 Java 8。虽然这是很久以前的事了，但 Java 8 仍然在许多应用程序和项目中使用，因为 Java 8 得到了重大更新，增加了几个新特性。让我们用示例代码来看看 Java 8 的所有特性。本文列出了 Java 8 的重要特性和示例代码，如 lambda 表达式、Java 流、函数接口、默认方法和日期时间 API 变化等。

# JAVA 8 特性

## λ表达式

Lambda 表达式在 Scala 等其他流行的编程语言中也很常见。这是 Java 第一次在 Java 8 上支持 Lambda 表达式。在 Java 中，Lambda expression 只是一个*匿名函数(*函数，没有名字，也没有绑定到标识符)。以下是 lambda 表达式的一些基本语法:

```
(parameters) -> expression

(parameters) -> { statements; }

() -> expression
```

一个著名的 lambda 表达式示例如下:

```
*//This function takes two parameters and return their a+b*
(a, b) -> a+b
```

Lambda 表达式通过实现函数接口中提供的单个抽象函数来实现函数接口。正如我们上面提到的 Lambda 表达式的基本示例，上面的公式接受两个参数:“A”和“b”，并返回它们的总和“a+b”。根据“a”和“b”的数据类型，该函数可能会在不同的位置使用多次。参数“a”和“b”将根据上下文匹配“int”或“Integer”和“string”。如果参数是“int/Integer ”,它会将两个数字相加；如果参数是“String ”,它会将两个字符串连接起来。

这是一个如何在 Java 8 中使用 Lambda 表达式的示例:

```
**interface I**CalculationInterface {
  **void** abstractFunc(**int** x, **int** y);
}

**public class** TechIsBeautiful {
  **public static void** main(String args[]) {
    ICalculationInterface output = (**int** x, **int** y) -> System.***out***.println(x+y);
    System.***out***.print(**"Output: "**);
    output.abstractFunc(100,100);
  }
}
```

上面代码的输出:

```
Output: 200Process finished with exit code 0
```

注意，数据类型在 Lambda 中不是必需的，您可以只使用它:

```
**interface I**CalculationInterface {
  **void** abstractFunc(**int** x, **int** y);
}

**public class** TechIsBeautiful {
  **public static void** main(String args[]) {
    ICalculationInterface output = (x, y) -> System.***out***.println(x+y);
    System.***out***.print(**"Output: "**);
    output.abstractFunc(100,100);
  }
}
```

## 功能界面

Java 8 中引入了一个名为“函数接口”的强大新特性。函数接口是只有一个抽象方法的接口。@FunctionalInterface 注释防止抽象方法被意外添加到函数接口中。函数接口最吸引人的特性之一是使用 lambda 表达式创建对象。我们可以使用匿名类创建一个接口，如下所示:

```
@FunctionalInterface
public interface IMyFunctionalInterface
{
    public void myMethod();
    @Override
    public String toString(); 
    @Override
    public boolean equals(Object obj);
}
```

## 接口支持默认和静态方法

在 Java 8 之前，接口不能有方法实现。从 Java 8 开始，我们可以向接口添加非抽象方法，从而允许用方法实现来声明接口。要使用具有方法实现的接口，可以使用“default”关键字。此外，Lambda 表达式功能主要是通过默认方法实现的。这是一个使用接口声明的示例，包括方法实现:

```
**interface** IMyInterface {
  **default void** defaultMethod(){
    *// Detail implementation of method which does not allow before Java 8* System.***out***.println(**"default method of interface"**);
  }
}

**public class** TechIsBeautiful **implements** IMyInterface{
  **public static void** main(String[] args){
    TechIsBeautiful techIsBeautiful = **new** TechIsBeautiful();
    techIsBeautiful.defaultMethod();
  }
}
```

上面代码的输出:

```
default method of interfaceProcess finished with exit code 0
```

## 选修课

Java 8 在“java.util”包中包含了一个可选类。公共最终类“Optional”有助于避免 Java 应用程序中的 NullPointerException。有助于减少运行时避免 nullPointerException 所需的空检查次数的可选帮助。我们可以使用可选的类来防止应用程序在生产中意外崩溃和终止。可选类具有检查给定变量的值是否存在的方法。

下面的示例代码演示了可选类的工作方式。为了验证这个应用程序中的字符串是否为空，我们利用可选类的“ofNullable”属性向用户抛出错误消息:

```
**import** java.util.Optional;

**public class** TechIsBeautiful {
  **public static void** main(String[] args) {
    String[] string = **new** String[100];
    Optional<String> isNull = Optional.*ofNullable*(string[1]);

    **if** (isNull.isPresent()) {
      String word = string[1].toUpperCase();
      System.***out***.print(string);
    }
    **else**{
      System.***out***.println(**"null string"**);
    }
  }
}
```

上面代码的输出:

```
null stringProcess finished with exit code 0
```

## 可迭代接口中的 forEach()方法

在 Java 8 中，Java.lang 接口支持一个“forEach”函数，这样它就可以迭代集合的项目。集合类使用它来迭代项，这扩展了 Iterable 接口。这种“forEach”帮助开发人员在编写代码时专注于业务逻辑。这是下面的示例代码，forEach 方法将 java.util.function.Consumer 对象作为参数，因此我们可以在一个可以重用的单独类中实现我们的逻辑。让我们看看“forEach”在一个简单的代码中是如何工作的:

```
**import** java.util.ArrayList;
**import** java.util.Iterator;
**import** java.util.List;
**import** java.util.function.Consumer;
**import** java.lang.Integer;

**public class** TechIsBeautiful {
  **public static void** main(String[] args) {
    *// Sample ArrayList* List<Integer> sampleIntegerList = **new** ArrayList<Integer>();

    *// Add element to ArrayList* **for**(**int** i=0; i<10; i++) {
      sampleIntegerList.add(i);
    }

    *// Traverse using Iterator* Iterator<Integer> iterator = sampleIntegerList.iterator();
    **while**(iterator.hasNext()){
      Integer i = iterator.next();
      System.***out***.println(**"Value in iterator: "**+i);
    }

    *// Traverse through forEach method of Iterable with anonymous class* sampleIntegerList.forEach(**new** Consumer<Integer>() {
      **public void** accept(Integer t) {
        System.***out***.println(**"Value in forEach anonymous class: "**+t);
      }
    });

    *// Traverse with Consumer interface implementation* CustomConsumer customConsumer = **new** CustomConsumer();
    sampleIntegerList.forEach(customConsumer);
  }
}

*// Custom Consumer that can be reused* **class** CustomConsumer **implements** Consumer<Integer>{
  **public void** accept(Integer t) {
    System.***out***.println(**"Value in Custom Consumer implementation: "**+t);
  }
}
```

上面代码的输出:

```
Value in iterator: 0
Value in iterator: 1
Value in iterator: 2
Value in iterator: 3
Value in iterator: 4
Value in iterator: 5
Value in iterator: 6
Value in iterator: 7
Value in iterator: 8
Value in iterator: 9
Value in forEach anonymous class: 0
Value in forEach anonymous class: 1
Value in forEach anonymous class: 2
Value in forEach anonymous class: 3
Value in forEach anonymous class: 4
Value in forEach anonymous class: 5
Value in forEach anonymous class: 6
Value in forEach anonymous class: 7
Value in forEach anonymous class: 8
Value in forEach anonymous class: 9
Value in Custom Consumer implementation: 0
Value in Custom Consumer implementation: 1
Value in Custom Consumer implementation: 2
Value in Custom Consumer implementation: 3
Value in Custom Consumer implementation: 4
Value in Custom Consumer implementation: 5
Value in Custom Consumer implementation: 6
Value in Custom Consumer implementation: 7
Value in Custom Consumer implementation: 8
Value in Custom Consumer implementation: 9Process finished with exit code 0
```

## 用于集合上批量数据操作的 Java 流 API

Java 8 的另一个重要特性是新的流 API。java 8 中的“java.util.stream”包引入了一个新的 Streams API，可以并行处理 Java 集合的组件。Java 8 中增加了“java.util.stream”来对集合执行 filter/map/reduce 之类的操作。流 API 将允许顺序和并行执行，因此这非常有用。

Java 集合接口已经用 *stream()* 和 *parallelStream()* 默认方法进行了扩展，以获得用于顺序和并行执行的流。以下示例将展示流的特征:

```
**import** java.util.ArrayList;
**import** java.util.List;
**import** java.util.stream.Stream;

**public class** TechIsBeautiful {

  **public static void** main(String[] args) {

    List<Integer> list = **new** ArrayList<>();
    **for**(**int** i=0; i<100; i++) list.add(i);

    *//sequential stream* Stream<Integer> seqStream = list.stream();

    *//parallel stream* Stream<Integer> parStream = list.parallelStream();

    *//using lambda with Stream API, filter example* Stream<Integer> highNums = parStream.filter(a -> a > 95);

    *//using lambda in forEach* highNums.forEach(a -> System.***out***.println(**"Lambda in forEach with parallel: "**+a));

    Stream<Integer> highNumsSeq = seqStream.filter(a -> a > 95);
    highNumsSeq.forEach(a -> System.***out***.println(**"Lambda in forEach with sequential: "**+a));

  }

}
```

上面代码的输出:

```
Lambda in forEach with parallel: 96
Lambda in forEach with parallel: 98
Lambda in forEach with parallel: 97
Lambda in forEach with parallel: 99
Lambda in forEach with sequential: 96
Lambda in forEach with sequential: 97
Lambda in forEach with sequential: 98
Lambda in forEach with sequential: 99Process finished with exit code 0
```

## Java 日期时间 API

过去，在 Java 中处理日期、时间和时区非常不方便。Java 中没有处理日期和时间的标准方法。Java 8 中一个很好的新增功能是`java.time`包，它将简化 Java 中处理时间的过程。Java 8 为“日期”和“时间”引入了新的 API，以解决旧的“java.util.Date”和“java.util.Calendar”的缺点。

## 集合，Java IO API 改进

**采集 API**

正如我们在 Java 8 中探索集合的“forEach”方法和流 API 一样，集合 API 中还有许多其他改进:

*   HashMap 类的性能改进。
*   地图新增`replaceAll()`、`compute()`、`merge()`方法。
*   `Iterator`默认方法`forEachRemaining(Consumer action)`对每个剩余的元素执行给定的动作，直到所有的元素都被处理完或者动作抛出异常。
*   `Collection`默认方法`removeIf(Predicate filter)`删除集合中满足给定谓词的所有元素

**Java IO API**

Java 8 中 Java IO API 有一些改进:

*   `Files.lines(Path path)`从一个文件中读取所有行作为一个流。
*   `Files.find()`它返回一个流，该流通过在以给定的起始文件为根的文件树中搜索文件来惰性地填充 Path。
*   返回一个流，其元素是从 BufferedReader 中读取的行。

# 摘要

在本文中，我们通过示例代码了解了 Java 8 的一些新特性和改进。当然，在 Java 8 JDK 包和类中还有许多其他的改进。然而，本文中分享的信息是探索和学习 Java 8 的一些新特性的一个很好的切入点。你可以阅读更多我的 Java 版本系列的新特性:

*   [你必须知道的 Java 17: 5 特性](https://medium.com/@techisbeautiful/java-17-top-5-features-you-must-know-bbed2afaea3d)
*   [你必须知道的 Java 11: 8 特性](https://medium.com/@techisbeautiful/new-features-you-must-know-in-java-11-and-examples-3fda2ad26def)
*   [你必须知道的 Java 8 : 7 特性](/java-8-seven-features-you-must-know-and-examples-1c3964ae7fe8)

如果你喜欢这个故事，请[关注](https://medium.com/@techisbeautiful)，[让我](https://medium.com/subscribe/@techisbeautiful)成为第一个收到我下一个故事邮件的人。

[你可以在这里](https://medium.com/@techisbeautiful/membership)成为媒介会员，拥有**无限访问**媒介平台上的每一个故事。如果你使用上面的链接，它也支持我，因为我有一个来自 Medium 的小佣金。谢谢大家！