# JS 系列#1:学习 JS 的第一步

> 原文：<https://blog.devgenius.io/first-step-of-learning-js-6f83e446a341?source=collection_archive---------10----------------------->

## JS 构建块|原始数据类型

如果你是一个 Java 脚本初学者，并且想探索从哪里开始…
那么你就在正确的地方…

![](img/049eb0c907c69dc3802f2d85ddab55ba.png)

由[元素 5 数码](https://unsplash.com/@element5digital?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在每一种编程中，甚至是在 JS 中，一个很好的起点是语言的基础构件。我们在这里学到的东西将会用在每一行代码中。

# 目标

1.  什么是数据类型？
2.  JS 中的原始类型

# 数据类型

可以根据类型、属性和可以执行的操作对数据进行分类和分组。每一种编程语言都提供了一种将信息组织成组的方法，这些组被称为数据类型…

例如，看看下面的网络表单…

![](img/58d96e34e4655750376d7a79220d4133.png)

webform 用于从用户那里获取数据。但是这个 webform 有不同类型的字段来捕获不同类型的数据。比如说…

1.  评级—评级可以是 1 到 5。所以它可以是**数值**类型的数据。
2.  评论、姓名、电子邮件—可以有文本类型的数据意味着**字符串**类型的数据。
3.  接受条款——可以检查，也可以不检查，意味着它可以是或否，真或假，因此它产生**布尔**类型的数据。

因此，我们捕获、存储和处理的数据可以有不同的格式或类型。

像 JavaScript 这样的编程语言都支持一些基本的数据类型，也称为原始数据类型。让我们看看 JS 支持的原始数据类型列表。

# 原始类型

1.  数字
2.  线
3.  布尔代数学体系的
4.  空
5.  不明确的

*技术上还有另外两个**符号**和 **BigInt** 。

让我们逐一讨论所有原始数据类型…

# 数字

数字是一种用于存储和表示数字的数据类型。一些编程语言如 C++，Java 有不同的数据类型来存储整数和十进制数。

与其他编程不同，JavaScript 只有一种数字数据类型。所以 Number 能够存储以下所有数字…

`50, 50.40, -67, 0.5555`

我们可以使用运算符对数字进行各种运算。看看可以对数字进行的简单运算。

## 线

字符串表示字符序列，字符串数据类型用于存储字符序列。下面给出了一些字符串值的示例…

```
"Alex"
"jhondeo@example.com"
'36 Bandra House - Mumbai'
'Drafting and article is really a fun'
```

注意，字符串值/文字总是用单引号**或双引号**或**括起来。**

## 布尔代数学体系的

布尔表示并存储布尔值，即**真**或**假**。计算布尔值的表达式也被视为布尔值。

**true** 和 **false** 是 JavaScript 中用来表示布尔值的两个保留值。甚至每一个 ***非零*** 值都被当作**真**和 ***零*** 被当作**假**。

```
var a = true;var b = false;var x = 5 > 4; // truevar y = 5 > 4 > 1 // false
```

上面的前三个表达式是不言自明的，让我们来关注最后一个表达式。

```
var y = 5 > 2 > 1
```

为了评估上述表达式，首先评估`5 > 4`，评估`true`，表达式变成`true > 1`，但是`true`和`1`是不同的数据类型，不能比较。所以`true` 被转换成一个等价的数值，即`1`，表达式变成`1 > 1`，它计算`false`。

## NULL &未定义

大多数 JavaScript 程序员认为 NULL 和 Undefined 的作用是一样的，但实际上它们是不同的。我们来了解一下区别。

## 不明确的

没有赋值的变量是未定义的。

```
var a;
console.log(a); // undefined
```

和上面的例子一样，变量`a`没有赋值，所以它返回`undefined`。

## 空

如果你想指定一个变量中没有任何值，必须用 NULL 来指定。

```
var a = 5;
console.log(a); // 5a = null;
console.log(a); //null
```

如果你喜欢这篇文章，请关注我:

**中:**[https://medium.com/@maheshshittlani](https://medium.com/@maheshshittlani)
**Github:**[https://github.com/maheshshittlani](https://github.com/maheshshittlani)
**LinkedIn:**[https://in.linkedin.com/in/mahesh-shittlani-638b7429](https://in.linkedin.com/in/mahesh-shittlani-638b7429)