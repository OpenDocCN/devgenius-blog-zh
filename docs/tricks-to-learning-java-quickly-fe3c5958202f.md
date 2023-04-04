# 快速学习 Java 的窍门

> 原文：<https://blog.devgenius.io/tricks-to-learning-java-quickly-fe3c5958202f?source=collection_archive---------4----------------------->

![](img/b4942263be836bfb371ebe01871c4f84.png)

马里乌斯·特奥多尔斯库在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在这篇文章中，我将提到一些重要的技巧，它们将帮助您成长为一名 Java 开发人员，并获得更多关于该语言的知识。

## **1-掌握基本知识**

由于 Java 为开发人员提供了如此多的特性和选项，人们有时会被诱惑在太少的时间内学习太多的东西。

结果是，他们获得了 Java 提供的一些选项的零碎知识，但是他们的基础知识仍然悬而未决。

然而，Java 是一种编程语言，如果你关注简单的基础知识，它是很容易的；如果你变得贪婪，试图走更短的路，这可能会令人沮丧。

## **2。不要光看**

好吧，如果你学习 Java 的唯一目的是为了应付第二天的考试，那就继续努力，尽你所能地把所有的东西都记下来，这样你就有可能获得及格分数。

但是，如果你真的很想学习 Java，并且越学越好，最好的方法不是通过阅读，而是通过实现。

获取知识，然后以代码的形式执行你所学到的东西。

如果你不愿意弄脏你的手，你永远也学不会 Java。

## **3。理解你的代码和算法**

即使您正在编写一个包含 if else 语句的简单代码，作为初学者，也要从在一张纸上实现代码开始。

一旦你理解了代码背后的思想，算法和整个编译过程看起来就会很有意义。

即使对于专家来说，解决一个复杂问题或制定一个算法来解决一个 Java 程序的最好方法是将问题分解成子部分，然后尝试为每个子部分设计一个解决方案。

当你开始找到正确的解决方案时，你会有信心做更多的工作。

## **4。不要忘记分配内存**

这一招对于从 C、C++转 Java 的人特别有用。

在 Java 中使用 new 关键字进行内存分配是必要的，因为 Java 是一种动态编程语言。

C，C++没有明确的这个特性，因此在 Java 中处理数组和对象声明时必须小心。

不使用 new 关键字将在代码中显示空指针异常。

## **5。避免创建无用的对象**

当你用 Java 创建一个对象时，你会耗尽系统的内存和处理器速度。

由于没有分配内存，对象创建是不完整的，所以最好检查对象需求，不要在代码中创建不需要的对象。

## **6。接口优于抽象类**

Java 中没有多重继承，在学习这门语言的过程中，这一点会被灌输给你很多次，你可能一辈子都不会忘记。

然而，这里的技巧是不要忘记 Java 中没有多重继承，但是如果您想在不使用 extends 关键字的情况下实现类似多重继承的东西，interface 将会很方便。

记住，在 Java 中，当一切都不顺心的时候，你总会有接口在你身边。

然而，抽象类并不总是给程序员提供使用各种方法的自由；接口只有抽象方法。

因此，它完成了抽象类的工作，并且还有其他优点。

## **7。标准图书馆是一种福气**

从编程的角度来看，Java 相对于其前辈的最大优势可能是其丰富的标准库方法集。

使用 Java 的标准库使程序员的工作变得更容易、更高效，并给代码一个组织良好的流程。

此外，可以在库中指定的方法上容易地执行操作。

## **8。优先选择原始类而不是包装类**

毫无疑问，包装类非常有用，但是它们通常比原始类慢。

原始类只有值，而包装类存储整个类的信息。

此外，因为包装类经常处理对象值，所以像基本类那样比较它们并不能得到想要的结果，因为它最终比较的是对象而不是存储在其中的值。

示例:

`int` `number_1 = 10;`

`int`

`Integer wraperNum_1 = new` `Integer(10);`

`Integer wraperNum_2 = new` `Integer(10);`

`System.out.println(number_1 == number_2);`

`System.out.println(wraperNum_1 == wraperNum_2);`

在上面的例子中，第二个 print 语句不会显示 true，因为比较的是包装类对象，而不是它们的值。

## **9。处理字符串**

由于面向对象编程将 string 分类为一个类，所以两个字符串的简单连接可能会导致在 Java 中创建新的 String 对象。

这最终会影响系统的内存和速度。

最好是直接实例化一个字符串对象，而不要为此使用构造函数。

示例:

`String slowIntiat = new`

`String fastIntiat = "This string is faster"; //fast instantiation`

## 10。编码，编码，编码

关于 Java 有太多的东西需要学习，以至于你根本无法理解这种编程语言，然而它变得越来越有趣；保持学习的兴趣和变得更好的渴望是很重要的。

# 结论:

Java 编程语言可以通过我们自己的努力学习，并取得巨大的成功，但唯一需要的是不断学习和编码来测试你所学的东西。

Java 很像一项运动，练习时流汗越多，比赛时流血就越少。

**奖励:**

我会给你一个工具来帮助你听视频教程或课程，声音很好听。

这是[创意的 Pebble 2.0 USB 驱动桌面扬声器，带有远场驱动器和用于个人电脑和笔记本电脑的无源散热器(白色)](https://amzn.to/2Depszz)

受日本禅宗摇滚花园的启发，圆形创意 Pebble 是一款时尚优雅的 2.0 扬声器系统，在任何家庭和办公室都非常合适。
它配有一个 45°高架声场，用于增强音频投影，由一根 USB 电缆供电。

这将有助于你更好地理解视频课程，并促进你的职业生涯。

一些您可能感兴趣的相关文章:

1- [学习 JavaScript，成为专业人士的最佳途径](https://selcote.com/2020/08/25/the-best-way-to-learn-javascriptand-become-a-professional/) ( [视频](https://www.youtube.com/watch?v=VE116PwsbCg))

1- [最好和低成本的虚拟主机使用](https://selcote.com/2020/08/21/the-best-and-low-cost-web-hosting-to-use/)

2- [棱角开始慢慢枯萎](https://selcote.com/2020/08/19/angular-start-to-slowly-dying/) ( [视频](https://www.youtube.com/watch?v=iWLRpanlBjY&t=29s))

3- [4 本软件架构实用书籍](https://selcote.com/2020/08/12/4-practical-books-for-software-architecture/) ( [视频](https://www.youtube.com/watch?v=RWnI7z7NhAk))

4- [专业说明在跳到代码前的规格](https://selcote.com/2020/07/28/professional-illustrate-the-specifications-before-jumping-to-code/)

5- [设计不可示教](https://selcote.com/2020/06/17/the-design-cannot-be-taught/)

6- [类图是最流行和最复杂的](https://selcote.com/2020/06/12/class-diagram-is-the-most-popular-and-complex/)

7- [如何成为一名出色的解决问题的软件工程师](https://selcote.com/2020/05/30/how-to-be-a-great-problem-solver-software-engineer/)

8- [成为专业软件工程师的关键](https://selcote.com/2020/05/29/the-key-to-becoming-a-professional-software-engineer/)

与我联系:[博客](https://selcote.com/)， [Youtube](https://www.youtube.com/channel/UCU_LhClyNOtEQw7R0q9ovoQ?view_as=subscriber) ，[脸书](https://www.facebook.com/zelakioui)，[推特](https://twitter.com/zelakioui)

这个故事起源于:[selcote.com](https://selcote.com/2020/09/01/tricks-to-learning-java-quickly/)