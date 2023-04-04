# 大 O 符号的大混乱

> 原文：<https://blog.devgenius.io/the-big-confusion-of-big-o-notation-f23a9235d47d?source=collection_archive---------6----------------------->

![](img/0d4dba53b9215a9d275fdb3b043e121f.png)

上周我开始钻研算法。这是一条漫长而艰难的路，我也很挣扎。

如果你犹豫从哪里开始，我建议你先了解基础知识。这不是真的关于代码，更多的是关于数学概念，但是这会给你你需要的背景知识，并且你会意外地明白为什么我们应该学习算法。

是的，我说的是大 O 符号。

![](img/3755a25cf5383eb736f298464bbd6b93.png)

Wiki 说，大 O 符号是一种数学符号，它描述了当自变量趋向于某个特定值或无穷大时，函数的极限行为。听起来很吓人，对吧？简单来说，大 O 的意思是一种比较代码的方式，以找出哪个效果最好。

对于一个程序员来说，主要目标是创建速度最快、占用内存最少的代码(好吧，试着去创建)，Big O 来了！它帮助我们分析代码的性能，也允许我们正式地谈论代码(不要忘记——交流是关键！).

说到**时间复杂度**(=我们的代码有多快)**，**有人会说:好吧，为什么我们不能只使用定时器函数来测量速度呢？事情是这样的，计时器会在不同的时间记录不同的时间(甚至是相同的时间，取决于一天中的时间！)机器。还有，不够精确。

所以，最好是数**运算次数**而不是秒。让我们来看看这两个例子。

```
// write a function that calculates the sum of all numbers from 1 up to (and including) some number n.//example 1function addUpTo(n) {
  let sum = 0;
  for (let i = 1; i <= n; i++) {
    sum += i;
  }
  return sum;
}//example 2function addUpTo(n) {
  return n * (n + 1) / 2;
}
```

第二个例子很简单:我们有三个操作，不管什么是 *n.* 这不能说关于第一个例子:操作的数量随着 *n* 增长*而增长。*

因此，我们称之为…

```
function addUpTo(n) {
  return n * (n + 1) / 2;
}
```

**O(1)** 因为有静态数的运算。还有…

```
function addUpTo(n) {
  let sum = 0;
  for (let i = 1; i <= n; i++) {
    sum += i;
  }
  return sum;
}
```

被称为 **O(n)** 是因为运算次数取决于 *n* 。

此外，还有更复杂的函数叫做嵌套函数，我们可以在 O(n)操作中包含 O(n)操作。这里有一个例子:

```
function printAllPairs(n) {
  for (var i = 0; i < n; i++) {
    for (var j = 0; j < n; j++) {
      console.log(i, j);
    }
  }
}
```

我们可以看到，循环内部有一个循环，我们称之为 **O(n )** 。

从逻辑上讲，最好的可能情况是 O(1) —如果第一个选项不可行，则是 O(n)。

现在，我们来谈谈空间复杂度(=运行我们的代码需要多少额外的内存)。这里也隐含了大 O 符号。从被强烈推荐的名为 [JavaScript 算法和数据结构大师班](https://www.udemy.com/course/js-algorithms-and-data-structures-masterclass/)的 Udemy 课程中，我学到了一些有用的经验法则。

1.  大多数原语(布尔、数字、未定义、null)都是常数空间— O(1)
2.  字符串需要 O( *n* )空间，其中 *n* 是字符串长度
3.  对象和数组一般都是 O( *n* )，其中 n 是长度(对于数组)或者键的数量(对于对象)

所以，这是一个很好的开始！我们遇到过一些最常见的复杂性:O(1)，O( *n* ，O( *n*2* )

有时大 O 符号使用更难的数学表达式，但这是另一个博客的主题。

享受你的算法研究！

![](img/fab15ffd45de6d40a535ee7da4390319.png)

**来源:**

1.  [JavaScript 算法和数据结构 Masterclass](https://www.udemy.com/course/js-algorithms-and-data-structures-masterclass/)
2.  [大 O 符号](https://en.wikipedia.org/wiki/Big_O_notation)