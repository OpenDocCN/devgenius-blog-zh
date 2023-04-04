# 代码气味 104 —断言为真

> 原文：<https://blog.devgenius.io/code-smell-104-assert-true-ec6a9ef6dafd?source=collection_archive---------6----------------------->

## 针对布尔值进行断言会增加错误跟踪的难度。

![](img/a077d8d651641ce27e498ffef796d0d6.png)

jol de Vriend 在 [Unsplash](https://unsplash.com/s/photos/truth) 上拍摄的照片

> TL；DR:除非你正在检查一个布尔值，否则不要断言真

# 问题

*   快速失效原则

# 解决方法

1.检查布尔条件是否可以重写得更好

2.赞成主张平等

# 语境

当断言一个布尔值时，我们的测试引擎不能帮助我们太多。

他们只是告诉我们有些事情失败了。

错误跟踪变得更加困难。

# 示例代码

## 错误的

```
<?final class RangeUnitTest extends TestCase {

  function testValidOffset() {
    $range = new Range(1, 1);
    $offset = $range->offset();
    $this->assertTrue(10 == $offset);    
    //No functional essential description :(
    //Accidental description provided by tests is very bad
  }  
}// When failing Unit framework will show us
//
// 1 Test, 1 failed
// Failing asserting true matches expected false :(
// () <-- no business description :(
//
// <Click to see difference> - Two booleans
// (and a diff comparator will show us two booleans)
```

## 对吧

```
<?final class RangeUnitTest extends TestCase {

  function testValidOffset() {
    $range = new Range(1, 1);
    $offset = $range->offset();
    $this->assertEquals(10, $offset, 'All pages must have 10 as offset');    
    //Expected value should always be first argument
    //We add a functional essential description 
    //to complement accidental description provided by tests
  }  
}// When failing Unit framework will show us
//
// 1 Test, 1 failed
// Failing asserting 0 matches expected 10
// All pages must have 10 as offset <-- business description
//
// <Click to see difference> 
// (and a diff comparator will help us and it will be a great help
// for complex objects like objects or jsons)
```

# 侦查

[X]半自动

如果我们在设置这个条件后检查布尔值，一些 linters 会警告我们。

我们需要换成更具体的支票。

# 标签

*   测试气味

# 结论

尝试重写您的布尔断言，您将更快地修复故障。

# 关系

[](/code-smell-97-error-messages-without-empathy-952c320976d8) [## 代码气味 97——没有同理心的错误消息

### 我们应该特别注意用户(和我们自己)的错误描述。

blog.devgenius.io](/code-smell-97-error-messages-without-empathy-952c320976d8) [](/code-smell-07-boolean-variables-260bef4d1146) [## 代码气味 07 —布尔变量

### 使用布尔变量作为标志，暴露了意外的实现，并用 Ifs 污染了代码。

blog.devgenius.io](/code-smell-07-boolean-variables-260bef4d1146) 

# 更多信息

[](https://codeburst.io/fail-fast-3f3f036032b0) [## 快速失败

### 失败是时尚。制造比思考容易得多，失败不是耻辱，让我们把这个想法带到我们的…

codeburst.io](https://codeburst.io/fail-fast-3f3f036032b0) 

> 我终于明白了“向上兼容”是什么意思。这意味着我们可以保留所有以前的错误。

*丹尼·范·塔索*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)