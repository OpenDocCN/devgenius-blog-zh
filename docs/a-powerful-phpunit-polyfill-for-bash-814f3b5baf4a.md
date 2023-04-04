# 用于 BASH 的强大 PHPUnit 聚合填充

> 原文：<https://blog.devgenius.io/a-powerful-phpunit-polyfill-for-bash-814f3b5baf4a?source=collection_archive---------10----------------------->

![](img/dfbeaa054e51f6b12ebd1535ea19a5ea.png)

照片由[托尔加·乌尔坎](https://unsplash.com/@tolga__?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

如果您使用 PHP 进行任何开源开发，您很可能会发现需要使用多个 PHPUnit 版本。这很快就会变成一种痛苦。

你使用供应商目录吗？

供应商箱？

是 PHPUnit 7，还是可以用 PHPUnit 9？

我经常处理这个问题，因为我在 Laravel 和 WordPress 开源项目之间切换，然后是我喜欢参与的随机项目。每个人似乎都有自己的 PHPUnit 需求，我不得不不断地寻找我应该使用哪个版本。

# 解决方案

这就是为什么我创建这个 BASH 脚本来充当 PHPUnit 的 polyfill。我没有试图修改 PHPUnit，也没有只使用一个文件，而是在操作系统级别处理这项工作。

首先，我们检查 PHPUnit 是否作为依赖项安装在 vendor/bin 目录中。如果是这样，我们使用它，因为它是由项目提供的。

如果不是，我们检查供应商 bin 目录，因为有些供应商在那里提供它。

接下来，我们检查我们当前是否运行在 WordPress 目录中，以及 phpunit-7 是否如我们预期的那样安装。如果是这样，我们就用那个。

最后，我们搜索`/usr/bin`并找到已安装的 PHPUnit 的所有版本，按照相反的字母顺序排序，并选择最高的版本号。

> 注意:随着 PHPUnit 10 在 2021 年 2 月的发布，您可能希望使用零左填充来命名您的版本。

# 这有必要吗？

显然不是，但是作为专业开发人员的一个重要部分是找到改进工作流程的方法。如果您花费大量时间在需要各种 PHPUnit 版本的项目之间切换，这个脚本可以让您消除这种认知负担。

# 我需要做些什么来设置它？

首先，在 PHPUnit 上安装 phar 版本，您需要在名称为`phpunit-[major number]`的`/usr/bin`中使用，所以在我的例子中是`phpunit-7`和`phpunit-9`，因为这是我需要使用的仅有的两个。

接下来，将这个脚本复制到`/usr/bin/phpunit`并使其可执行。

就是这样。这就是需要发生的一切。它应该像 PHPUnit 一样接受所有相同的命令行参数，因为它只是将这些参数传递给定义的版本。

# 为什么这比化名好

对你来说，可能不是。事实上，我最初创建它是作为一个别名，但是因为需要，我把它移到了一个脚本中。我一直在进行一组特定的测试，我希望能够运行这个命令:

```
npm run grunt watch -- --phpunit --group=query
```

我发现 Grunt 只使用了我的别名，无法找到 phpunit 命令。这很合理，因为这不是命令。但是通过使用这个 polyfill，PHPUnit 是 Grunt 可以找到的命令。

更重要的是，它成功地识别出我们是在 WordPress 安装中，并传递到 PHPUnit 7。

您可能会发现这在其他领域会派上用场。如果有，请告诉我，我很想听听他们的故事！

如果你对这篇文章感兴趣，你会爱上我的 Twitter 上的技术意识流。 [*头那边，给我跟着*](https://twitter.com/n00bJackleCity) *。你不会失望的。*