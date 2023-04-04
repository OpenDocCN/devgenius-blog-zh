# 代码气味 04 —字符串滥用者

> 原文：<https://blog.devgenius.io/code-smell-04-string-abusers-ec67005d9d5f?source=collection_archive---------6----------------------->

太多的解析、分解、正则表达式、strcomp、strpos 和字符串操作函数。

![](img/9776fc482e4dc08c312568bea225234f.png)

纳撒尼尔·舒曼在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 问题

*   复杂性
*   可读性
*   可维护性
*   缺乏抽象

# 解决方法

1)改为使用对象。

2)用处理对象关系的数据结构替换字符串。

3)回到 Perl:)

4)找出实物和琴弦之间的[双射](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)问题。

# 例子

*   序列化程序
*   分析器

# 示例代码

## 错误的

## 对吧

# 侦查

自动检测并不容易。如果使用了太多的字符串函数，就会发出警告。

# 关系

*   原始痴迷

# 标签

*   绘图

本文是 CodeSmell 系列的一部分。

[](https://mcsee.medium.com/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

mcsee.medium.com](https://mcsee.medium.com/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)