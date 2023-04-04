# 反应最佳实践—表达式

> 原文：<https://blog.devgenius.io/react-best-practices-expressions-6724d83de3f9?source=collection_archive---------26----------------------->

![](img/a4bb7f07fdf26a85cfadca02e0b210a3.png)

安妮·斯普拉特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

像任何种类的应用程序一样，React 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 React 应用程序时的一些最佳实践。

# 等号两边的空格

等号周围应该有空格。

例如，我们应该有:

```
<Hello sameName={firstname === lastName} />;
```

现在我们可以很容易地看到我们比较的表情。

# 可以有 JSX 的文件的文件扩展名

我们应该在`.js`或`.jsx`文件中有 JSX 代码。

这样，我们知道它们是我们 React 项目的一部分。

# 第一个属性的位置

如果第一个属性在一行中，它可以与组件或元素名称在同一行中，

否则，我们可以把它们放到下一行。

例如，我们可以写:

```
<Hello personal={true} />
```

或者:

```
<Hello 
  personal={true}
  foo="bar"
/>
```

它们都易于阅读，不会横向溢出页面。

# 反应碎片

我们可以使用 React Fragements 使我们的生活变得更容易，因为它让我们有一个包装器，而不是呈现任何元素或组件。

我们可以写:

```
<React.Fragment><Foo /></React.Fragment>
```

或者:

```
<><Foo /></>
```

简称。

但是，如果我们需要将一个道具传递给一个片段，那么我们必须使用`React.Fragment`:

```
<React.Fragment key="key"><Foo /></React.Fragment>
```

# 事件处理程序命名约定

我们的事件处理程序应该有一些命名约定，这样我们可以更容易地找到它们。

例如，我们可能坚持使用像`handle`或`on`这样的前缀:

```
<MyComponent handleChange={handleChange} />
```

或者:

```
<MyComponent onChange={onChange} />
```

这样，我们知道任何带有这些前缀的都是事件处理程序。

# JSX 压痕

2 个空格是缩进的理想长度。当我们仍然可以看到缩进的时候，它最小化了输入。

选项卡在不同平台上呈现时会有问题，所以最好不要使用它。

但是，我们可以将空格转换成制表符。

例如，我们可以写:

```
<App>
  <Hello />
</App>
```

我们也可以在文本编辑器中将制表符转换成两个空格。

如果我们这样做，我们可以使用标签。

# 支柱缩进

如果每行有一个道具，那么道具应该缩进。

同样，出于同样的原因，两个空间也是理想的。

例如，我们应该写:

```
<Hello
  firstName="John"
/>
```

现在我们可以很容易地看到我们的代码。

# 总是在列表中包含关键道具

每当我们在列表中呈现一个数组时，我们应该添加`key` prop。

这样，React 可以正确地跟踪。

我们应该确保我们传递的是一个独特的值。

例如，我们可以写:

```
data.map(item => <Hello key={item.id}>{item.name}</Hello>);
```

这样，我们就不会从 React 得到错误，也不会在操作列表时出现意外的行为。

# JSX 码的最大深度

嵌套代码很难阅读，所以我们应该尽量减少它们。

例如，我们应该写:

```
<Foo>
  <Bar />
</Foo>
```

但不是:

```
<App>
  <Foo>
    <Bar>
      <Baz />
    </Bar>
  </Foo>
</App>
```

只是嵌套太多了。

大概 3 层以上的嵌套对我们的眼睛来说太多了。

# 单行的最大道具数

我们不应该在一行有太多的道具，因为它会溢出页面。

这样，我们在阅读代码时就不用水平滚动了。

例如，我们可以每行使用一个或两个道具:

```
<Hello lastName="Smith" firstName="John" />;
```

或:

```
<Hello 
  lastName="Smith" 
  firstName="John" 
/>;
```

这样，我们就不用滚动了。

# 在 JSX 道具中使用箭头功能

如果我们不需要在传递的函数中引用`this`作为合适的值，那么我们应该使用箭头函数。

它们更短，没有自己的`this`价值。

例如，我们应该写道:

```
<Foo onClick={() => console.log('Hello!')}></Foo>
```

如果需要指定`this`的值，需要使用`bind`和传统函数。

所以我们必须写下:

```
<Foo onClick={function() { console.log(this.foo) }.bind(this)}></Foo>
```

![](img/65f303cc60cac624f7cc05fd5334f55f.png)

照片由 [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们应该减少代码的长度，这样我们就不用水平滚动了。

此外，我们不应该有太多的嵌套，因为它很难阅读。

如果不需要通过`bind`设置`this`的值，可以使用箭头功能。

我们可以使用片段来包装元素，但不想渲染任何东西。