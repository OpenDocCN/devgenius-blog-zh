# 在 Golang 中，如何在字符串片段中组合(连接)由分隔符分隔的字符串

> 原文：<https://blog.devgenius.io/how-to-combine-strings-separated-by-a-separator-in-a-slice-of-strings-in-golang-dd5f1b083ea7?source=collection_archive---------5----------------------->

![](img/12363d724a73b6ceca49cc4bbf4b90fe.png)

[GolangMarket](https://gopher.golangmarket.com/products/practical-gopher-plush) 原创图片。

如果您希望将一串由某个或某些字符分隔的字符串附加在一起，那么 Golang 中有一种简单的方法可以做到这一点。

# 我们想要完成什么？

假设你有一份食物清单:披萨、意大利面、寿司、河粉和提卡马萨拉。

在 Golang 中，可以这样表示:

注意，我们在那里声明了一个 slice。

Golang 的[博客对切片描述如下](https://blog.golang.org/slices-intro):

> Go 的切片类型为处理类型化数据序列提供了一种方便有效的方法。切片类似于其他语言中的数组，但是有一些不寻常的属性。

更进一步说，切片基本上是一个更好的数组，因为它不必有固定的大小；一般来说，切片在语言的使用中更为常见。

我们希望完成的是获取该切片，然后生成一个包含所有内容的字符串值。事实上，我们希望将所有内容合并到这个新的字符串变量中，然后用逗号分隔切片中的每个字符串。

我们也许可以通过更手动的迭代方法来实现这一点，方法是遍历切片，然后用`+`操作符逐个添加字符串，然后在每个字符串后手动添加一个逗号，但是在 Golang 中，有一种更简单的方法。

# 你能做些什么来加入他们

要连接由分隔符字符串分隔的切片中的字符串，您可以这样做:

代码所做的是获取我们上面讨论过的切片声明，然后使用`strings.Join()`方法(我们可以通过导入`strings`包来访问它)。

在这个`Join()`方法中，我们将所讨论切片作为第一个参数，然后我们提供确切的字符串来分隔切片中的每个字符串。在这种情况下，我们提供了一个字符串，它有一个逗号，然后是一个空格，遵循英语中的惯例。

最后，代码使用`fmt`包的`Println()`函数来打印`strings.Join()`调用的结果。因此，该程序将打印:

```
pizza, pasta, sushi, pho, tikka masala
```

如果你想自己运行那个代码，也可以玩玩它，[然后点击这个我用上面的代码](https://play.golang.org/p/rW2fFCAZf2e)建立的 Go playground。

## 关于 Join()的更多信息

Golang 的[文档](https://golang.org/pkg/strings/#Join)对`Join()`方法解释如下:

> Join 将第一个参数的元素连接起来，创建一个字符串。分隔符字符串 sep 放在结果字符串的元素之间。

当用例出现时，将一个片段中的字符串连接在一起是非常有用的，我很感激它是可用的。希望在这篇文章之后，您也能够在您的 Go 代码中使用它。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)