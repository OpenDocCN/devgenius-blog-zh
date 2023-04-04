# 重构 002 —提取方法

> 原文：<https://blog.devgenius.io/refactoring-002-extract-method-1478f421b35a?source=collection_archive---------2----------------------->

## 找到一些可以分组并被原子调用的代码片段。

![](img/42b0cc0e7425fe2ccf7b2bd55800e371.png)

图片来自 [Pixabay](https://pixabay.com/) 的 [Hreisho](https://pixabay.com/users/hreisho-2216364/)

> TL；大卫:把你的连贯句子组合在一起

# 解决的问题

*   可读性
*   复杂性
*   代码重用

# 相关代码气味

[](/code-smell-03-functions-are-too-long-accea7eb4ae9) [## 代码气味 03 —函数太长

### 人类过了 10 线就烦了。

blog.devgenius.io](/code-smell-03-functions-are-too-long-accea7eb4ae9) [](/code-smell-05-comment-abusers-feec3aeb926) [## 代码气味 05 —评论滥用者

### 代码有很多注释。注释与实现联系在一起，很难维护。

blog.devgenius.io](/code-smell-05-comment-abusers-feec3aeb926) [](/code-smell-18-static-functions-68d35c15e76) [## 代码味道 18 —静态函数

### 又一个全球接入加上懒惰。

blog.devgenius.io](/code-smell-18-static-functions-68d35c15e76) [](/code-smell-22-helpers-1d5057137ddb) [## 代码气味 22 —助手

### 你需要帮助吗？你要给谁打电话？

blog.devgenius.io](/code-smell-22-helpers-1d5057137ddb) [](/code-smell-74-empty-lines-66d8d41d83e4) [## 代码气味 74 —空行

blog.devgenius.io](/code-smell-74-empty-lines-66d8d41d83e4) [](/code-smell-78-callback-hell-aeb4a3010270) [## 代码气味 78 —回调地狱

### 将算法作为一系列嵌套回调来处理并不明智。

blog.devgenius.io](/code-smell-78-callback-hell-aeb4a3010270) [](/code-smell-102-arrow-code-3e9301e83269) [## 代码气味 102 —箭头代码

### 嵌套的 if 和 Elses 很难阅读和测试

blog.devgenius.io](/code-smell-102-arrow-code-3e9301e83269) 

# 步伐

1.将代码片段移到一个单独的新方法中

2.用对最近创建的方法的调用替换旧代码。

# 示例代码

## 以前

```
object Ingenuity {
 fun moveFollowingPerseverance() {
   //take Off
   raiseTo(10 feet)

 //move forward to perseverance
   while (distanceToPerseverance() < 5 feet){
     moveForward() 
   }

   //land
   raiseTo(0 feet)
 }
```

## 在...之后

```
object Ingenuity { 
 //1\. Move the code fragment to a separate new method 
 private fun takeOff() {
   raiseTo(10 feet)
 }

 //1\. Move the code fragment to a separate new method 
 private fun moveForwardToPerseverance() {
   while (distanceToPerseverance() < 5 feet){
     moveForward() 
   }
 }

 //1\. Move the code fragment to a separate new method 
 private fun land() {
  raiseTo(0 feet)
 }

 fun moveFollowingPerseverance() {
   takeOff()
   //2\. Replace the old code with a call to the recently created   method.
   moveForwardToPerseverance()
   //2\. Replace the old code with a call to the recently created method.
   land()
   //2\. Replace the old code with a call to the recently created method.
 }
}
```

# 类型

许多 ide 都支持这种安全重构

# 限制

如果您使用元编程反模式，就不能很好地工作

[](https://codeburst.io/laziness-i-meta-programming-34ef4abdafc6) [## 懒惰 I:元编程

### 元编程是神奇的。这是我们不应该使用它的主要原因。有许多可怕的后果…

codeburst.io](https://codeburst.io/laziness-i-meta-programming-34ef4abdafc6) 

# 标签

*   复杂性
*   可读性

# 相关重构

*   将方法移动到新的类中

本文是重构系列的一部分。