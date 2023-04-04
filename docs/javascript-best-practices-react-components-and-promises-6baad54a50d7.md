# JavaScript 最佳实践—反应组件和承诺

> 原文：<https://blog.devgenius.io/javascript-best-practices-react-components-and-promises-6baad54a50d7?source=collection_archive---------3----------------------->

![](img/42c0934a2d4729843a68840e4b3602e5.png)

照片由[波格丹一世·托多兰](https://unsplash.com/@todoranb_26?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 读取承诺值时，首选`await`而非`then()`

`async`和`await`比回调模式短，所以我们可以转而起诉那个。

例如，不写:

```
myPromise
  .then((val) => {
    return foo();
  })
  .then((val) => {
    return bar();
  })
  .then((val) => {
    return val;
  })
```

我们写道:

```
const foo = async () => {
  const val1 = await myPromise;
  const val2 = await foo();
  const val3 = await bar();
  return va3;
}
```

现在我们没有更多的回调。

# 确保将适当数量的参数传递给 Promise 函数

我们应该将适当的参数传递给 promise 方法。

`Promise.all`被称为满口答应。

与`Promise.race`相同。

用解析的值调用`Promise.resolve`。

`Promise.reject`以拒绝的理由被调用。

`then`被称为成功和可选地和失败回调。

`catch`用回调调用。

`finally`与`catch`相同。

# 对象文字属性名的引号

如果一个属性名不是一个有效的标识符，那么我们需要用引号括起来。

例如，我们写道:

```
const obj = {
  "foo bar": true
};
```

否则，我们不需要引号:

```
const obj = {
  foo: true
};
```

# 一致使用反斜线、双引号或单引号

如果我们选择一个在任何地方都使用的字符串分隔符，它将是反勾号。

无论是否需要插值表达式，我们都可以使用模板字符串。

例如，我们可以写:

```
const backtick = `backtick`;
```

或者:

```
const greet = `hello ${name}`;
```

我们甚至可以用它们编写多行字符串:

```
const backtick = `foo
bar`;
```

如果字符串中有一个换行符，那么它将被这样显示。

# 需要基数参数

我们应该在`parseInt`方法调用中输入基数参数，以确保我们用正确的基数解析数字。

例如，以下内容:

```
const num = parseInt("091");
```

可以解释为八进制数。

相反，我们应该写:

```
const num = parseInt("091", 10);
```

将其转换为十进制数。

# 对布尔属性实施一致的命名

在 React 中，我们应该确保布尔属性有一致的命名方案，以减少混淆。

例如，不写:

```
import PropTypes from 'prop-types';class Heading extends React.Component {
  render() {
    return (
      <h1>{this.props.enabled}</h1>
    );
  }
}Heading.propTypes = {
  enabled: PropTypes.boolean
};
```

我们写道:

```
import PropTypes from 'prop-types';class Heading extends React.Component {
  render() {
    return (
      <h1>{this.props.enabled}</h1>
    );
  }
}Heading.propTypes = {
  isEnabled: PropTypes.boolean
};
```

前缀`is`很容易告诉我们这是一个布尔属性。

# 不要使用没有显式 T `ype`属性的`button`元素

我们不应该使用没有显式类型属性的按钮元素。

否则，我们可能会得到意想不到的结果。

例如，不写:

```
const Foo = <button>Foo</button>
```

我们写道:

```
const Foo = <button type='button'>Foo</button>
```

# 默认属性应该有相应的非必需属性类型

对于非必需的属性类型，我们应该有一个默认的属性。

这样的话，道具永远都不是`undefined`。

例如，代替写作:

```
function FooBar({ foo, bar }) {
  return <div>{foo}{bar}</div>;
}FooBar.propTypes = {
  foo: React.PropTypes.string.isRequired,
  bar: React.PropTypes.string
};FooBar.defaultProps = {
  foo: "foo"
};
```

我们写道:

```
function FooBar({ foo, bar }) {
  return <div>{foo}{bar}</div>;
}FooBar.propTypes = {
  foo: React.PropTypes.string.isRequired,
  bar: React.PropTypes.string
};FooBar.defaultProps = {
  bar: "bar"
};
```

# 在属性、状态和上下文的赋值过程中，强制使用一致的析构

我们应该使用属性、状态和上下文一致的析构赋值。

例如，我们应该写:

```
const IdComponent = ({id}) => {
  return (<div id={id} />)
};
```

或者:

```
const Foo = class extends React.PureComponent {
  render() {
    const { name } = this.state;
    return <div>{name}</div>;
  }
};
```

# 在 React 组件定义中添加显示名称

我们应该设置`displayNamne`属性，这样当我们用 React dev 工具调试时就可以看到这个名称。

例如，我们写道:

```
export default function Greet({ name }) {
  return <div>hello {name}</div>;
}
Greet.displayName = 'Greet';
```

![](img/96f7b4c2263115e91ecfcf96b90e91bc.png)

[杰克·布林德](https://unsplash.com/@brindo_?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 结论

我们一定要确保正确定义和使用承诺。

此外，当我们创建 React 组件时，我们应该为可选组件提供一些属性验证和默认属性。

另外，`displayName`对于调试很有用。