# 代码味道 53——显式迭代

> 原文：<https://blog.devgenius.io/code-smell-53-explicit-iteration-d481d8a7e5d7?source=collection_archive---------4----------------------->

## 我们在学校学过循环。但是枚举器和迭代器是下一代。

![](img/8ef0ea3db06c81529113371453d74941.png)

由[埃琳娜·莫日维洛](https://unsplash.com/@miracleday)在 [Unsplash](https://unsplash.com/s/photos/jack-in-the--box) 上拍摄的照片

# 问题

*   包装
*   声明性

# 解决方法

1.  偏爱 *foreach()* 或高阶迭代器
2.  当您隐藏您的实现细节时，您将能够使用 yield()、缓存、代理、延迟加载等等。

# 示例代码

## 错误的

## 对吧

# 侦查

Linters 可以使用 regex 找到这种气味。

可能会有假阳性。请参见下面的例外情况。

# 例外

如果问题域需要元素服从自然数，如指数，第一个解决方案就足够了。

记住所有的时间去寻找真实世界的类比。

[](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) [## 唯一的软件设计原则

### 如果我们在一个单一的规则上建立我们的整个范式，我们可以保持它的简单并做出优秀的模型。

codeburst.io](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) 

# 标签

*   宣言的

# 结论

这种味道并没有给许多开发商敲响警钟，因为他们认为这是一个微妙之处。

干净的代码充满了这几个可以产生影响的声明性的东西。

# 关系

[](https://medium.com/dev-genius/code-smell-33-abbreviations-ba5149c93a68) [## 代码气味 33 —缩写

### 缩写是非常重要的，这样我们看起来聪明，节省记忆和思维空间

medium.com](https://medium.com/dev-genius/code-smell-33-abbreviations-ba5149c93a68) 

# 更多信息

[](https://codeburst.io/what-is-software-9a78c1172cf9) [## 软件有什么问题？

### 软件正在吞噬世界。我们这些工作、生活和热爱软件的人通常不会停下来思考它的…

codeburst.io](https://codeburst.io/what-is-software-9a78c1172cf9) 

> 如果你厌倦了写循环，休息一下，以后再继续。

*大卫·沃克*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

最初的 twitter 帖子

本文是 CodeSmell 系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)