# 反应最佳实践—节点

> 原文：<https://blog.devgenius.io/react-best-practices-nodes-ce60fef1366e?source=collection_archive---------25----------------------->

![](img/394280b514f97f938809c75f2d1f277b.png)

照片由[欧文·马丁内斯](https://unsplash.com/@idmg_photography?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

像任何种类的应用程序一样，React 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 React 应用程序时的一些最佳实践。

# 文本节点中没有注释

如果我们需要添加一个评论来反应，我们不能直接把它直接放在一个文本节点中。

相反，我们需要用花括号将它们括起来。

例如，我们写道:

```
{/* empty div */}
```

添加评论。

这只是一个普通的 JavaScript 注释。

# JSX 没有重复的道具

我们不应该在 JSX 代码中使用重复的属性，因为它们会导致意想不到的行为。

例如，我们不应该:

```
<Hello name="james" name="smith" />;
```

相反，我们写道:

```
<Hello firstname="james" lastname="smith" />;
```

# JSX 中没有字符串文字

在 JSX 我们不需要字符串文字。

他们是多余的。

例如，以下内容:

```
const Hello = <div>test</div>;
```

与以下内容相同:

```
const Hello = <div>{'test'}</div>;
```

所以我们可以用第一个没有花括号的例子。

# React 代码中没有脚本 URL

我们不应该在 URL 中有任何以`javascript:`开头的内容。

如果需要，我们可以用道具代替。

例如，我们不应该写:

```
<a href="javascript:"></a>
```

相反，我们写道:

```
<a href="http://example.com"></a>
```

# 没有 target='_blank '的不安全用法

我们不应该让`rel='noreferrer'`属性与`target='_blank'`属性搭配。

这是一个很大的安全漏洞，因为我们没有`rel`属性。

例如，我们应该写:

```
<a target="_blank" rel="noreferrer" href="http://example.com"></a>
```

现在，攻击者攻击我们应用的方式少了一种。

# JSX 没有未声明的变量

未声明的变量在 JSX 是无用的，所以我们不应该有它们。

它们也会导致错误

例如，如果我们有:

```
<Hello name="John" />;
```

那没用，那我们应该把它们去掉。

它们可能是错误，我们不应该在代码中出现它们。

# 没有无用的碎片

我们的代码中不应该有无用的片段。

只有当我们需要包装多个子元素时，我们才需要它们。

例如，以下片段的使用是多余的:

```
<>{foo}</>

<><Foo /></><></>
```

相反，当我们有多个孩子需要照顾时，我们会使用它们:

```
<>
  <div />
  <div />
</>
```

# 每行一个 JSX 元素

我们应该每行都有 JSX 元素，这样我们可以更容易地阅读它们。

例如，下面的内容看起来很狭窄:

```
<App><Hello /></App>
```

但是，下面的看起来好多了:

```
<App>
  <Hello />
</App>
```

所以我们应该把它们放在多行上。

# 将 PascalCase 用于用户定义的 JSX 组件

对于用户定义的 JSX 组件，我们应该使用 PascalCase。

这样，我们可以将它们与 HTML 元素区分开来。

使用 PascalCase 也是一个 React 约定，所以我们应该遵循它以保持一致性。

例如，我们应该写:

```
<FooComponent />
```

# JSX 道具之间没有多个空格

我们不应该在 JSX 道具之间添加多个空格。

一个以上就太多了。

所以与其写:

```
<App  space />
```

我们写道:

```
<App space />
```

# JSX 道具传播

道具的散布使得它们更难阅读。所以在使用 spread 运算符之前，我们应该三思而行。

如果我们能明确地把它们写出来，最好用那个。

例如，不写:

```
<App {...props} />
```

我们写道:

```
<img src={src} alt={alt} />
```

现在我们知道了传递给 img 元素的显式值。

# 默认属性声明按字母顺序排序

如果我们有很多默认道具，我们可能希望按字母顺序对它们进行排序。

例如，不要写:

```
Component.defaultProps = {
  z: "z",
  a: "a",
  b: "b"
};
```

我们写道:

```
Component.defaultProps = {
  a: "a",
  b: "b",
  z: "z"
};
```

这样，我们可以更容易地找到他们。

# 道具字母排序

像默认道具一样，我们可能想按字母顺序排列我们的道具名，这样我们可以很容易地找到它们。

例如，如果我们有:

```
<Hello lastName="Smith" firstName="John" />;
```

那么我们可以改为写:

```
<Hello firstName="John" lastName="Smith" />;
```

如果它们是按字母顺序排列的，那么我们就更容易找到它们。

# 右括号的间距

我们应该在右括号前留有空格。

这样，我们可以更容易地看到它们。

例如，不要写:

```
<Hello/>
```

我们写道:

```
<Hello />
```

![](img/c562d879a71af850feec79003205a892.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Terrah Holly](https://unsplash.com/@tlholly?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

我们应该确保代码中有良好的间距。

此外，在我们的 React 组件中应该有多余的或导致错误的代码。