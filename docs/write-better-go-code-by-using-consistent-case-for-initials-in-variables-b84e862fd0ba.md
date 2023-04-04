# 通过在变量中使用一致的首字母大小写来编写更好的 Go 代码

> 原文：<https://blog.devgenius.io/write-better-go-code-by-using-consistent-case-for-initials-in-variables-b84e862fd0ba?source=collection_archive---------2----------------------->

![](img/853f681f42c600b55549feab70e21f36.png)

[布雷特·乔丹](https://unsplash.com/@brett_jordan)在 [Unsplash](https://unsplash.com/photos/M9NVqELEtHU) 上的原始照片。

如果你曾经不得不在变量名中使用首字母——我们正在看你，HTTP——那么这篇文章可能会让你感兴趣。

正如 Golang 在他们的文档中所说的，在 Golang 中，有一些约定要写得更“有效”。

Golang 的“有效的 Go”的补充是[“code review comments”](https://github.com/golang/go/wiki/CodeReviewComments#imports)，它进入了常见的陷阱和改善你的 Go 代码的技巧。

其中一个叫做“首字母”，它和我之前提到的完全一样:变量名中的首字母。

简单地说，变量或函数名或标识符中的首字母需要大小写一致。例如，这意味着`HTTP`应该留在`HTTP`而不是转向`Http`。`URL`应该是`URL`或者`url`，但不应该是`Url`。换句话说，不要把大写字母和小写字母混在一起。

“CodeReviewComments”提供了更多示例:

> 名称中的缩写词(如“URL”或“NATO”)大小写一致。例如，“url”应该显示为“Url”或“URL”(如在“urlPony”或“URLPony”中)，而决不能显示为“URL”。举个例子:ServeHttp 不是 ServeHTTP。对于具有多个初始化“单词”的标识符，使用例如“XMLHTTPRequest”或“xmlHTTPRequest”。

特别是，当你定义一个像`xmlHTTPRequest`这样的变量时，这看起来有点混乱，但是如果你坚持这个保持大小写一致的建议，那么就不会那么混乱了。`XMLHTTPRequest`，正如他们提到的，也是可以的，因为你保持初始情况不变。在这一点上，这将是一个偏好的问题。

那么，不能接受的是尝试使用`xmlHttpRequest`，按照 [camel case](https://en.wikipedia.org/wiki/Camel_case) (或 camel case)的标准，它看起来不错。

另一个经常弹出的是“用户 ID”中的“ID”。你可能已经看到有人——甚至你自己——在变量或函数中使用它作为“Id ”(比如说`UserId`),但是现在你在 CodeReviewComments 中有了一个引用，你可以在 pull request reviews 中指向它，当你建议使情况一致时，它可以支持你。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)