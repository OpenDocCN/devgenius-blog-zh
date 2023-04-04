# 如何轻松通过任何编程面试！

> 原文：<https://blog.devgenius.io/how-to-pass-programming-interview-easily-7130517a59b6?source=collection_archive---------7----------------------->

![](img/b31c0736480c1eb9885c2f24020a2ff4.png)

由 [AltumCode](https://unsplash.com/@altumcode?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在这个故事中，我将要谈论一个非常有趣的话题，那就是通过编程面试。

我在编程领域有 6 年多的经验，我通过了很多面试。

从我的经验来看，这种面试对公司来说更重要的是了解候选人是否有能力解决问题，以及她/他如何解决问题。

因此，我将提出一个挑战，我们将尝试以有效的方式解决它:-)

**挑战是:**

给你一串空格分隔的数字，你必须返回最高和最低的数字。

示例:

```
findHighAndLow("1 2 3 4 5")  *// return "5 1"*
findHighAndLow("1 2 -4 4 5") *// return "5 -4"*
findHighAndLow("1 9 3 4 -6") *// return "9 -6"*
```

注意事项:

*   **所有数字都有效** `**Int32**` **，没有*需要*验证。**
*   **输入字符串中至少有一个数字。**
*   **输出字符串必须是由一个空格分隔的两个数字，最大的数字在前。**

在大多数情况下，你会得到一个单元测试。

我更熟悉 java 编程语言，所以我打算用 JUnit 库来编写它们，但是你也可以用你选择的任何其他语言和测试库来编写它们。

```
import org.junit.Test;import static junit.framework.TestCase.fail;
import static org.junit.Assert.*;public class HighestLowestTest {
  [@Test](http://twitter.com/Test)
  public void findTest() {
    assertEquals("42 -9", HighestLowestTest.find("8 3 -5 42 -1 0 0 -9 4 7 4 -4"));
  }
}
```

因此，要解决这一挑战，我们将遵循以下步骤:

***1-获取一个字符串数组中的所有数字***

***2-我们将把这些字符串转换成数字***

3-我们将按升序对它们进行排序

***4-然后，得到最高和最低的数字，中间用空格隔开***

让我们实现这些步骤:

```
import java.util.stream.*;

public class HighestLowest{
  public static String find(String numbers) {

    String[] chrs = numbers.split(" ");

    int[] array = Stream.of(chrs)
                        .mapToInt(Integer::parseInt)
                        .sorted()
                        .toArray();

     return array[array.length - 1] + " " + array[0] ;
  }
}
```

此外，该解决方案的视频版本具有额外的功能:

如你所见，当你把问题分成小问题或挑战，然后一个一个地解决时，解决问题会容易得多。

我们实现它的第一步是使用 String 的 split 方法。

然后，我们使用流对象将数组元素从 String 转换为 int，然后对它们进行排序。最后，返回数组中最大值的最后一个元素和最小值的第一个元素的字符串，用空格分隔。

# 结论:

通过很好地理解这个挑战，并试图把它分成小的挑战并一个一个地解决它们，你将能够轻松有效地通过任何编码挑战:-)

在评论里和我分享一下你的看法:-)

感谢在 [**patreon**](https://www.patreon.com/zelakioui?fan_landing=true) 上支持我，成为我的贡献者:-)

# 推荐书籍

## [干净的代码:敏捷软件工艺手册](https://amzn.to/3j1lNWx)

## [头脑优先设计模式:一个对大脑友好的指南](https://amzn.to/3vG0Zbl)

## [洁净建筑](https://amzn.to/3cVvkfy)

# 计算机和显示器

## [新款苹果 MacBook Pro](https://amzn.to/3wOP10M)

## [戴尔 27 英寸 Ultrasharp U2719D 显示器](https://amzn.to/3zIcT7M)

## [双臂支架桌面支架](https://amzn.to/2SNdHI3)

## [USB C Hub 多端口适配器](https://amzn.to/2VfxiS6)

## 我用于编码的 IDE:

- IntelliJ
- Vscode

这些是我推荐的东西，它们是附属链接，谢谢支持我的写作:-)