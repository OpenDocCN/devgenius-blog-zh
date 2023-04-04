# iOS —通过图像缓存支持异步加载远程图像。

> 原文：<https://blog.devgenius.io/ios-loading-remote-image-asynchronously-with-image-cache-support-5c7df4a15708?source=collection_archive---------5----------------------->

![](img/52149281806b36e4c7c3589e1d77f68b.png)

[Yura Fresh](https://unsplash.com/@mr_fresh?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

UIImageView 用于显示 iOS 平台中的图像。
加载静态图像是一项简单的任务。但是如果我们需要使用 URL 从远程服务器加载图像呢？

我们必须使用给定的 url 下载图像，将数据转换为 UIImage 对象，然后将其分配给 UIImageView。

让我们看看我们能做些什么。

这个块将下载 url 内容并将数据转换成 UIImage 对象。在 success 块中，您可以自由地使用 image 对象在 UIImageView 中显示它。

现在，每当我们需要设置一个图像时，我们将不得不使用这个块来下载图像。

**如果我们缓存图像并重用它会怎么样。**

因此，在 UIImageView 扩展中创建一个函数，实现一个以图像 URL 为参数的函数，将下载的图像存储到 NSCache 对象中，并将图像设置为图像视图。

为了显示图像下载状态，我们可以添加一个活动指示器，并相应地制作动画。

**使用 NSCache 的优势:**

我们不必多次下载相同的图像。

NSCache 是线程安全的，可以从任何线程调用。

iOS 将在内存警告期间清除缓存数据。