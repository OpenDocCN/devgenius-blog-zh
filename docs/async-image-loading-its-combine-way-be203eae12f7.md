# 异步图像加载——这是一种组合方式

> 原文：<https://blog.devgenius.io/async-image-loading-its-combine-way-be203eae12f7?source=collection_archive---------1----------------------->

![](img/c63b0fc871086edc6d2a851e784be082.png)

苹果在 2019 年 WWDC 发布了 SwiftUI 和 Combine 框架。从那以后，它改变了编码的方式。我们知道如何从远程 URL 下载并显示图像。我相信我们已经这样做了一千次了。有些开发人员使用内置框架来完成这项任务，有些开发人员则自行实现。

我们将从远程 URL 下载一个图像，并在 SwiftUI Image 中显示该图像。但是，我们将借助 Combine framework 来处理从 URLSession dataTask 接收的数据，并在 SwiftUI 中呈现该图像。我希望你对 SwiftUI 和 Combine 框架有所了解。让我们开始吧…

# 入门指南

我们将创建一个 SwiftUI 文件，并将其命名为 AsyncWebImageView。这里我们将渲染下载的图像。

```
import SwiftUI struct AsyncWebImageView: View {
    private var url: URL
    private var placeHolder: Image init(url: URL, placeHolder: Image) {
        self.url = url
        self.placeHolder = placeHolder
    } var body: some View {
        placeHolder
            .resizable()
            .onAppear { }
            .onDisappear { }
    }}
```

现在是时候设计 *AsyncImageBinder* 类了。这个调用将负责下载和缓存图像。这里我将使用 Combine 框架。

```
import Foundation
import Combine
import UIKit// 1
class AsyncImageBinder: ObservableObject {       
    private var subscription: AnyCancellable? // 2
    @Published private(set) var image: UIImage? // 3    
    func load(url: URL) {
        //do something
    } // 4
    func cancel() {
        // do something
    }}
```

我们知道联合收割机有两个组成部分。即*发布者*和*订阅者。* 发布者发布值，订阅者接收值。

1.  这里我们使用了 *ObservableObject* 协议。这是使模型可观察的综合方法。
2.  我们使用了带有图像属性的已发布属性包装。每当我们更新图像的值时，它都会通知订阅者。
3.  *load(url:)* 方法将从远程 url 获取图像。
4.  *取消*方法会在我们不想在 UI 中渲染图像的时候取消订阅。

## 从远程 URL 获取图像

让我们在 load(url:)方法中实现图像下载代码。

引入 Combine 之后，URLSession 又增加了两个实例方法。他们是

1.  dataTaskPublisher(对于 url: URL) -> URLSession。DataTaskPublisher
2.  dataTaskPublisher(for request:URL request)-> URL session。DataTaskPublisher

```
func load(url: URL) {
    subscription = URLSession.shared
                       .dataTaskPublisher(for: url)      // 1
                       .map { UIImage(data: $0.data) }   // 2
                       .replaceError(with: nil)          // 3
                       .receive(on: DispatchQueue.main)  // 4
                       .assign(to: \.image, on: self)    // 5}func cancel() {
    subscription?.cancel()
}
```

是不是很酷！！！

我们只需要写这么多代码来从远程 URL 获取图像。让我们明白我们在做什么。

1.  dataTaskPublisher(对于 url: URL)会给我们 URLSession.DataTaskPublisher .我们知道，每个 Publisher 都有两个关联的类型。它们是输出和失败(错误)。所以 URLSession 的输出。DataTaskPublisher 有数据和响应。我们需要处理数据。所以我们在这个发布器上推出了一系列的操作。
2.  map 将尝试从数据中获取 UIImage。
3.  如果我们无法获取图像数据并以错误结束，replaceError(with:)将用 nil 替换错误。
4.  根据多线程规则，我们应该只在主线程上更新 UI。这里我们将在主线程上接收。
5.  这里我们将把接收到的值赋给图像属性。

## 在用户界面中渲染图像

现在是时候更新 AsyncWebImageView 来在这里呈现下载的图像了。

```
struct AsyncWebImageView: View {
    // ... // 1
    @ObservedObject var binder = AsyncImageBinder()
    init(url: URL, placeHolder: Image) {
        // ... // 2
        self.binder.load(url: self.url)
    } var body: some View {
        VStack {
            // 3
            if binder.image != nil {
                Image(uiImage: binder.image!)
                    .renderingMode(.original)
                    .resizable()
            } else {
                placeHolder
            }
        }
        .onAppear {  }
        .onDisappear { self.binder.cancel() }

    }}
```

让我们了解一下这是怎么回事…

1.  ObservedObject 属性包装类型订阅可观察类型对象。所以，每当我们要改变 *AsyncImageBinder 类*的 image 属性的值时， *AsyncWebImageView* 就会被通知这个改变。
2.  我们正在调用 *AsyncImageBinder 的 load(url:)方法。*
3.  这里我们将使用一个条件操作。如果 image 属性的值为零，我们将显示占位符图像。

## 让我们看看结果

现在是时候看看我们到目前为止都做了些什么。我们将在预览中看到这些变化。让我们在 *AsyncWebImageView* 中添加以下代码

```
struct AsyncWebImageView_Previews: PreviewProvider {
    static let url = URL(string: "https://image.tmdb.org/t/p/original/cDbOrc2RtIA37nLm0CzVpFLrdaG.jpg")! static var previews: some View {
        AsyncWebImageView(url: url)
    }}
```

这是我们将在预览中看到的。

![](img/d07cae0be86d80b287e0524ecc147e5c.png)

这是从预览中捕获的图像

## 缓存图像

让我们创建一个 AsyncImageCache 类。在这里，我们将 NSCache 来存储图像。

```
class AsyncImageCache {

    // 1
    static let shared = AsyncImageCache() // 2
    private var cache: NSCache = NSCache<NSString, UIImage>() // 3
    subscript(key: String) -> UIImage? {
        get { cache.object(forKey: key as NSString) }
        set(image) { image == nil ? self.cache.removeObject(forKey: (key as NSString)) : self.cache.setObject(image!, forKey: (key as NSString)) } }}
```

让我们明白我们在做什么。

1.  我们将创建单例。Singleton 将确保 AsyncImageCache 只有一个入口点。当我们在一个列表中使用多个 *AsyncWebImageView* 时，这将对我们很有帮助。
2.  我们将有一个 NSCache 实例。
3.  NSCache 是一个可变的集合，我们将数据临时存储在一个键值对中。我们知道下标为我们提供了一种从集合中访问数据的快捷方式。因此，我们将使用下标。

现在是时候更新 AsyncImageBinder 类了。

```
//...private var cache = AsyncImageCache.shared//...func load(url: URL) {
    // 1
if let image: UIImage = cache[url.absoluteString] {
        self.image = image
        return
    }
        subscription = URLSession.shared.dataTaskPublisher(for: url)
                       .map { UIImage(data: $0.data) }
                       .replaceError(with: nil)
                       .handleEvents(receiveOutput: { self.cache[url.absoluteString] = $0 })                 // 2
                       .receive(on: DispatchQueue.main)
                       .assign(to: \.image, on: self)}
```

1.  在从远程 URL 获取图像之前，我们将检查是否已将图像存储在特定 URL 的 NSCache 中。如果有，我们将从 NSCache 获取图像并更新图像属性。
2.  发布者的 handleEvents 操作帮助我们跟踪发布者的事件。在 receiveOutput 事件中，我们将图像存储在 NSCache 中。

酷…

# 结论

组合框架非常强大。它提供了一种更简单的方法来完成异步任务。

感谢阅读。希望你喜欢它。随意评论。

如果你觉得这篇文章有帮助，请鼓掌，你知道每个人可以鼓掌 50 次😉，并鼓励我进一步写更多的文章。