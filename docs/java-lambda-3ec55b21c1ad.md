# Java Lambda

> 原文：<https://blog.devgenius.io/java-lambda-3ec55b21c1ad?source=collection_archive---------8----------------------->

## Lambda 是一小段代码，它是一个没有名字的函数

# 概观

Lambda 是在版本 8 中添加到 Java 中的。与 Python lambda 类似的是一小段代码，它是一个没有名称的函数。像函数一样，它们可以接受参数并返回值。与 Pythons 不同，lambda Java 有自己使用 lambda 的方式。与 Python 中一样，lambda 函数通常用于高阶函数或接受另一个函数作为参数的函数。

![](img/f96b0cc5d9b18f286d5e9abc0b64fea2.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 履行

要使用 lambda，首先声明由->分隔的参数，然后声明函数定义。

```
// argument -> definition
x -> System.out.println(x)
```

如果 lambda 函数接受 1 个以上的参数或 not 参数，它需要一个括号。

```
(x, y) -> x + y
() -> System.out.println("hi")
```

如果 lambda 函数不止一行，你可以用花括号把定义括起来，但是如果它应该返回一个值，一定要加上。

```
(x, y) -> {
  System.out.println("x: " + x);
  System.out.println("y: " + y);
  return x+y;
}
```

# 分配

要将 lambda 函数赋给变量，必须使用消费者关键字声明变量。泛型声明函数将接受什么类型的参数。

```
Consumer<Integer> display = x -> System.out.println(x);
```

这可以由高阶方法使用。

```
ArrayList<Integer> numbers = new ArrayList<Integer>();
numbers.add(1);
numbers.add(2);
numbers.add(3);
numbers.add(4);
Consumer<Integer> display = x -> System.out.println(x);
numbers.forEach( display );>> 1
>> 2
>> 3
>> 4
```

要在高阶函数之外使用消费者方法，您必须使用接受方法。

```
Consumer<Integer> display = x -> System.out.println(x);
display.accept(1);>> 1
```

# 结论

使用 lambda 有助于减少代码行，并利用 Java 内置的高阶函数。它也用于函数式编程，效率也更高。总的来说，虽然没有必要使用 lambda，但它有助于保持代码的可读性。