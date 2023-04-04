# 代码气味 103 —双重封装

> 原文：<https://blog.devgenius.io/code-smell-103-double-encapsulation-32c5d0978988?source=collection_archive---------3----------------------->

## 调用我们自己的访问器方法似乎是一个很好的封装想法。但事实并非如此。

![](img/e433e35e72ffa30220ed47330a64810d.png)

雷·轩尼诗在 [Unsplash](https://unsplash.com/s/photos/double) 上的照片

> TL；DR:不要使用 setters 和 getters，即使是私人使用

# 问题

*   安装员
*   吸气剂
*   公开私有属性

# 解决方法

1.  拆下 setters

[](/refactoring-001-remove-setters-e5acdf60462c) [## 重构 001 —移除设置器

### Setters 违反了不变性并增加了意外耦合

blog.devgenius.io](/refactoring-001-remove-setters-e5acdf60462c) 

2.移除吸气剂

3.保护你的属性

# 语境

使用双重封装是 90 年代的标准程序。

我们想隐藏实现细节，即使是私人使用。

当太多的函数依赖于数据结构和偶然的实现时，这就隐藏了另一种味道。

例如，我们可以改变一个对象的内部表示并依赖它的外部协议。

成本/收益不值得。

# 示例代码

## 错误的

```
contract MessageContract {
 string message = “Let’s trade”;

 function getMessage() public constant returns(string) {
 return message;
 }

 function setMessage(string newMessage) public {
 message = newMessage;
 }

 function sendMessage() public constant {
 this.send(this.getMessage());
 //We can access property but make a self call instead
 }
}
```

# 对吧

```
contract MessageContract {
 string message = “Let’s trade”;

 function sendMessage() public constant {
 this.send(message);
 }
}
```

# 侦查

[X]半自动

我们可以推断 getters 和 setters，并检查它们是否是从同一个对象中调用的。

# 标签

*   包装

# 结论

双重封装是保护意外实现的时髦想法，但它暴露的不仅仅是受保护的。

# 关系

[](/code-smell-37-protected-attributes-4607260edb2d) [## 代码气味 37 —受保护的属性

### 受保护的属性对于封装和控制对我们属性的访问非常有用。他们可能在警告我们…

blog.devgenius.io](/code-smell-37-protected-attributes-4607260edb2d) [](/code-smell-28-setters-5b0e764049aa) [## 气味代码 28 —设定器

### 初级程序员做的第一个练习。ide、教程和高级开发人员不断教他们这种反模式。

blog.devgenius.io](/code-smell-28-setters-5b0e764049aa) [](/code-smell-68-getters-68571a0f7fa8) [## 代码气味 68 —吸气剂

### 得到的东西是广泛而安全的。但这是一种非常糟糕的做法。

blog.devgenius.io](/code-smell-68-getters-68571a0f7fa8) 

# 更多信息

[](https://softwareengineering.stackexchange.com/questions/181567/should-the-methods-of-a-class-call-its-own-getters-and-setters) [## 类的方法应该调用自己的 getters 和 setters 吗？

### 软件工程栈交换是一个为专业人士，学者和学生工作的问答网站

softwareengineering.stackexchange.com](https://softwareengineering.stackexchange.com/questions/181567/should-the-methods-of-a-class-call-its-own-getters-and-setters) 

[https://www . infoworld . com/article/2073723/why-getter-and-setter-methods-are-evil . html](https://www.infoworld.com/article/2073723/why-getter-and-setter-methods-are-evil.html)

> 概括不同的概念。

*埃里希·伽马*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)