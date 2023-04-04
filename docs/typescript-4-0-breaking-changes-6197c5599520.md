# TypeScript 4.0 —突破性更改

> 原文：<https://blog.devgenius.io/typescript-4-0-breaking-changes-6197c5599520?source=collection_archive---------1----------------------->

![](img/9cbc301caeafc9401de78e167f67a6b3.png)

哈维尔·阿莱格·巴罗斯[在](https://unsplash.com/@soymeraki?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

TypeScript 4.0 提供了许多新特性，使得 JavaScript 开发更加容易。

在本文中，我们将研究 TypeScript 4 的最佳特性。

# 定制 JSX 工厂

TypeScript 4.0 允许我们用`jsxFragmentFactory`选项定制片段工厂。

我们可以通过编写以下内容来设置`tsconfig.json`中的设置:

```
{
  compilerOptions: {
    target: "esnext",
    module: "commonjs",
    jsx: "react",
    jsxFactory: "h",
    jsxFragmentFactory: "Fragment",
  },
}
```

此外，我们可以基于每个文件设置工厂:

```
/** @jsx h */
/** @jsxFrag Fragment */
```

`jsxFactory`是将 JSX 渲染成 JavaScript 的函数。

并且`jsxFragmentFactory`让我们将 JSX 片段呈现给 JavaScript。

# 使用`--noEmitOnError`提高`build`模式下的速度

`noEmitError`选项允许我们通过在`.tsbuildinfo`文件中缓存数据来进行增量编译。

这将给我们一个使用`--incremental`旗帜建造时的性能。

# `--incremental`同`--noEmit`

`--noEmit`标志现在可以和`--incremental`标志一起使用来加速增量构建。

# 编辑器改进

有了 TypeScript 4.0，Visual Studio 代码和 Visual Studio 变得更加智能。

它能做的一件事是将长串未定义的检查收缩到可选的链接或 nullish coaslescing 表达式中。

例如，如果可以收缩:

```
a && a.b && a.b.c
```

变成:

```
a?.b?.c
```

它还增加了对`/** @deprecated */`标志的支持，以将代码标记为不推荐使用。

那么代码将显示为被删除。

# 启动时的部分语义模式

部分语义模式通过在启动期间部分解析代码而不是解析所有内容来加快 Visual Studio 代码和 Visual Studio 的启动时间。

因此，现在启动延迟应该只有几秒钟。

这意味着我们将立即获得编辑的丰富经验。

# 更智能的自动导入

自动导入现在更智能了，因为它可以处理自带类型的包。

TypeScript 4.0 可以在`package.json`中列出的包中搜索类型，这样它就可以找到类型，并让我们在这些包中进行自动导入。

我们将`typescript.preferences.includePackageJsonAutoImports`设置为`true`，让我们进行自动导入。

# 属性重写访问器(反之亦然)是错误的

在 TypeScript 4.0 中，重写访问器的属性是一个错误，反之亦然。

例如，如果我们有:

```
class Base {
  get foo() {
    return 100;
  }
  set foo(value) {
    // ...
  }
}class Derived extends Base {
  foo = 10;
}
```

然后我们会得到一个错误，因为我们将`this.foo`设置为一个新值。

# `delete`的操作数必须是可选的

`delete`的操作数必须是可选的，因为它们可以被删除。

所以如果我们有这样的东西:

```
interface Thing {
  prop: string;
}function f(x: Thing) {
  delete x.prop;
}
```

然后我们得到一个错误，因为`Thing`需要`props`。

# 结论

TypeScript 4.0 带来了一些突破性的变化。

此外，它还有更多设置 JSX 工厂和自动转换语法的功能。