# 代码气味 90 —实现性回调事件

> 原文：<https://blog.devgenius.io/code-smell-90-implementative-callback-events-df1d2b371087?source=collection_archive---------6----------------------->

![](img/5ef22c600bf3a9548fde833538a0d7da.png)

Ashim D'Silva 在 [Unsplash](https://unsplash.com/s/photos/button-pressed) 上拍摄的照片

*在创建事件时，我们应该将触发器与动作分离。*

> TL；DR:根据发生的事情说出你的函数。

# 问题

*   [观察者模式](https://en.wikipedia.org/wiki/Observer_pattern)违反
*   连接

# 解决方法

1.  以“发生了什么”来命名事件，而不是“你应该做什么”。

# 示例代码

# 错误的

```
const Item = ({name, handlePageChange)} =>
  <li onClick={handlePageChange}>
    {name}
  </li>//handlePageChange is coupled to what you decide to do
//instead of what really happened
//
//We cannot reuse this kind of callbacks
```

# 对吧

```
const Item = ({name, onItemSelected)} =>
  <li onClick={onItemSelected}>
    {name}
  </li>//onItemSelected will be called just when a item was selected. KISS
//Parent can decide what to do (or do nothing)
//We defer the decision
```

# 侦查

这是一种语义气味。我们可以在同行代码评审中发现它。

# 标签

*   连接
*   命名

# 结论

名字很重要。我们应该将耦合名称的实现推迟到最后一刻。

# 更多信息

*   [名字里到底有什么](https://medium.com/dev-genius/what-exactly-is-a-name-part-i-the-quest-b812a4b1e0bf)

# 信用

Ashim D'Silva 在 [Unsplash](https://unsplash.com/s/photos/button-pressed) 上拍摄的照片

感谢 Maciej 的提示

> 除了基本的数学能力，优秀的程序员和伟大的程序员之间的区别是语言能力。

*玛丽莎·梅耶*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)