# 地形:使用计数条件时保持干燥

> 原文：<https://blog.devgenius.io/terraform-staying-dry-when-using-count-conditionals-2c4f6c15a4ac?source=collection_archive---------3----------------------->

![](img/9b1f86f302ab3e94eccd2790f1f2bc2c.png)

照片由[萨纳·赛迪](https://unsplash.com/@sanasaidi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

技术领域的每个人都听说过“保持干燥”这个词或它的一些变体。写代码的时候，保持干爽的意思是:“不要重复自己”。如果您发现自己将代码片段复制到应用程序的多个部分，可以考虑制作一个:类、方法/函数、变量、常量等…

我们都理解上面的内容，但是当涉及到 Terraform 中跨多个资源的`count`条件语句时，我经常看到以下两种情况:

## 让我们处处重复自己

```
count = var.variableA == "" || var.variableB == "" ? 0 : 1
count = var.variableA == "" || var.variableB == "" ? 0 : 1
count = var.variableA == "" || var.variableB == "" ? 0 : 1
count = var.variableA == "" || var.variableB == "" ? 0 : 1
```

这正是我们试图用干来防止的事情。它是:

*   写起来很乏味
*   更难阅读
*   更难维护

当然，在这种情况下，重复并不是世界末日，但是让我们尽可能地提高效率。

## 2)选择一个切换特征的任意变量(当还需要其他变量时)

```
count = var.ssh_pub_key == "" ? 0 : 1
count = var.ssh_pub_key == "" ? 0 : 1
count = var.ssh_pub_key == "" ? 0 : 1
count = var.ssh_pub_key == "" ? 0 : 1
count = var.ssh_pub_key == "" ? 0 : 1
count = var.ssh_pub_key == "" ? 0 : 1
count = var.ssh_pub_key == "" ? 0 : 1
```

这种方法似乎比第一种更好，但可以说是更差。这种方法大大降低了代码的可读性。这些事情中的一个或者全部都会发生:

*   有人会定义单个变量，运行 terraform，它会出错，因为没有定义`variableB`。
*   有人会定义单变量，运行 terraform，有些资源会因为没有定义`variableC`而无法正确命名或配置。
*   有人必须阅读自述文件，并从那里获得他们需要的所有信息(假设有自述文件)。
*   有人将不得不仔细检查代码，并找出需要的其他变量。

由于显而易见的原因，上述所有场景都不太好。应该有人能够接管你的代码，并理解正在发生的事情。

有些人可能会说:

*   “您的代码有一些开销是合理的”
*   “一个人必须记住某些事情是合理的”

我会反驳:“有人建立和使用模式是合理的”。不过，以上并不是一种模式，这是一种记忆游戏，每次在代码库中使用这种方法，它都会变得更加复杂。

另外，当代码需要更新时，6 个月后会发生什么？你又看了一遍发现？碰到所有的绊脚石，然后你终于能够开始生产？

# 保持干燥

在这些情况下，怎样做才能在保持逻辑性和可读性的同时成为一个干衣机？

通常，当您在 Terraform 中跨多个资源使用`count`条件语句时，您试图做两件事之一:

1.  确保定义了所有必要的变量
2.  确保所有必要的变量都有正确的值。

这两种情况都可以在 Terraform 中使用局部变量轻松解决。

## 确保定义了所有必要的变量

在您想要检查所有先决条件变量都已定义的情况下，我们可以利用 Terraform 的`[contains](https://www.terraform.io/docs/configuration/functions/contains.html)` [函数](https://www.terraform.io/docs/configuration/functions/contains.html)。这个函数将遍历一个列表，检查是否有任何元素包含特定的值。

这是我们的使用案例中的情况:

```
locals {
  is_feature_enabled = contains([
    var.variableA,
    var.variableB,
    var.variableC
  ],
  "") ? 0 : 1
}
```

在这个例子中，我们寻找一个定义为`""`的空字符串。如果找到了空字符串，`contains`将返回`true`，变量的值被设置为`0`。如果函数返回`false`，没有变量是空的，我们可以继续。

## 确保所有必要的变量都有正确的值。

在这种情况下，我们所要做的就是将逻辑从`count` 语句移到一个局部变量中。

```
locals {
  is_feature_enabled =  var.variableA != "" && var.variableB == "randomValue" ? 1: 0
}
```

这看起来不如使用`contains`函数好，但至少它是在一个地方定义的，而不是在代码库中的多个地方。

一旦使用任何一种方法定义了局部变量，就可以在参考资料中多次引用它。

```
resource "aws_lambda_permission" "test" {
  count = local.is_feature_enabled == 1 ? 1 : 0
  ...
}resource "aws_lambda_function" "test" {
  count = local.is_feature_enabled == 1 ? 1 : 0
  ...
}resource "aws_cloudwatch_log_group" "test" {
  count = local.is_feature_enabled == 1 ? 1 : 0
  ...
}
```

## 额外的东西

我上面所有的例子和参考都使用了`count`原语。这个逻辑也适用于`for_each`。

```
for_each = local.is_feature_enabled == 1 && var.listA != [] ? var.listA : []
```

你会注意到，除了`local.is_feature_enabled`，我还会检查`var.listA`是否为空。这是为了演示在您拥有可选资源的情况下，可以添加额外的逻辑。

# 结论

我希望这篇文章展示了两种在陆地上保持干燥的方法。保持干燥可以提高可读性、可维护性和有效性。

*为了获得无限的故事，你也可以考虑* [*注册*](https://blog.rhel.solutions/membership) *成为中等会员，只需 5 美元。如果你使用* [*我的链接*](https://blog.rhel.solutions/membership) *注册，我会收到一小笔佣金(不需要你额外付费)。*