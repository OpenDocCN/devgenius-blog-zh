# 作为程序员赚钱的唯一方法

> 原文：<https://blog.devgenius.io/the-only-way-to-make-money-as-a-programmer-28b9ba56e6bd?source=collection_archive---------5----------------------->

![](img/8fe60b3d087baeaa72befb2103bd854a.png)

乔治·特罗瓦托在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

说到做程序员赚钱，我在网上找了很多建议。有人提到 10 种方法，有人提到 3 种方法，等等

但是，通过深入分析这些方法，我发现这一切都是为了解决问题。如果你解决了他们的问题，他们会给你钱的，就是这样:-)。

> 解决的问题越多，赚的就越多！

只是，你必须通过练习、坚持和耐心来发展这种惊人的技能。

我不会在理论上说太多，让我们跳到一个具体的例子或挑战，因为你是一个程序员，解决问题将是你的日常工作。

让我们考虑这个小挑战:

**你的函数有两个参数:**

1.  **当前父亲的年龄(岁)**
2.  **他儿子的当前年龄(岁)**

**с计算多少年前父亲的年龄是儿子的两倍(或者再过多少年他将是儿子的两倍)。**

顺便说一下，你可以使用任何你更习惯的编程语言。对于我的情况，我将使用 java。

最佳实践之一是使用 TDD(测试驱动开发)，这意味着我将在实现解决方案之前准备测试:

```
import org.junit.Test;
import static org.junit.Assert.assertEquals;
import org.junit.runners.JUnit4;public class TwiceAsOldTest {
    [@Test](http://twitter.com/Test)
    public void testSomething() {
      assertEquals(30, TwiceAsOld.TwiceAsOld(30,0));
      assertEquals(16, TwiceAsOld.TwiceAsOld(30,7));
      assertEquals(15, TwiceAsOld.TwiceAsOld(45,30));

    }
}
```

所以，理想的情况是，爸爸年龄等于儿子年龄乘以 2。也就是说 dadAge-2*sonAge = 0。但是，他们会遇到结果> 0 或<0.So, for this we will use absolute value.

The solution will be as below :

```
import java.lang.*;

public class TwiceAsOld{

  public static int TwiceAsOld(int dadYears, int sonYears){

     return (int)Math.abs(dadYears - sonYears * 2);

  }

}
```

If we take the first test case : 30–7 * 2 = 30 -14 = 16, which means in 16 years the dad will double the son’s age. The dad will have 46 years old and the son will have 23 years old.

***的情况，解决方案是在多次尝试和错误之后，并使用试探法得出的。***

## 结论:

通过这些挑战来训练你的思维，并为客户的问题提供技术解决方案，你的收益将会是指数级的。

在评论中与我分享你的想法:-)

# 推荐书籍

## [干净的代码:敏捷软件技术手册](https://amzn.to/3j1lNWx)

## [头脑优先设计模式:一个对大脑友好的指南](https://amzn.to/3vG0Zbl)

## [干净的建筑](https://amzn.to/3cVvkfy)

# 计算机和显示器

## [新款苹果 MacBook Pro](https://amzn.to/3wOP10M)

## [戴尔 27 英寸 Ultrasharp U2719D 显示器](https://amzn.to/3zIcT7M)

## [双臂支架桌面支架](https://amzn.to/2SNdHI3)

## [USB C Hub 多端口适配器](https://amzn.to/2VfxiS6)

## 我用于编码的 IDE:

- IntelliJ
- Vscode