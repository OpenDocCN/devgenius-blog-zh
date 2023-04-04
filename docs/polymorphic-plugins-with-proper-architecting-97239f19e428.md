# 具有适当架构的多态插件

> 原文：<https://blog.devgenius.io/polymorphic-plugins-with-proper-architecting-97239f19e428?source=collection_archive---------4----------------------->

![](img/5135d0a56a652ab3a0e4a4f6c1f26716.png)

由[卢卡·布拉沃](https://unsplash.com/@lucabravo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

我喜欢 DVD。我知道，现在一切都数字化了，但我还是喜欢 DVD。为什么？因为我拥有很多视频游戏系统，当我想看 DVD 时，我所要做的就是把它插入其中一个系统，然后打开它。

它*刚刚工作。*

为什么我们的插件不工作？为什么我需要下载一个不同的文件在 WordPress 上加载一个插件，而不是在 Joomla 上加载一个插件？还是 Drupal？如果我想在自己的网站上使用插件代码，但是我没有使用 CMS，该怎么办？

## “但是，特拉维斯，那是不可能的。那是完全不同的系统！”

是吗？

我不仅要说这不是不可能的，我还要证明给你看。当然，这并不容易，但是这是可行的，而且总体来说会产生更好的代码。

# 输入“你好，多莉”

对于任何使用 WordPress 的人来说，Hello Dolly 是最容易识别的插件。这主要是因为它预装了 WordPress。

客观地说，它也写得不好。当然没必要这样。插件本身只有几行，是安装最多的插件，也是卸载最多的插件。我没有数字来支持这种说法，我只是假设这是真的。

我不是来找你好多莉的茬的。马特·莫楞威格已经证明了自己是一个有能力的开发人员，所以我们不会侮辱他的技能，这不是本文的重点。

重点是:我把 Hello Dolly 改写成了多态插件。

# 现在说什么？

你可以从我的 Hello Dolly (Redux)插件中取出 zip 文件，直接上传到 WordPress 和 Joomla！它会立即生效。开箱即用。Joomla 有一些设置步骤，但这只是 Joomla 的工作方式。

首先，我们来看看原来的 Hello Dolly 插件。

简单，100 行代码，3 个函数。我们将讨论一些问题，但是这个插件非常适合它的本来面目:一个预装的 WordPress 插件。

# 我们来分解一下

你在这个插件中注意到的第一件事是什么？对我来说，是 Hello Dolly 插件和 WordPress 核心之间的强耦合。这个插件立即知道它是一个 WordPress 插件，因为要运行它必须访问`add_action`函数。

事实上，有四种不同的 WordPress 核心功能，add_action 是其中侵入性最小的。你能找到他们吗？在进一步阅读之前先看一看。

你找到他们了吗？

如果没有，别担心，我会把它们列在这里:

1.  添加 _ 操作
2.  wptexturize
3.  获取 _ 用户 _ 区域设置
4.  __

第四种可能是最难找到的。我第一次看代码的时候就错过了。

# 让我们退后一秒钟

什么甚至是*是一个多态插件？好吧，如果你之前没有收集它，我将在这里清楚地定义它:多态性是一个事物采取许多形式的能力。在面向对象编程中，我们用它来描述能够被当作其他类对待的类。*

*在这个例子中，我使用多态来描述一个插件如何被多个内容管理系统使用。在这种情况下，它不是真正的多态性，因为必须添加一些方法才能使它工作，但是，我喜欢这个名字。*

*告我吧。*

# *回到重构！*

*我们需要做的第一件事是清理这些代码。首先，我们将函数转换成一个类，并给这个类命名空间 HelloDolly。*

*接下来，真的没有必要把多行字符串转换成数组，只是为了得到一个随机的行。因此，我们将该文本块转换为一个数组，删除转换行，并使用`array_rand`更新随机选择。*

*这仍然给我们留下了所有内置的 WordPress 核心功能，现在已经坏了。*

*我们要做的是使用[依赖注入](https://en.wikipedia.org/wiki/Dependency_injection)来传递两个[匿名函数](https://www.php.net/manual/en/functions.anonymous.php)，它们将接受相同的参数，并运行 WordPress 核心函数。*

*所有这些函数都被转移到一个`wp-loader.php`文件中，包括插件头注释，告诉 WordPress 哪个文件是插件的入口点。*

*完成后，我们的 hello.php 文件将如下所示:*

**注:现在来看，我真的应该用一个* [*闭包*](https://www.php.net/manual/en/class.closure.php) *来代替两个匿名函数。使用闭包可以避免将匿名函数传递给局部变量来调用它们。**

*既然我们已经将核心的 WordPress 方法分离出来，它们需要放在某个地方。那就是`wp-loader.php`，看起来是这样的:*

*首先，我们检查以确保我们正在 WordPress 内部运行。常数`WPINC`总是在 WordPress 中定义，所以这是一个很好的代理。*

*在那里，我们创建我们的匿名函数，这些函数本质上是 WordPress 核心函数的代理，并将它们传递给新的 HelloDolly 类。*

*从那里，我们将 Hello Dolly 类作为参数传递给一个新的 WordPress DisplayDriver 类，并调用 register 方法。*

*该类如下所示:*

**注意:为了保持代码尽可能的独立，我避免使用 Composer 或自动加载。你可以很容易地添加一个自动加载程序，但是，摆脱所需的调用。**

*我并不真的需要在这里构建`printProxy`方法，但是我想展示如何使用代理方法来调用需要更多参数的方法。*

*这两个插件在功能上是相同的。所以何必呢？*

*嗯，这就是我们使插件多态的地方。*

# *准备 Joomla！*

*在这个项目之前，我甚至从未安装过 Joomla，更不用说为它编写模块了。这就是为什么我花了整整 15 分钟的开发时间来编写这个 Joomla 模块。*

*对于 Joomla 模块，我们需要三样东西:*

1.  *模块 XML 配置文件*
2.  *模块入口文件*
3.  *模板目录/文件*

*我不确定最后一个是不是必须的，但是在 Joomla 网站的教程里用过，所以我在这里用了一个。*

*首先，XML 文件。*

*我不会试图解释每一项的确切含义，但我会指出文件块。这些文件中的每一个都需要准确地命名它在那里的位置。*

*我们已经讨论了我们的核心插件代码，这里没有改变，所以让我们看看 mod_hellodolly.php 文件。*

*这是我们的 Joomla！相当于 wp-loader.php 文件。*

*幸运的是，Joomla 有一个常量用来定义模块何时在 Joomla 内部直接运行。所以我们在这里使用它，其他的看起来几乎一样。*

*唯一的区别是第 16 行，这里我们使用`JModuleHelper::getLayoutPath`来加载默认布局到我们的`tmpl`文件夹中。*

*那个文件在这里*真的*没有必要。我们可以很容易地直接回显代码，但我想模拟一个完整的模块。*

*如你所见，这里没什么事情。只需打印 CSS 和格式化的歌词，就大功告成了。*

# *我们结束了*

*就是这样。这就是你创建一个插件所需要的一切，这个插件将在 Joomla 中安装这两个插件！还有 WordPress，直接。*

*在 Joomla 上，如果您在安装后查看安装目录，您会注意到只有您的 XML 中列出的文件被实际保存到磁盘上。这意味着你可以上传带有整个 WordPress 插件的 Zip 文件，它将只安装 Joomla 代码所需的文件。*

*WordPress 会安装每个文件，但是不会使用 Joomla 部分。*

*尝试一下，看看我的示例库，压缩内容，上传到你的 WordPress 和 Joomla！网站。*

*[](https://github.com/anubisthejackle/hello-dolly) [## anubisthejackle/hello-dolly

### 在我的中篇博文 GitHub 中，一个重构版的 Hello Dolly 是超过 5000 万开发者的家园…

github.com](https://github.com/anubisthejackle/hello-dolly) 

当你完成后，继续创建多态插件。

然后[在推特上给我打电话](https://twitter.com/n00bJackleCity)，这样我就可以查看他们了。* 

*如果你对这篇文章感兴趣，你会爱上我的 Twitter 上的技术意识流。 [*头那边，给我跟着*](https://twitter.com/n00bJackleCity) *。你不会失望的。**