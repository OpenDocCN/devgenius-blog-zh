# JavaScript:面向新开发人员的对象实例化模式快速参考

> 原文：<https://blog.devgenius.io/javascript-a-quick-reference-to-object-instantiation-patterns-for-new-developers-304f806a9b82?source=collection_archive---------10----------------------->

![](img/dba92944cf1a73a4bb122be8122c17b4.png)

照片由[亚历克斯·科特利亚斯基](https://unsplash.com/@frantic?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在面向对象编程( [**OOP**](https://www.webopedia.com/TERM/O/object_oriented_programming_OOP.html#:~:text=Object%2Doriented%20programming%20(OOP),applied%20to%20the%20data%20structure.) )中，有不同的模式来实例化(这意味着“创建新的实例”)对象。我喜欢把它想象成一个制造对象的工厂或系统，这些对象存储了数据并提供了功能。没有一个正确的实例化模式可以适用于所有场景。每种实例化模式都有自己的优点和缺点，这将允许您确定哪种模式会产生更好的解决方案。JS 实例化模式有:

*   功能的
*   功能共享
*   原型
*   伪古典的
*   ES6 级

JavaScript 中一个流行的约定是构造函数的第一个字母要大写。

# **功能性**

构造函数创建一个对象，向该对象添加属性和方法，并返回新创建的对象。

**好处:**写起来简单。容易理解和遵循。

**缺点:**如果要创建大量(比如说 100 万)对象。每个对象都有冗余的函数定义。内存低效。

我们怎样才能让这些实例不重复方法或属性呢？

# 功能共享

实例可以通过使用单独的对象存储属性和方法来共享方法和属性。_.[扩展](https://underscorejs.org/#extend)用共享方法扩展构造器对象以避免重复。`this`关键字用于引用创建的对象。

**好处:**提高记忆效率。删除重复的函数定义，干。

**缺点:**容易出错。如果在创建实例后修改了共享方法，新实例和旧实例将具有不同的共享方法。

如何才能避免引用不同的方法？

# 原型

Object.create() 方法创建一个新对象，并通过原型链继承传入的对象参数。如果在对象中没有找到方法，它将向上委托原型链，直到找到目标方法。

**好处:**提高记忆效率。删除重复的函数定义，干。降低出错风险。

**缺点:**这与其说是缺点，不如说是不便，因为每个构造函数都有一行用于创建和返回一个对象。

我们如何清理这些并隐藏那两行代码(上面例子中的第 2 行和第 5 行),这样我们就不用一直输入了？

# 伪古典的

当在创建实例时使用`new`关键字时，将创建并返回一个具有原型继承的对象。我猜这就是所谓的[句法糖](https://en.wikipedia.org/wiki/Syntactic_sugar#:~:text=In%20computer%20science%2C%20syntactic%20sugar,style%20that%20some%20may%20prefer.)！

**好处:**句法上比较甜。如果你删除了上面所有的评论，看看它有多简洁和“甜蜜”。

**缺点:**如果`new`关键字丢失或遗忘，可能会导致 bug！可能很难理解引擎盖下发生了什么。

另一个语法上“甜蜜”的版本怎么样？

# ES6 级

这里也必须使用`new`关键字。另外，还有其他关键字，如`class`和`constructor`。注意没有使用关键字`function`，函数是用它们的标签、括号和花括号定义的。只不过是用原型继承创建对象的语法糖。

**好处:**句法上甜。实例化的最新方法。

缺点:模糊你对正在发生的事情的理解。从技术上讲，JavaScript 没有其他语言中的“类”基于类的 OOP。JavaScript 使用原型链来模仿类。

这是黑客反应堆的第二周，我还有 34 周的时间。我想还有很多东西要学！