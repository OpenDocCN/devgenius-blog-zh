# PHP:在自己的类中使用“when”和“unless”

> 原文：<https://blog.devgenius.io/php-using-when-and-unless-in-your-own-classes-97035c81a0da?source=collection_archive---------1----------------------->

## 再见，街区里有新来的孩子。

![](img/b93c5039e1b31c5f1fb62d605dafbcfc.png)

由 [sergio souza](https://unsplash.com/@serjosoza?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有时，对象周围的逻辑取决于一个条件。比方说，如果一个变量是`true`，我们需要改变它的状态，否则不做任何事情`false`。

```
$transport->setPackage($package);if ($express) {
    $transport->setExpressDelivery();
}$transport->send();
```

通过在对象实例上使用`when()`和`unless()`方法帮助器，有一种*简洁的*方式来压缩这些代码行。行为看起来会是这样的:

```
$transport->setPackage($package)
    ->when($express, [$transport, 'setExpressDelivery'])
    ->send();
```

并且它应该允许使用`Closure`的更复杂的逻辑:

```
$transport->setPackage($package)
    ->unless($isSmallPackage, function ($transport) {
        $transport->setNormalDelivery();
        $transport->setBigPackageHandling();
    })
    ->send();
```

对于一些人来说，用`if`打破流程可能是标准的，也更容易理解，但是我个人喜欢可理解的、表达流畅的方法，这样其他开发人员就可以阅读并理解它。

# 何时以及除非

为了做到这一点，我们可以创建一个简单的特征，这样我们就可以在任何我们想要的东西上使用助手。这些方法将接受条件，如果它的计算结果为`true`，则调用一个`callable`。对于`unless`，我们可以只调用`when()`，但是条件会翻转:

```
<?phptrait Conditionable
{
    public function when($condition, $callable)
    {
        if ($condition) {
            $callable($this, $condition);
        } return $this;
    } public function unless($condition, $callable)
    {
       return $this->when(!$condition, $callable);
    }
}
```

而瓦拉，我们完全可以做到以上这样的事情。由于它接收了一个`callable`，我们甚至可以从其他地方调用公共静态方法，因为我们可以这样做。

```
$transport->when($express, [Courier::class, 'setExpress'])
```

当然这是*的延伸*，但是这允许有条件地调用你在代码中使用的实例的任何方法，而不是给你的对象中的每个方法添加一个条件。

例如，假设一个类有许多可条件化的方法。

```
class Foo
{
    public function whenFooDoBar() {}
    public function whenQuzDoQux() {}
    public function whenQuuzDoQuux() {}
    public function whenCorgeDoGrault() {}
}
```

不用在每个方法中编程编码条件，您可以直接使用 trait，从方法本身中保存条件逻辑。

```
class Foo
{
    use Conditionable; public function doBar() {}
    public function doQux() {}
    public function doQuux() {}
    public function doGrault() {}
}
```

你可以在我的[拉拉训练包](https://github.com/DarkGhostHunter/Laratraits)里找到这个特性和其他帮助者。如果你发现在你的项目中有用的东西，给它一个机会。

[](https://github.com/DarkGhostHunter/Laratraits) [## 黑暗幽灵之旅

### Laratraits 是一个 Laravel 包，包含有用的特征和一些类，可以和你的模型、控制器一起使用

github.com](https://github.com/DarkGhostHunter/Laratraits)