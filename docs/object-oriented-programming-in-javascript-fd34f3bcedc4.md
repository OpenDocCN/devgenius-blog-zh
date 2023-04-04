# JavaScript 中的面向对象编程

> 原文：<https://blog.devgenius.io/object-oriented-programming-in-javascript-fd34f3bcedc4?source=collection_archive---------4----------------------->

对象，它们为什么重要？

![](img/2c1aec7a2b561540a9bba9fcdfa0b4fd.png)

照片由[格雷格·拉科齐](https://unsplash.com/@grakozy?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 什么是对象？

Ruby 编程语言通常被认为是一切都是对象的语言。

```
2.6.1 :001 > **Class**.ancestors=> [**Class**, **Module**, **Object**, **Kernel**, **BasicObject**]
```

如果你看这段从终端上截取的代码片段，我们正在调用`.ancestors`来跟踪`Class`的继承。这向我们展示了父树，让我们看到，事实上，所有的 Ruby 实际上都是一个对象，你可以一直追溯到 BasicObject。JavaScript 是相似的，因为 JS 中几乎所有的东西都是对象。你可以花几个小时来辩论这一点的利弊，或者讨论 JS 创建时没有预料到它会像今天这样受欢迎，所以他们把一切都变成了对象。

```
> typeof{Function}'object'
```

但是我跑题了，JS 实际上是所有的对象，那么到底什么是对象呢？对象是包含数据和代码的方法，这些数据和代码可用于在我们的代码中表示真实世界的事物(对象)。在面向对象编程(OOP)中，程序员问自己如何最好地表示这个对象？是什么让这个物体独一无二，你怎样才能最好的识别它？

例如，如何用代码表示一只狗？我们必须考虑是什么让狗成为狗吗？作为一只狗有什么特征？我们知道狗可以有名字，它可以通过叫来说话，它可以摇尾巴。为了更好地表达这一点，我们使用类和构造函数在程序创建狗时给它添加这些属性。

```
// assume a constructor was created to give a dog instance these methods and attributes //let sparky = new Dog("Sparky", "woof")
sparky.name = "Sparky"
sparky.speak = "Woof"
```

在 JS OOP 中，你可以使用“点符号”来调用对象实例的方法。所以`object.method()`在我们的 sparky 例子中，`sparky.name(“Sparky”)`我们使用这个奇特的点符号来调用 sparky 上的 name 方法，sparky 是 Dog 类的一个实例。在括号中，我们将“Sparky”的值指定为名称。

OOP 中的编码会变得非常元和哲学，这与我的历史背景很合拍。

## 什么是句法糖？

ES6 向 JavaScript 编程领域引入了一个熟悉的(如果你懂其他语言的话)概念，即类的引入。如前所述，在 Java、Ruby、C#和 PHP 等其他编程语言中，类也很常见。当你写这些类的时候，你是在模仿这些语言，用一种新的方式来格式化你的代码。在幕后，JavaScript 类仍然是那些相同的原型、继承和 ES5 语法。因此命名为 syntax sugar，这是一种方便的语法，可以让您编写其他语言所熟悉的语法！

## 我的使用 OOP 的 JS 项目

这是我在 JS 项目中使用的 Post 类的一个例子。

```
class Post { static allPosts = [] constructor(post){
    this.id = post.id
    this.content = post.attributes.content
    this.comments = post.attributes.comments
    Post.allPosts.push(this)
  }
}
```

我通过使用`class Post {}`来声明 post 类。一旦我的 Post 实例启动，我们立即调用`constructor(){}`方法来为 Post 分配属性。

## 这个？

在上面的代码片段中，您可能已经注意到了`this.id = post.id`。这是什么？这是一个非常元的主题，但是`this`关键字指的是它所属的对象。这可能有多种含义和关联，但这正是 JavaScript 中上下文非常重要的地方。所以在`this.id = post.id`中，`this`关键字指的是 Post 类对象，并给它赋值。这可能会变得非常模糊不清，但是请记住使用`this`的上下文。