# 停止在 PHP 中使用“静态”

> 原文：<https://blog.devgenius.io/stop-using-static-in-php-b150527819b2?source=collection_archive---------2----------------------->

## 让我试着说服你为什么共享状态是错误的，以及如何避免它以成为一个更好的开发者

![](img/83ddee61b4aeacdd136f485d89ffc3ad.png)

安瓦尔·阿里在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在我之前的文章中，我试图说服您停止在 PHP 中使用“extends”关键字。今天我有一些关于使用“静态”时共享状态的提示给你。**记住** : *仅仅因为你能，并不意味着你应该。*

**TL；没有优点，只有缺点。正确设计的代码不需要任何静态方法或静态属性。静态闭包和静态工厂方法非常好。**

[](/stop-using-extends-in-php-37c9da1cce83) [## 停止在 PHP 中使用“扩展”

### 你在你的域代码中使用抽象类还是扩展类？希望从今天起，你会停止这样做…

blog.devgenius.io](/stop-using-extends-in-php-37c9da1cce83) 

当参考**静态**时，在写入时有三种可能性。我们将逐一讨论这些问题，并讨论为什么经常使用它们以及如何避免它们。

# 静态类方法

静态类方法主要用于轻松地从一个类调用另一个类。一开始这样做可能很诱人，但最终会导致不稳定的决策，比如从用户对象发送电子邮件。最后的结果总是一样:一塌糊涂。

![](img/22ca1e37303c81950b8a6cdafc072f05.png)

拉尔夫(拉维)凯登在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

**缺点**:

*   助长了跨越不应该混合的层之间的边界的坏习惯(领域层中的基础设施层)
*   单元测试是困难或乏味的(依赖性不容易被模仿)

拥抱依赖注入，而不是使用静态方法。实际上，每个现代框架都提供了开箱即用的依赖注入特性。从方法中移除 static 关键字，并在构造函数中注入该类。

# 静态类属性

静态类属性主要用于应用内存化模式来应用微优化。这种简单的缓存技术可以通过将频繁访问的数据保存在实现附近来提高性能。这种方法在 PHP 中不会适得其反，因为该进程只在 HTTP 请求期间是活动的，然后内容被快速释放。

它们还被用于应用单例模式，这是业内流传的最糟糕的模式之一，原因如上所述*静态类方法*。

当我们开始将代码转移到像 cron 命令、队列工作器这样的长时间运行的进程时，或者当切换到像 [Swoole](https://www.php.net/manual/en/intro.swoole.php) 这样的异步 PHP 时，事情就变得复杂了。如果数据没有被正确释放，你可能会遇到内存泄漏，内存使用增加，应用程序死于内存不足的错误。

**缺点**:

*   如果不小心处理，可能会导致代码泄漏(在长时间运行流程的情况下)
*   单元测试是乏味的(程序员必须在运行之间清除静态属性的内容，以避免由于测试中的数据重叠而导致的错误)
*   在并行处理的情况下，缓存用于每个进程，这不必要地增加了 RAM 的使用

相反，您应该使用框架的依赖注入特性注入一个缓存适配器，并确保为每个项目设置生存时间，以允许缓存最终被清除。

# 静态变量

我从未使用过它们，但我见过它们被用来缓存数据或通过保持可重用的服务(如 HTTP 客户端)来减少内存使用。

使用静态变量的缺点和静态类属性完全一样，同样没有优点。

您应该在构造函数中注入依赖项，而不是保留静态属性。

# 静态代码的有效方案

静态代码有一些完美的场景。这些包括:

## 不可变闭包

```
$userList = $collection->map(static function(User $user): array {
    return [
        'id'       => $user->getId(),
        'username' => $user->getUsername(),
    ];
});
```

## 不可变工厂方法

```
final class UserNotFoundException extends \DomainException
{
    public static function byId(int $id): self {
        return new self(\sprintf('User by id %d not found', $id));
    }
}
```

## 私有不可变方法

```
public function getPrintableName(): string
{
    return \sprintf(
        '%s: %s',
        self::cleanEmoji($this->title),
        self::cleanEmoji($this->subtitle),
    );
}private static function cleanEmoji(string $input): string
{
    // do the work
}
```

一旦脱离了共享状态，在云中部署代码就容易多了。避免静态方法会促进良好的习惯，并使您的代码不那么混乱，更容易测试。

在我专业工作中必须维护的成千上万行代码中，我找不到使用静态代码的理由，你也没有理由这么做。问问你自己，你准备好成为一名更好的开发者了吗？从今天起停止使用静态代码，拥抱依赖注入和不变性。**你以后会感谢我的。**

有疑问吗？想法？请给我留下你的看法。让我们一起让我们的代码变得美丽。订阅我的出版物，获取更多类似的文章，成为更好的开发人员。

这个故事对你有价值吗？请留言支持我的工作👏鼓掌表示感谢你知道你可以不止一次鼓掌吗？🥰 *谢谢你。*