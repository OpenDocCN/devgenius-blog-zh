# Rust 中的智能指针

> 原文：<https://blog.devgenius.io/smart-pointers-in-rust-158046006f15?source=collection_archive---------3----------------------->

![](img/fc71e755e670991bb55f672cacf09ea2.png)

作为一个新手，你想在 Rust 中使用高阶函数，并且你几乎失去了尝试 trait object 来得到它的权利。

我想向您展示如何在 Rust 中使用智能指针的一种便捷方式，这非常方便，因为它使用了单点真模式。

因此，你想在代码中使用闭包作为函数参数，但是不知道怎么做，这就是我一直做的方式。

# 创建类型别名

```
pub type ProducerFn<R> = dyn Fn() -> R;

pub type ConsumerFn<T> = dyn Fn(T);

// or anything like this here as an example
pub type Function1<T, R> = dyn Fn(T) -> R;
pub type Function2<T1, T2, R> = dyn Fn(T1, T2) -> R; // and so on
```

这将把函数定义包装成一个类型别名。它现在不工作，我会马上解释它。

首先，我们在函数签名中使用这种类型。

```
pub fn fun<T>(producer: ProducerFn<T>) {
  let output = producer();
  ... // do something with the output
}
```

这只是…的别名

```
pub fn fun<T>(producer: dyn Fn() -> T) {
  let output = producer();
  ... // do something with the output
}
```

但是这不起作用，因为我们需要签名中的大小值，而一个 *dyn Something* 是任意大小的。

因此，我们需要在编译时将它包装成某种给定大小的结构。

最好的选择是*智能指针*。

Rust 中的智能指针可能是&…，盒形<t>，或者弧形<t>。</t></t>

所以，这样做是有效的:

```
pub type ProducerFn<R> = &dyn Fn() -> R;

pub type ConsumerFn<T> = Box<dyn Fn(T)>;

pub type Function1<T, R> = Arc<dyn Fn(T) -> R>;
```

为您使用的东西创建类型别名总是一个好习惯，比如

```
pub type Millimeters = f64; // or the like
```

所以没什么好困惑的。(f64 又是什么意思？厘米？英寸？…)

# 高阶函数的使用

(不要迷惑霍夫；) )

现在我们可以使用 Rust 中定义为闭包的高阶函数。

```
fun(|| { return 42u8 }); // as the usage of the producer || { return 42u8 }
```

但是等等，不对劲！

我们需要将它和我们的类型别名定义一起包装到一个智能指针中。

```
// So for 
pub type ProducerFn<T> = Box<dyn Fn() -> T>;

// we have to wrap it with
fun(Box::new(||{ return 42u8 }));
```

如果你经常使用生产者，这很容易出错，而且很乏味，如果你决定以后改变智能指针。

# 宏来了

只需在类型别名旁边定义宏。

```
pub type ProducerFn<R> = Box<dyn Fn() -> R>;
#[macro_export]
macro_rules! producer {
  ($e:expr) => {
    Box::new($e);
  }
}

pub type ConsumerFn<T> = Box<dyn Fn(T)>;
#[macro_export]
macro_rules! consumer {
  ($e:expr) => {
    Box::new($e);
  }
} 
```

这是一个非常简单的规则，不要害怕重复。这将会带来巨大的回报，因为如果你需要将它从框改为弧，例如，只需要在代码中的一个位置就可以很容易地做到。

你只需要在用法上保持一致。

```
fun(producer!(|| { return 42u8 }));
//or
doit(consumer!(|value| { // do sumething with value }));
```

# 只有一个补充

```
&... // single-threaded, pointing to the stack
Box... // single-threaded, pointing to the heap
Arc... // multi-threaded, pointing to the heap
```

只要明智地选择你的智能指针。

# 结论

使用这种简单的类型别名和宏定义方法，您将在开发程序时获得更安全和更快的速度。

我希望这篇文章能帮助您更好地理解如何将智能指针用于像闭包这样的特殊对象。