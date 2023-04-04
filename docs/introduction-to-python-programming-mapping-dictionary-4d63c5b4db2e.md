# Python 编程教程:映射-字典

> 原文：<https://blog.devgenius.io/introduction-to-python-programming-mapping-dictionary-4d63c5b4db2e?source=collection_archive---------2----------------------->

![](img/35ecb00f2fddb4b2bce588ca6231c910.png)

摄影:约书亚·赫内谈 unsplash.com

编程中的映射是索引值的一种灵活方法。Python 使用字典作为映射手段。

> 这篇文章是我的 [Python 编程教程](https://arc-sosangyo.medium.com/list/introduction-to-python-programming-80e79264dcad)系列的一部分。

# Python 词典

字典是一种包含无序的键值对集合的数据结构。

例如

```
revealName = {
  "Batman": "Bruce",
  "Superman": "Clark",
  "Wonder Woman": "Diana",
  "Green Lantern": "Hal",
  "Flash": "Barry",
  "Aquaman": "Arthur",
  "Martian Manhunter": "Jonn"
}print(revealName)
```

输出将是:

```
{'Batman': 'Bruce', 'Superman': 'Clark', 'Wonder Woman': 'Diana', 'Green Lantern': 'Hal', 'Flash': 'Barry', 'Aquaman': 'Arthur', 'Martian Manhunter': 'Jonn'}
```

> **注意:python 中的** [字符串](https://arc-sosangyo.medium.com/introduction-to-python-programming-variables-and-data-types-ef29710fad9e)接受单引号 **(')** 或者双引号**("**)。不管你在字符串中使用什么，它都是一样的。

# 提取值

与列表中的[序列相比，字典中的值是无序的。这意味着您不能像从列表中提取数据那样使用索引。字典中的数据是通过声明其对应的键值对来提取的。](https://arc-sosangyo.medium.com/introduction-to-python-programming-sequence-list-tuples-and-range-1caa03ffd55c)

例如

```
revealName = {
  "Batman": "Bruce",
  "Superman": "Clark",
  "Wonder Woman": "Diana",
  "Green Lantern": "Hal",
  "Flash": "Barry",
  "Aquaman": "Arthur",
  "Martian Manhunter": "Jonn"
}print(revealName["Superman"])
```

输出将是:

```
Clark
```

# 添加值

通过执行下面的代码，可以为字典增加更多的价值。

例如

```
revealName = {
  "Batman": "Bruce",
  "Superman": "Clark",
  "Wonder Woman": "Diana",
  "Green Lantern": "Hal",
  "Flash": "Barry",
  "Aquaman": "Arthur",
  "Martian Manhunter": "Jonn"
}revealName["Green Arrow"] = "Oliver"print(revealName)
```

绿色箭头现已添加到词典中。

输出将是:

```
{'Batman': 'Bruce', 'Superman': 'Clark', 'Wonder Woman': 'Diana', 'Green Lantern': 'Hal', 'Flash': 'Barry', 'Aquaman': 'Arthur', 'Martian Manhunter': 'Jonn', 'Green Arrow': 'Oliver'}
```

# 更新值

也可以通过执行相同的 add value 语法来编辑现有值。

例如

```
revealName = {
  "Batman": "Bruce",
  "Superman": "Clark",
  "Wonder Woman": "Diana",
  "Green Lantern": "Hal",
  "Flash": "Barry",
  "Aquaman": "Arthur",
  "Martian Manhunter": "Jonn"
}revealName["Flash"] = "Wally"print(revealName["Flash"])
```

输出将是:

```
Wally
```

闪光灯从巴里换成了沃利。

# 移除值

Python 具有 **pop()** 功能，用于删除字典中具有指定键值的条目。

例如

```
revealName = {
  "Batman": "Bruce",
  "Superman": "Clark",
  "Wonder Woman": "Diana",
  "Green Lantern": "Hal",
  "Flash": "Barry",
  "Aquaman": "Arthur",
  "Martian Manhunter": "Jonn"
}revealName.pop("Batman")print(revealName)
```

输出将是:

```
{'Superman': 'Clark', 'Wonder Woman': 'Diana', 'Green Lantern': 'Hal', 'Flash': 'Barry', 'Aquaman': 'Arthur', 'Martian Manhunter': 'Jonn'}
```

请注意，蝙蝠侠已经不在字典值中了。

在我们的下一个教程中，我们将通过处理关于 python [注释](https://arc-sosangyo.medium.com/python-programming-tutorial-comments-cb1c2497b3c3)来稍微放慢速度。

愿法典与你同在，

*   弧

*更多内容请看*[*blog . dev genius . io*](http://blog.devgenius.io)*。*