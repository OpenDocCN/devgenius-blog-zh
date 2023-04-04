# 六十秒内的戈朗——命名返回值

> 原文：<https://blog.devgenius.io/golang-in-sixty-seconds-named-return-values-1f2eabaef4e2?source=collection_archive---------18----------------------->

![](img/7227b106a068969317723d92ab5efdd0.png)

由[特拉维斯·怀斯](https://www.flickr.com/people/94599716@N06)

命名返回值正是它们听起来的样子。您可以实际命名正在返回的值，而不仅仅是在您的函数上有一个返回类型。让我给你看一个简单的例子:

```
func sumValues(vals []int) (total int) {
  for _, v := range vals {
    total += v
  }
  return
}
```

好处主要是可读性。您可以清楚地知道函数应该返回什么。您可能会注意到上面代码块中的一些奇怪的东西:

1.  `total`变量似乎未定义。这是因为变量实际上是通过将它作为一个命名的返回值来创建的`nil`值(在本例中为`0`)。
2.  返回值之后什么都没有。这被称为“裸返回”，Go 知道返回变量`total`。建议您只对小函数使用裸返回，因为较长的函数会失去可读性。

我发现命名返回值对于上面的例子非常有用，在这个例子中，你不需要在函数的开始定义一个`nil`值变量。您可以在这里试试上面的例子。

更多[戈朗在六十秒](https://richard-t-bell90.medium.com/list/golang-in-sixty-seconds-7a26c5131734)

[*无限制获取介质*](https://richard-t-bell90.medium.com/membership)

[*如果你喜欢这篇文章，请给我买杯咖啡:*](https://ko-fi.com/richardtbell)