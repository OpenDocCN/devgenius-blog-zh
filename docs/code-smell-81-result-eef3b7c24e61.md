# 代码气味 81 —结果

> 原文：<https://blog.devgenius.io/code-smell-81-result-eef3b7c24e61?source=collection_archive---------3----------------------->

## 结果=？？？

![](img/bdb370a008ee92c66a8ad02d8968f111.png)

KMA 拍摄的照片。 on [Unsplash](https://unsplash.com/s/photos/magician)

> TL；大卫:总是使用好名字。结果总是一个很坏的名字。

# 问题

*   可读性

# 解决方法

1.重命名*结果*。

2.如果不知道怎么命名，就用和上次函数调用一样的名字命名变量。

3.不要使用没有自动重构的 ide。

# 示例代码

## 错误的

## 对吧

# 侦查

我们必须禁止单词*结果*成为变量名。

# 标签

*   可读性

# 结论

*结果*是一个通用且无意义的名字的例子。

重构既便宜又安全。

> 离开露营地时，一定要比你发现时干净。
> 
> 当你发现地上一片狼藉时，打扫干净，谁干的并不重要。你的工作是永远把地面清洁留给下一批露营者。

# 关系

[](/code-smell-79-theresult-3c536d926f06) [## 气味代码 79 —结果

### 如果一个名字已经被使用，我们总是可以在它前面加上“the”。

blog.devgenius.io](/code-smell-79-theresult-3c536d926f06) 

# 更多信息

[](/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) [## 名字到底是什么？第二部分:康复

### 我们都同意:好名声永远是最重要的。让我们找到他们。

blog.devgenius.io](/what-exactly-is-a-name-part-ii-rehab-fc5058b3ff1) 

> 代码就像幽默。当你不得不解释它的时候，它是糟糕的。

*科里的房子*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)