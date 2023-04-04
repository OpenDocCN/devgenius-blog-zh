# Java 专家喜欢看到的 5 个习惯

> 原文：<https://blog.devgenius.io/5-habits-java-experts-like-to-see-67d963dd3689?source=collection_archive---------2----------------------->

## **经验丰富的 Java 开发人员喜欢在公关中看到这些实践**

![](img/ae5dfc915edc6105bab273ee6321e27e.png)

照片由[马体·米罗什尼琴科](https://www.pexels.com/@tima-miroshnichenko?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄，来自[Pexels](https://www.pexels.com/photo/man-in-orange-blazer-using-black-tablet-computer-5439455/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)——作者编辑

大多数 Java 专家喜欢看到年轻开发人员的这些实践。

[我自己看了很多 pr](https://zivce.gumroad.com/l/become-high-quality-code-reviewer)之后，确实喜欢看接下来的习惯。你会同意，其他人也喜欢这些做法。其中大部分提高了 Java 代码的可读性和性能，还有一些减少了冗长性。

让我们看看专家们喜欢从年轻的开发人员那里看到什么实践。

# 1.尽可能使用方法引用

即便如此，大多数开发者还是创造了大 lambdas。

小型 lambdas 更容易阅读，并且[具有更好的性能](https://medium.com/javarevisited/experienced-developers-use-these-quirks-to-create-better-java-lambdas-4ae656148274)。如果可能的话，在 lambdas 中放置尽可能少的行。最好使用方法引用，如果可能的话，使用未绑定的方法引用。

你看到类似的 lambda 代码了吗？提取这些，并使用方法引用。

最好使用未绑定的方法引用，`Class::yourMethod`。由于有一个绑定的小性能打击，`Instance::yourMethod`。同样，方法引用的可读性比重复的 lambda 代码好。

# 2.你翻译异常

阅读更多关于[异常翻译](https://blog.maskalik.com/blog/2018/09/12/best-practices-for-exceptions-in-java/)的信息。

下面是[詹姆斯·高斯林对异常翻译](https://web.archive.org/web/20060407124638/http://www.artima.com/intv/solid3.html)的评论:
*这就是异常翻译能派上用场的地方。假设你有一个读取数据库的软件。在它内心深处的某个地方，它可能会得到一个* `*MalformedURLException*` *。把那个向外宣传成* `*MalformedURLException*` *是没有意义的。*

*最好想想什么样的异常是接口的一部分。而不是底层方法是如何实现的。在 Java 异常机制中，您可以将 cause 包装在异常中。于是你创建了一个新的* `*IOException*` *，你把它的起因设置为这个* `*MalformedURLException*` *。你把它们捆在一起。这非常有效。*

没有人喜欢看到这段代码:

```
throw ex;
```

使用这种方法会丢失堆栈跟踪。甚至只有`throw`也不是一条好路可走。

更好的方法是重新抛出您的自定义异常。如果你不知道如何在一个较低的层次上处理它，翻译成一个较高层次的异常。

```
throw new CustomException(ex);
```

# 3.你用枚举来表示常数

你可以在一些代码库中看到只有常量的接口。即便如此，它们也是一种反模式。

[来源](https://www.govnokod.ru/17104)

*枚举是常量的更好选择。*

*您可以向枚举添加自定义方法。*或者迭代值，或者[缓存它们以提高性能](https://richardstartin.github.io/posts/5-java-mundane-performance-tricks#dont-iterate-over-enumvalues)。并在[适当的枚举结构](https://richardstartin.github.io/posts/5-java-mundane-performance-tricks)中使用它们。

*我喜欢枚举的另一点是凝聚力。他们把相似的常数包装在一起。而在界面中，它会变得混乱。这里有一个例子:*

[来源](https://stackoverflow.com/a/14419212/5999670)

# 4.您避免了异常反模式

***我最不喜欢的就是异常吞咽和空抓。***

还有其他的，比如:

*   日志重掷
*   暴露安全细节
*   将检查的异常转换为未检查的异常。

你应该处理异常或者翻译异常。你不能两样都做。

此外，您不能转换为运行时异常。您首先需要理解检查异常和未检查异常之间的区别。

*“对可恢复条件使用检查异常，对编程错误使用运行时异常。”—约书亚·布洛赫*

# 5.需要的时候你用`varargs`

尽管在某些情况下更清洁，但这可能会造成问题。

例如当`varargs`与仿制药混合使用时。Varargs 创建了一个遭受未检查强制转换的数组。所以确保类型安全是你的工作。

```
// Joshua Bloch - Item 32 - snippet around type safety**// Mixing generics and varargs can violate type safety!**
static void dangerous(**List<String>... stringLists**) {
    List<Integer> intList = List.of(42);
    Object[] objects = stringLists;
    objects[0] = intList;             **// Heap pollution**
    String s = stringLists[0].get(0); **// ClassCastException**
}
```

当你需要变量 arity，或者传入一个参数数组时，你应该使用`varargs`。尽管它创建了一个`Object[]`,但它更简洁，可读性更好。希望在将来，这个数组会被转换成一个不可变的列表。

# 外卖食品

*枚举实际上是创建常数的工具。*

这是有原因的。它们可读性强，性能好，简洁明了。因此，年轻的开发人员没有必要忽视它们。

*此外，年轻的开发人员不会在异常上投入时间。*

我不喜欢创建例外镇，但是连一些基础知识都缺乏。正如我所提到的，专家们希望看到的是了解检查异常和未检查异常之间的区别。