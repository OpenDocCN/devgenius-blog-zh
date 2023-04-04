# 用 AssertJ 改进你的单元测试

> 原文：<https://blog.devgenius.io/improve-your-unit-tests-with-assertj-469f56ffc411?source=collection_archive---------4----------------------->

![](img/d3902ee69b65320163167a37f0d6d569.png)

[JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

您可能正在编写单元测试，如果不是，您可能应该这样做。在我的职业生涯中，我参加了很多关于如何正确写作的研讨会和演讲。我一直强调的一点是，这些测试要有可理解的输出。没有什么比单元测试失败并显示类似`Failed. True != False`的消息更糟糕的了，你该怎么办呢？使用所有测试框架提供的标准断言函数，您可以通过使用正确的函数、提供额外的消息等来稍微改进这一点...它并不完美，有时工作量很大。

AssertJ 来了。这是一个简单的库，旨在改进您的断言。我认为这对于我的测试需求是必不可少的。它提供了各种各样的断言、最新的错误消息。此外，它还提高了代码的可读性，理解您想要断言的内容非常简单。

# 装置

AssertJ 可以在 Maven central 上获得，所以安装就像添加一个测试依赖项一样简单。

```
<dependency>
 <groupId>org.assertj</groupId>
 <artifactId>assertj-core</artifactId>
 <version>3.19.0</version>
 <scope>test</scope>
</dependency>
```

# 使用

在本文中，我将通过几个例子来说明 AssertJ 有多棒。这些示例将在 JUnit5 中完成，结构如下。

```
class DtoComparisonTest {

 @Test
 void testComparison() {
 var x = new TestedDto("a", "c", new TestedNestedDto(1, 2));
 var y = new TestedDto("a", "b", new TestedNestedDto(1, 3));

 Assertions.assertThat(x).isEqualTo(y);
 }

 @Data
 @AllArgsConstructor
 private static class TestedDto {

 String firstString;
 String secondString;
 TestedNestedDto nested;
 }

 @Data
 @AllArgsConstructor
 private static class TestedNestedDto {
 int firstInt;
 int secondInt;
 }

}
```

这个例子比较了`x`和`y`对象，并输出了下面的错误信息。

```
org.opentest4j.AssertionFailedError: 
Expecting:
 <DtoComparisonTest.TestedDto(firstString=a, secondString=c, nested=DtoComparisonTest.TestedNestedDto(firstInt=1, secondInt=2))>
to be equal to:
 <DtoComparisonTest.TestedDto(firstString=a, secondString=b, nested=DtoComparisonTest.TestedNestedDto(firstInt=1, secondInt=3))>
but was not.
Expected :DtoComparisonTest.TestedDto(firstString=a, secondString=b, nested=DtoComparisonTest.TestedNestedDto(firstInt=1, secondInt=3))
Actual :DtoComparisonTest.TestedDto(firstString=a, secondString=c, nested=DtoComparisonTest.TestedNestedDto(firstInt=1, secondInt=2))
```

多棒啊。

它有很多针对 String 的内置断言。让我们看一些例子:

```
@Test
 void testComparison() {

 var x = new TestedDto("Dragon", "Goblin", new TestedNestedDto(1, 2));

 Assertions.assertThat(x.getFirstString())
 .startsWith("D")
 .endsWith("n")
 .isLowerCase();
 }
```

这将输出

```
java.lang.AssertionError: 
Expecting <"Dragon"> to be a lowercase
```

想象一下这样做并得到与普通断言相同的输出。

它还有大量的内置收藏。

```
@Test
 void testComparison() {

 var x = new TestedDto("a", "b", new TestedNestedDto(1, 2));
 var y = new TestedDto("c", "d", new TestedNestedDto(1, 2));
 var collection = Arrays.asList(x, y);

 Assertions.assertThat(collection)
 .hasSize(2)
 .contains(x)
 .allMatch(tested -> tested.getNested().getFirstInt() == 1)
 .anyMatch(tested -> tested.getSecondString().equals("b"))
 .containsNull();
 }
```

打印此错误消息:

```
java.lang.AssertionError: 
Expecting:
 <[DtoComparisonTest.TestedDto(firstString=a, secondString=b, nested=DtoComparisonTest.TestedNestedDto(firstInt=1, secondInt=2)),
 DtoComparisonTest.TestedDto(firstString=c, secondString=d, nested=DtoComparisonTest.TestedNestedDto(firstInt=1, secondInt=2))]>
to contain a <null> element
```

我非常喜欢这个图书馆。它非常简单，可以极大地改善您的测试。我建议您今天就开始使用它(也可以深入研究他们的文档，AssertJ 还有很多内容)。

更多类似的建议，你可以在推特上关注我。

*原载于 2021 年 4 月 30 日*[*https://dev . to*](https://dev.to/pavel_polivka/improve-your-unit-tests-with-assertj-35m2)*。*