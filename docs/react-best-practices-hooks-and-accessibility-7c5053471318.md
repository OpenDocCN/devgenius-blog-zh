# 反应最佳实践—挂钩和可访问性

> 原文：<https://blog.devgenius.io/react-best-practices-hooks-and-accessibility-7c5053471318?source=collection_archive---------26----------------------->

![](img/d8c42591882df2e943fc7b33380fbfc0.png)

照片由[马腾·德克斯](https://unsplash.com/@maartendeckers?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，React 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 React 应用程序时的一些最佳实践。

# JSX 左括号和右括号周围的空格

在我们的 JSX 代码中，我们应该在左括号和右括号之间保持一致的间距。

例如，以下内容没有合适的间距:

```
<App/ >
<input/
>
<Foo>< /Foo>
```

斜线前没有空格，斜线后有一个空格，不是八。

同样，`input`在斜杠前没有空格，在斜杠后有换行符。

`Foo`在结束标签的斜杠前有一个空格字符，这也不好。

相反，我们应该写:

```
<Hello />
<Hello firstName="james" />
```

# 只有当我们有 JSX 代码时，引用才会起作用

如果我们的文件中没有任何 React 组件，那么我们不需要导入 React。

例如，我们不应该写:

```
const React = require('react');

// nothing to do with React
```

相反，我们应该写:

```
const React = require('react');

const Hello = <div>Hello {this.props.name}</div>;
```

# 使用导入的组件

我们应该使用我们进口的部件。

例如，我们应该写:

```
const Hello = require('./Hello');

<Hello name="james" />;
```

# JSX 表达式缺少括号

如果我们有多行 JSX 表达式，我们应该用括号把它括起来。

例如，我们应该写:

```
return (
  <div>
    <p>Hello {name}</p>
  </div>
);
```

这样，我们返回 JSX 表达式，并得到我们期望的结果。

# 向 React 组件添加显示名称

为了使调试更容易，我们应该添加`displayName`道具，这样我们就可以用 React dev 工具更容易地调试我们的应用程序。

例如，我们写道:

```
export default function Hello({ name }) {
  return <div>Hello {name}</div>;
}
Hello.displayName = 'Hello';
```

现在我们可以在 React dev 工具中看到`'Hello'`这个名字。

# 丢失关键道具

如果我们要呈现一个列表，那么我们应该给列表项添加一个`key`道具。

该值应该是唯一的 ID，以便 React 可以跟踪它。

数组索引不好，因为它们可能会随着列表的改变而改变。

因此，我们应该写:

```
data.map((x) => <Hello key={x.id}>{x.name}</Hello>);
```

# 文本节点中没有注释

如果我们想写注释，我们应该用花括号把它们括起来。

否则，它们将被呈现为文本。

例如，我们应该写:

```
<div>{/* empty div */}</div>;
```

# 在 JSX 没有重复的房产

我们在 JSX 不应该有重复的道具。

这是因为如果我们拥有它们，我们会得到意想不到的结果。

例如，我们应该写:

```
<Hello firstname="james" lastname="smith" />;
```

而不是:

```
<Hello name="james" name="smith" />;
```

# 只调用顶层的钩子

我们应该只调用顶层的钩子。

它们不能在循环或其他块中使用。

这样做是为了保持钩子调用的顺序。

# 仅从 React 函数调用挂钩

钩子不应该从普通的 JavaScript 函数中调用。

它们应该只能从 React 函数组件和其他自定义挂钩中调用。

# 无障碍表情符号

我们应该使用易理解的表情符号。

这样屏幕阅读器可以阅读它们。

为了使它们易于访问，首先，我们将它们包装在一个 span 元素中。

为了使表情符号易于理解，我们应该使用`aria-label`属性来解释我们使用了什么表情符号。

此外，我们应该给它`role='img'`，这样屏幕阅读器就可以把它读给用户听。

# 替代文本

我们应该在我们的 img 元素中有替代文本。

这样大家就知道是什么了。

同样，对于区域和对象标签，我们也应该有同样的东西。

如果我们用类型 image 输入，我们应该添加一个 alt 属性。

# 主播要有内容

如果我们使用`a`元素，我们应该有内容在里面。

这样，我们都可以使用锚元素。

例如，我们写道:

```
<a>Content</a>
```

或者:

```
<a><TextWrapper /><a>
```

# 添加 aria-activedescendant 和 tabindex 属性

`aria-activedescendant`属性保留活动文档焦点。

它通过将元素的 ID 分配给`aria-activedescendant`的值来指示哪个子元素具有次要焦点。

带有`aria-activedescendant`的元素必须是可制表的，因此`tabIndex`也必须包含在内。

例如，我们写道:

```
<div aria-activedescendant={id} tabIndex={0} />
<div aria-activedescendant={id} tabIndex="0" />
<div aria-activedescendant={id} tabIndex={1} />
```

![](img/9bbfafcf8c306a5dda9c476dd22d776a.png)

由 [Olia Gozha](https://unsplash.com/@olia?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

为了正确使用钩子，我们应该遵循一些规则。

此外，为了使我们的应用程序易于访问，我们需要在组件中添加一些额外的属性。