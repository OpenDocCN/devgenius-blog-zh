# 代码气味 91 —没有描述的测试断言

> 原文：<https://blog.devgenius.io/code-smell-91-test-asserts-without-description-fcf1ea2bcbf9?source=collection_archive---------4----------------------->

## 我们是 xUnit 的忠实粉丝。但是我们不太关心程序员。

![](img/85e502234dd680ef3f481a6c6a316c47.png)

[Startaê Team](https://unsplash.com/@startaeteam) 在 [Unsplash](https://unsplash.com/s/photos/dialogue) 上的照片

> *TL；DR:使用带有声明性描述的断言。*

# 问题

*   可读性
*   硬调试
*   浪费时间

# 解决方法

1.  放一个漂亮的描述性断言
2.  分享解决问题的指南

# 示例代码

## 错误的

```
<?public function testNoNewStarsAppeared(): void
  {
     $expectedStars = $this->historicStarsOnFrame();
     $observedStars = $this->starsFromObservation();
     //These sentences get a very large collection $this->assertEquals($expectedStars, $observedStars);
     //If something fails we will have a very hard debugging time
    }
```

## 对吧

```
<?public function testNoNewStarsAppeared(): void
  {
     $expectedStars = $this->historicStarsOnFrame();
     $observedStars = $this->starsFromObservation();
     //These sentences get a very large collection $newStars = array_diff($expectedStars, $observedStars); $this->assertEquals($expectedStars, $observedStars ,
         'There are new stars ' . print_r($newStars,true));
     //Now we can see EXACTLY why the assertion failed with a clear and
     //Declarative Message 
    }
```

# 侦查

由于*断言*和*断言描述*是不同的函数，我们可以调整策略以支持后者。

# 标签

*   测试气味

# 结论

尊重你的主张的读者。

甚至可能是你自己！

# 更多信息

*   [XUnit:Assert Description Deprecation](https://github.com/xunit/xunit/issues/350)

> 总是这样编码，就好像最终维护你的代码的人会是一个知道你住在哪里的暴力精神病患者。

约翰·伍兹

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)