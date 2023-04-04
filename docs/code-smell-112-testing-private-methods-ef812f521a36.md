# 代码气味 112 —测试私有方法

> 原文：<https://blog.devgenius.io/code-smell-112-testing-private-methods-ef812f521a36?source=collection_archive---------8----------------------->

## 如果你从事单元测试，你迟早会面临这个困境

![](img/0521f40bd68581b418380734cbe52788.png)

丹·尼尔森在 [Unsplash](https://unsplash.com/s/photos/private) 上拍摄的照片

> TL；博士:不要测试你的私有方法。

# 问题

*   打破封装
*   代码复制

# 解决方法

1.如果你的方法简单，就不需要测试了。

2.如果你的方法很复杂，你需要把它转换成一个方法对象。

3.不要为了测试而公开你的方法。

4.不要[使用元编程](https://codeburst.io/laziness-i-meta-programming-34ef4abdafc6)来避免保护。

5.不要将私有计算移动到[助手](https://medium.com/dev-genius/code-smell-22-helpers-1d5057137ddb)中。

6.不要使用[静态方法](/code-smell-18-static-functions-68d35c15e76)进行计算。

# 语境

我们测试我们的类和方法。

在某些时候，我们依赖于辅助计算，我们需要以白盒的方式测试它们。

# 示例代码

## 错误的

```
<?final class Star {

 private $distanceInParsecs;

 public function timeToReachLightToUs() {
 return $this->convertDistanceInParsecsToLightYears($this->distanceInParsecs);
 }

 private function convertDistanceInParsecsToLightYears($distanceInParsecs) {
 return 3.26 * $distanceInParsecs;
 //function is using an argument which is already available.
 //since it has private access to $distanceInParsecs
 //this is another smell indicator.//We cannot test this function since it is private
 }
}
```

## 对吧

```
<?final class Star {

 private $distanceInParsecs; 

 public function timeToReachLightToUs() {
 return new ParsecsToLightYearsConverter($this->distanceInParsecs);
 }
}final class ParsecsToLightYearsConverter {
 public function convert($distanceInParsecs) {
 return 3.26 * $distanceInParsecs;
 }
}final class ParsecsToLightYearsConverterTest extends TestCase {
 public function testConvert0ParsecsReturns0LightYears() {
 $this->assertEquals(0, (new ParsecsToLightYearsConverter)->convert(0));
 }
 //we can add lots of tests and rely on this object
 //So we don’t need to test Star conversions.
 //We can yet test Star public timeToReachLightToUs()
 //This is a simplified scenario}
```

# 侦查

[X]半自动

这是一种语义气味。

我们只能在一些单元框架上发现元编程的滥用。

# 标签

*   测试气味

# 结论

有了这个指南，我们应该总是选择方法对象解决方案。

# 关系

[](/code-smell-21-anonymous-functions-abusers-e6f1e3db6a3f) [## 代码气味 21 —匿名函数滥用者

### 函数，lambdas，闭包。所以高阶，非声明，和热。

blog.devgenius.io](/code-smell-21-anonymous-functions-abusers-e6f1e3db6a3f) [](/code-smell-22-helpers-1d5057137ddb) [## 代码气味 22 —助手

### 你需要帮助吗？你要给谁打电话？

blog.devgenius.io](/code-smell-22-helpers-1d5057137ddb) [](/code-smell-18-static-functions-68d35c15e76) [## 代码味道 18 —静态函数

### 又一个全球接入加上懒惰。

blog.devgenius.io](/code-smell-18-static-functions-68d35c15e76) 

**更多信息**

- [测试私有方法指南](http://shoulditestprivatemethods.com/)

[](https://codeburst.io/laziness-i-meta-programming-34ef4abdafc6) [## 懒惰 I:元编程

### 元编程是神奇的。这是我们不应该使用它的主要原因。有许多可怕的后果…

codeburst.io](https://codeburst.io/laziness-i-meta-programming-34ef4abdafc6) 

> 正如除非需要更大的可见性，否则将所有字段设为私有是一种好的做法一样，除非需要可变，否则将所有字段设为 final 也是一种好的做法。

布莱恩·戈茨

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)