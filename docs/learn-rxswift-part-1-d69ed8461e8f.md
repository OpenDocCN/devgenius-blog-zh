# 了解 RxSwift(第 1 部分)

> 原文：<https://blog.devgenius.io/learn-rxswift-part-1-d69ed8461e8f?source=collection_archive---------0----------------------->

![](img/65595261132ab59bfb96982f5dda5ca2.png)

[Rich Tervet](https://unsplash.com/@richtervet?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Rx 在这些日子里蓬勃发展，我认为如果每个开发人员都使用它会更好，所以什么是 **RxSwift？**

# RxSwift

**Rx** =反应式编程，这是一种与**数据流**和**变化传播**有关的声明式编程范例，例如，在*命令式*编程设置`**a = b+c**`中，这意味着`**a**`正在被分配`**b+c**` **、**和更高版本的结果，可以改变`**b**` 和`**c**` 的值，而对该值没有影响 但是在*反应式*编程中，每当`**b**` 或`**c**` 的值改变时，`**a**` 的值就会自动更新，而无需程序重新执行语句`**a:=b+c**` 来确定`**a**` **的当前赋值。** [维基百科](https://en.wikipedia.org/wiki/Reactive_programming)

> **react vex**结合了
> 观察者模式、迭代器模式和函数式编程的最佳理念。
> 
> **react vex**不仅仅是一个 API，它是编程的一个理念和突破。它启发了其他一些 API、框架，甚至编程语言。

# 可观察量

在 ReactiveX 中，一个观察者订阅一个可观察的。然后，观察者对可观察物发出的任何项目或项目序列做出反应。这种模式有助于并发操作，因为它不需要在等待可观察对象发射对象时阻塞，而是以观察器的形式创建一个岗哨，随时准备在可观察对象发射对象时做出适当的反应。

![](img/1c3eef1e85b00692536a5b0dbd54c152.png)

在 ReactiveX 中，许多指令可以并行执行，它们的结果稍后被捕获，你定义一种机制来检索和转换数据，以“可观察”的形式，然后*为它订阅*一个观察者，在这一点上，先前定义的机制启动，观察者站岗捕捉并响应它的发射，只要它们准备好。

这种方法的一个优点是，当您有一堆互不依赖的任务时，您可以同时开始它们，而不是等到每个任务完成后再开始下一个任务，这样，您的整个任务包只需要完成该任务包中最长的任务。

## 热点观察

一创建就发射物品，因此任何后来订阅该可观察物的观察者可以在中间的某个地方开始观察该序列。

## 冷观察

一直等到观察者订阅了它，它才开始发射物品，因此这样的观察者可以保证从头看到整个序列。

# 但是我们为什么要用 RxSwift 呢？！

RxSwift 非常强大，节省了大量开发人员的时间和精力，可用于:

**1-绑定**

```
Observable.combineLatest(firstName.rx.text, lastName.rx.text) { $0 + " " + $1 }
    .map { "Greetings, \($0)" }
    .bind(to: greetingLabel.rx.text)
```

**2 位代表**

而不是做那些乏味的、没有表达力的事情:

```
public func scrollViewDidScroll(scrollView: UIScrollView) { [weak self]
    self?.leftPositionConstraint.constant = scrollView.contentOffset.x
}
```

…写

```
self.resultsTableView
    .rx.contentOffset
    .map { $0.x }
    .bind(to: self.leftPositionConstraint.rx.constant)
```

**3- KVO**

而不是:

```
-(void)observeValueForKeyPath:(NSString *)keyPath
                     ofObject:(id)object
                       change:(NSDictionary *)change
                      context:(void *)context
```

使用`rx.observe`和`rx.observeWeakly`

这是它们的使用方法:

```
view.rx.observe(CGRect.self, "frame")
    .subscribe(onNext: { frame in
        print("Got new frame \(frame)")
    })
    .disposed(by: disposeBag)
```

或者

```
someSuspiciousViewController
    .rx.observeWeakly(Bool.self, "behavingOk")
    .subscribe(onNext: { behavingOk in
        print("Cats can purr? \(behavingOk)")
    })
    .disposed(by: disposeBag)
```

**4-通知**

不使用:

```
@available(iOS 4.0, *)
public func addObserverForName(name: String?, object obj: AnyObject?, queue: NSOperationQueue?, usingBlock block: (NSNotification) -> Void) -> NSObjectProtocol
```

…只是写

```
NotificationCenter.default
.rx.notification(NSNotification.Name.UITextViewTextDidBeginEditing, object: myTextView)
    .map {  /*do something with data*/ }
```

**5-异步自动完成搜索框**

```
searchTextField.rx.text
    .throttle(0.3, scheduler: MainScheduler.instance)
    .distinctUntilChanged()
    .flatMapLatest { query in
        API.getSearchResults(query)
            .retry(3)
            .startWith([]) // clears results on new search term
            .catchErrorJustReturn([])
    }
    .subscribe(onNext: { results in
      // bind to ui
    })
    .disposed(by: disposeBag)
```

**6-成分处理**

当你需要在单元格中下载图像时，当单元格可见时，会发送下载图像的请求，所以如果用户快速滑动，可能会有很多请求被触发和取消。如果我们可以限制并发图像操作的数量，那就更好了，RxSwift 可以轻松处理这一点。

```
let imageSubscription = imageURLs
    .throttle(0.2, scheduler: MainScheduler.instance)
    .flatMapLatest { imageURL in
        API.fetchImage(imageURL)
    }
    .observeOn(operationScheduler)
    .map { imageData in
        return decodeAndBlurImage(imageData)
    }
    .observeOn(MainScheduler.instance)
    .subscribe(onNext: { blurredImage in
        imageView.image = blurredImage
    })
    .disposed(by: reuseDisposeBag)
```

**7-聚合网络请求**

当您需要触发两个请求并在它们都完成后聚合结果时，RxSwift 可以出色地处理这一点

```
let userRequest: Observable<User> = API.getUser("me")
let friendsRequest: Observable<[Friend]> = API.getFriends("me")

Observable.zip(userRequest, friendsRequest) { user, friends in
    return (user, friends)
}
.subscribe(onNext: { user, friends in
    // bind them to the user interface
})
.disposed(by: disposeBag)
```

**8-MVVM -单向数据流**

**9-轻松整合**

# 利益

*   可组合的——因为 Rx 是组合的昵称
*   可重复使用——因为它是可组合的
*   **声明性的**——因为定义是不可变的，只有数据会改变
*   **可理解和简洁**——提高抽象层次，消除瞬态
*   **稳定** -因为 Rx 代码经过了彻底的单元测试
*   **状态更少**——因为您将应用程序建模为单向数据流
*   **无泄漏** -因为资源管理很容易

在本文中，我们讨论了什么是 RxSwift，它如何依赖于 Observer 设计模式，为什么我们应该使用它，以及它如何节省大量时间。
如何创建 RxSwift 观察值[(第二部分)](https://medium.com/@deda9/learn-rxswift-part2-3d99583417bf)

[RxSwift](https://github.com/ReactiveX/RxSwift)

[无功编程](https://en.wikipedia.org/wiki/Reactive_programming)

[react vex](http://reactivex.io/)