# 什么是 Eslint 规则？

> 原文：<https://blog.devgenius.io/what-are-eslint-rules-d2d99bf5387?source=collection_archive---------4----------------------->

## 在本文中，您将了解如何使用 eslint 在应用程序中强制编写正确的 Javascript 语法。

![](img/53228361f31c89e1dfdfb1159703505b.png)

Eslint 是一个识别和报告 Javascript/ECMAScript 代码中语法错误的工具。该工具非常有用，因为它有助于确保一致性并最大限度地减少不必要的错误。

Eslint 可以手动安装到项目中。如果你使用的是 Javascript 框架，大多数都是作为 **devDependency** 预打包在 eslint 中的。

# Eslint 的特点

## 1.规则:

由于有规则，Eslint 能够识别语法错误。规则是给定代码块必须遵循的限制。Eslint 有自己的一套规则，但是你可以为你的个人项目或组织项目创建自定义规则。自定义规则作为插件创建。要开始创建你自己的自定义规则，你可以使用[这个](https://eslint.org/docs/latest/developer-guide/working-with-rules)详细的指南。

## 2.`.eslintrc.*`文件:

为了能够配置新的/现有的 eslint 规则，您需要创建这个文件。可以是. js，。json 或者。yml 文件。

添加下面的代码行将打开所有标记为推荐的 eslint 规则。

```
{
 “extends”: “eslint:recommended”
}
```

# 如何在中配置规则。eslintrc？

在我们添加新规则之前，让我们看看下面的代码片段，了解如何打开/关闭 eslint 规则。

```
{
  “rules”: {
     “semi”: [“error”, “always”],
     “quotes”: [“error”, “double”]
  }
}
```

正如您在上面看到的，规则是在`rules`对象中定义的。要配置规则，您需要设置规则 ID，它可以定义为以下三个值中的任何一个；

*   `"off"`或`0`——这将关闭规则，因此它对您的代码没有影响
*   `"warn"`或`1` -这将打开规则作为警告，不影响退出代码。
*   `"error"`或`2` -这将打开规则，并使用退出代码 1 终止代码执行。

例如，在上面的代码中，semi 规则和 quotes 规则被作为错误打开。

这些规则也可以使用配置注释进行配置，例如

```
/** eslint semi: “error”, quotes: “off” */
```

# Eslint 规则示例

## 1.`no-console`

调试代码时，我们经常使用`console.log`。不幸的是，将控制台方法留在代码中是非常常见的。这不会对你的申请产生负面影响，然而，这被认为是“不专业的”。

`no-console`规则不允许使用控制台。

```
// .eslintrc.json
{
   “rules” : {
      “no-console”: “error”
   }
}// custom console
Console.log(“Hello world!”);
```

关于这个规则的更多信息，请查看这个资源。

## 2.`no-eq-null`

与 null 进行比较可能会产生不良的结果，比如错误的输出，任何不了解 Javascript 如何处理 null 值的人都可能会忽略这一点。

`no-eq-null`规则不允许没有类型检查操作符的空比较。

```
// .eslintrc.json
{
    “rules” : {
       “no-eq-null”: “error”
    }
}// your code
if (foo === null) {
     bar();
}while (qux !== null) {
     baz();
}
```

关于这条规则的更多信息，请查看[这个](https://eslint.org/docs/latest/rules/no-eq-null)资源。

> *感谢你读到最后。喜欢，留下评论，并与您的网络分享，如果你觉得这篇文章有帮助。*

# 阅读资源

1.  Eslint 文档 —该文档非常详细，对于初学者来说很容易理解。
2.  [eslint playground](https://eslint.org/play/) —如果你想在将 Eslint 规则添加到你的项目之前测试它们，这个在线工具是一个使用 Eslint 插件的好地方。

*原发布于*[*https://Belinda marionk . hash node . dev*](https://belindamarionk.hashnode.dev/preview/6343c7a90961ceed80d0a643)*。*