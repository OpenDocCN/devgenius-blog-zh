# 拉勒维尔:添加那些你一直想要的缺失的帮手

> 原文：<https://blog.devgenius.io/laravel-adding-those-missing-helpers-you-always-wanted-acaea8cbddce?source=collection_archive---------7----------------------->

## 更喜欢有表现力的代码，而不是那些冗长的代码行

![](img/91a52cdc251e9d393a1b348a91bbc4d9.png)

马修·韦林在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我喜欢任何 PHP 项目的一点是拥有全局助手。你知道，那些你可以在任何地方调用的函数，可以把你的许多行或者冗长的代码删除或者保存成一行，或者两行，同时在一个地方分配任何逻辑。

```
$expected = from($this)->do('foo');
```

Laravel 本身的问题是，有时它所包含的助手是不够的。包括的大多是快速访问服务(比如`cache()`)或者工厂(比如`response()`)，还有一些帮助你有预期结果的(比如`data_get`)。

例如，假设我们希望一个函数被多次调用，但在执行之间休眠，就像我们需要避免外部 API 的速率限制。如果没有助手，我们将不得不创建一个类，并将逻辑放在一个公共静态方法中，并希望记住它的位置。

```
class Logics
{
    public static function logic_sleep($times, $sleep, $callback)
    {
        // Run and sleep between calls.
    }
}Logics::sleep(4, 10, function() {
    // ...
});
```

使用这种技术使你的全局不那么全局。因为这是我项目中需要的许多东西之一，所以我决定创建一个包含更多全局助手的包:

[拉拉 help](https://github.com/DarkGhostHunter/Larahelp)

# 那些你一直想要的帮手

至少对我来说，全局助手的主要思想是拥有一段提供简单性、可读性和灵活性的代码。把它们想象成你可以放在口袋里的小瑞士刀。

比如上面可以成为它自己的全局函数，我们可以随便在哪里调用。

```
public function handle()
{
    logic_sleep(10, 5, function () {
        $this->uploadRecords();
    });
}
```

助手操作起来很简单，但我们除非深入挖掘源代码，否则不会知道它在幕后到底做了什么，这是公平的。在任何情况下，拥有一个或两个单词的全局函数会使整个代码更具可读性。而且既然是自己的帮手，我们想怎么叫都行。

## 如何添加自己的助手

但这只是我决定为我经常使用的东西创建的众多助手之一。

要在项目中添加全局助手，只需在项目中的任意位置(最好在 PSR-4 根文件夹中)添加一个带有全局函数的 PHP 文件，并告诉 Composer 加载它。

你可以自由添加你想要的文件数量。我决定把它们分成*类*类，就像我对我的包所做的那样，以避免有一整墙的函数文本。

```
"autoload": {
    "psr-4": {            
        "App\\": "app"
    },
    "files": [
        "app/Helpers/datetime.php",
        "app/Helpers/filesystem.php",
        "app/Helpers/http.php",
        "app/Helpers/objects.php",
        "app/Helpers/services.php"
    ]
},
```

我也乐于接受建议，所以如果你认为它对你有用，那就试试吧:

[](https://github.com/DarkGhostHunter/Larahelp) [## darghosthunter/Lara help

### 使用超过 35 个有用的全球助手来增强您的 Laravel 项目。您可以通过 composer 安装软件包…

github.com](https://github.com/DarkGhostHunter/Larahelp)