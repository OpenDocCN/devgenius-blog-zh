# 3 分钟内反转任意单词！

> 原文：<https://blog.devgenius.io/reverse-any-word-in-3-minutes-47ca0ec3c175?source=collection_archive---------3----------------------->

![](img/b15bb4c0a1f39d309dacc79f526e6b00.png)

约翰·施诺布里奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我认为精通编程或任何其他领域的最好方法是通过解决问题。

在这个故事中，我们将使用 Java 编程语言解决一个新的挑战，这个解决方案也适用于任何其他编程语言。

挑战在于:

> 写一个函数，它反转传入的字符串中的所有单词。

示例:

```
"The best way to master programming is by solving problems " --> "problems solving by is programming master to way best the"
```

正如鲍勃叔叔(鲍伯·马丁)所说:

> “走得快唯一的方法，就是走得好”。

因此，出于这个目的，我们将在跳到解决方案之前创建单元测试。

```
import org.junit.Test;
import static org.junit.Assert.assertEquals;
import org.junit.runners.JUnit4;public class ReverseWordsTest {
    [@Test](http://twitter.com/Test)
    public void reverseTest() {
         assertEquals("drinking like I",ReverseWords.reverse("I like drinking"));assertEquals(
"running like I", 
ReverseWords.reverse("I like running")
);  
}
```

要解决这个问题，您应该遵循以下步骤:

***1-将字符串转换为字符串列表***

***2-颠倒列表顺序***

***3-然后连接列表元素得到一个反转的字符串***

在完成这些步骤后，你会得到下面用 Java 编程语言写的结果。

```
import java.util.*;

public class ReverseWords{

 public static String reverse(String words){

    List<String> wordsAsList = Arrays.asList( words.split(" ") );
    Collections.reverse( wordsAsList );

    return String.join( " ", wordsAsList );

 }

}
```

# 结论:

从这个挑战中，你可能学到了很多关于 java 的东西。

就像使用 string 方法 split 将字符串拆分成数组一样。

```
Collections.reverse to reverse a collectionand String.join to concat the elements and return the string
```

所以，这里的主要思想是专注于解决问题，然后你会自动掌握工具，如编程语言，框架，…

这个故事的视频版本:

在评论里和我分享一下你的看法:-)

# 推荐书籍

## [干净的代码:敏捷软件技术手册](https://amzn.to/3j1lNWx)

## [头脑优先设计模式:对大脑友好的指南](https://amzn.to/3vG0Zbl)

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